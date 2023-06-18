---
title: "이미지 리사이징하기"
date: 2023-06-18T21:31:30+09:00
draft: false
tags: ['kotlin', 'image']
---

Scalr 라이브러리를 사용하여 이미지를 리사이징하기
<!--more--> 

<br>

원본 이미지만을 사용해서 이미지 로드를 하고 있었는데, 이미지를 생각보다 사용하는 곳이 많았다.
현재 유저에게 보여주는 프로필 사이즈는 굉장히 작았기 때문에 더 최적화할 필요성이 있었고, 그래서 이미지 리사이징에 대해 알아보게 되었다.

<br>

### 이미지 리사이징은 왜 할까?
이미지 리사이징을 하게 되면 이미지를 더 빠르게 로드할 수 있고, 더 적은 용량으로 저장할 수 있다.
네트워크 전송 비용이 굉장히 많이 드는 부분이기 때문에 이미지 리사이징을 통해 최적화가 가능하다.

<br>

### 라이브러리 선택하기
Scalr 라이브러리로 선택하게 되었다. 여러 이미지 파일 형태를 리사이징 할 수 있도록 지원하고 있었다.
자바에 내장된 AWT 라이브러리도 고려사항 중 하나였지만, 조금 더 이미지에 초점이 맞춰져 있으면서 높은 화질로 리사이징을 할 수 있는 Scalr 라이브러리를 선택하게 되었다.

<br>

### Scalr 라이브러리 사용하기
먼저 라이브러리를 설정해준다. 나의 경우 gradle을 사용하고 있기 때문에 build.gradle에 아래와 같이 추가해주었다.
```groovy
dependencies {
    implementation 'org.imgscalr:imgscalr-lib:4.2'
}
```
이렇게 설정이 끝나면 이미지 리사이징을 할 수 있다. 코드로 간단히 보면 아래와 같다.
```kotlin
fun imageResize(width: Int, height: Int, image: BufferedImage): BufferedImage {
    val output = Scalr.resize(image, Scalr.Method.ULTRA_QUALITY, width, height)
    ImageIO.write(output, "jpg", File("output.jpg"))

    val byteArray = outputStream.toByteArray()
    val objectMetadata = ObjectMetadata()
    objectMetadata.contentLength = byteArray.size.toLong()

    ByteArrayInputStream(byteArray).use { resizeInput ->
        val resizedUpload: Upload = s3TransferManager.upload(
            bucketName, filePath, resizeInput, objectMetadata
        )
        resizedUpload.waitForCompletion()
    }
}
```
Scalr는 리사이징 시 Method를 지정할 수 있는데, Method에 따라 리사이징 속도와 화질이 달라진다.
나는 최대한 화질을 유지하기 위해서 ULTRA_QUALITY를 사용하였다.

그리고 원하는 가로, 세로 사이즈를 받아서 리사이징을 할 수 있다.

<br>

### 이미 저장되어 있는 이미지 리사이징하기
이미지들은 S3에 저장하고 있었는데, 이 파일들도 리사이징이 필요해서 Spring batch를 사용해서 리사이징을 하였다.
대략의 코드로 보자면 이렇다.
```kotlin
@Component
class ImageResizeTasklet(awsConfig: AwsConfig) {

    private val s3Client: AmazonS3 = awsConfig.s3Client()

    override fun execute(): RepeatStatus {

        val jobParameters = chunkContext.stepContext.jobParameters
        val imageCategory = jobParameters["bucketName"] as String

        imageResizeBatch(bucketName, width, height)

        return RepeatStatus.FINISHED
    }
    
    private fun imageResizeBatch(bucketName: String, width: Int, height: Int) {
        val listObjectsV2Request = ListObjectsV2Request().withBucketName(bucketName)
        var counter = 0
        val maxIterations = 10
        var token: String?
        
        do {
            if (counter++ >= maxIterations) {
                logger.error("Max iterations reached, breaking the loop to avoid potential infinite loop.")
                break
            }
            
            val listObjectsV2Result = s3Client.listObjectsV2(listObjectsV2Request)
            
            for (s3ObjectSummary in listObjectsV2Result.objectSummaries) {
                val key = s3ObjectSummary.key
                val s3Object = s3Client.getObject(bucketName, key)
                val image = ImageIO.read(s3Object.objectContent)
                val resizedImage = imageResize(width, height, image)
            }

            token = objectList.nextContinuationToken
            request = request.withContinuationToken(token)
        } while (token != null)
    }

}
```
여기서 s3 파일을 가져올 때는 listObjects를 사용할 수도 있는데, 이는 1,000개까지만 가져올 수 있다.
그래서 버킷에 1,000개가 넘을 수도 있기 때문에 listObjectsV2를 사용하였고, token을 사용해서 다음 1,000개를 가져오도록 하였다.
다만 maxIterations를 사용해서 무한 루프를 방지하였다. 이 때 s3의 총 갯수에 따라 maxIterations를 조절하면 된다.

그리고 위 코드는 테스트 코드라서 제외했지만, 버킷 경로에 따라 s3 파일을 찾기 위해서는 withPrefix를 사용하면 된다.

<br>

### 이미지 리사이징 시 주의할 점
이미지 리사이징 시 가장 주의해야 할 점은 **이미지 비율**이다. <U>이미지 비율을 유지하지 않으면 원본 이미지가 깨지거나 흐릿하게 나올 수 있다.</U>

이 때 **기준을 정하는 것이 중요**하다. 이미지의 가로와 세로는 어떤 길이일지는 알 수가 없다.
굉장히 가로가 길수도, 세로가 길수도 있기 때문에 이미지의 비율을 잘 파악해서 리사이징을 해야한다.

나의 경우 기준을 가로가 길다면 이미지 중앙에서 이미지 리사이징으로 원하는 사이즈만큼 잘라내고, 세로가 길다면 이미지의 윗부분에서 잘라내는 방식을 사용하였다.
이미지가 일부분 잘리게 되지만, 작은 프로필 이미지로 사용하고 있기 때문에 이러한 기준을 정한 것인데 리사이징의 목적에 따라 정할 필요가 있다.

그리고 비율과 더불어 중요한 점은 리사이징의 크기이다. 리사이징 하고자 하는 이미지의 크기가 작다면 리사이징 시 사이즈가 굉장히 많이 줄어들어야 할 것이다.
해상도가 심하게 줄어들면서 이미지의 품질이 손상되는 것은 어쩔 수 없이 발생하는 부분일 것이다.
그래서 이를 최소하기 위해서는 이미지 리사이징을 여러번 하는 것이 좋다. 

예를 들어 1,000 * 1,000 이미지를 100 * 100으로 리사이징한다고 가정하자. 이 때 1,000 * 1,000 이미지를 500 * 500으로 리사이징하고, 500 * 500 이미지를 100 * 100으로 리사이징하는 것이다.
나의 경우는 0.8의 비율로 원하는 가로와 세로의 사이즈가 될 때까지 리사이징을 하였다. 이렇게 하면 이미지의 품질이 손상되는 것을 최소화할 수 있다.

<br>

### 이미지 리사이징을 마무리하며
이미지 리사이징은 굉장히 많은 부분을 고려해야 하는 작업이다. 이미지의 비율, 이미지의 크기, 이미지의 품질 등등 많은 부분을 고려해야 한다.
그래서 이미지 리사이징을 할 때는 이러한 부분을 고려해서 리사이징을 해야한다.

이 작업을 진행하고 나서 실제로 로드를 하는 것을 보니 이미지가 깨지거나 흐릿하게 나오는 부분이 없어서 굉장히 만족스러웠다.
그리고 무엇보다 이미지 속도가 빨라져서 이미지 리사이징에 대한 효과를 더 체감할 수 있었다.

