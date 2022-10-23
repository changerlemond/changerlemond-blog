---
title: "toString에도 종류가 있다는 걸 아세요? : ToStringBuilder 사용기"
date: 2022-04-24T22:38:00+09:00
tags: ['java']
draft: false
---
toString 파헤치기 (feat. ToStringBuilder)
<!--more--> 

저는 항상 @toString 어노테이션을 잘 써왔습니다. 
lombok에서 제공하고 있는 아주 편한 어노테이션이죠. 
그럼 toString은 lombok을 써야만 볼 수 있을까요? 그렇지 않습니다. 
그럼 toString이 하나가 아니네요? 네, 하나가 아니었어요. 
저도 이번 기회에 조금은 알아가게 되면서 글을 적어보게 됐습니다.



1. toString - Object 클래스 안의 메서드

Object의 문자정보를 "class(이름)@16진수"의 해시 코드로 보여줄 수 있는 기능을 가지고 있습니다.

Object 객체에 구현된 내용을 보고 이해가 더 쉽게 되었습니다.

    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

Object는 최상위 클래스이기 때문에 다른 자료형에서도 이 toString을 사용할 수 있습니다. 
해시 코드는 해시 함수에 의해 자동으로 생성된 값이기 때문에 객체를 유일하게 식별할 수 있는 값이기도 합니다. 
보통 이 toString 함수를 @Override하여 재정의하여 사용하게 됩니다. 
다른 자료형에서도 이런 구현 방식을 찾아볼 수 있었습니다.

ObjectId

    @Override
    public String toString() {
        return toHexString();
    }


Arrays

    /**
     * Returns a string representation of the contents of the specified array.
     * If the array contains other arrays as elements, they are converted to
     * strings by the {@link Object#toString} method inherited from
     * {@code Object}, which describes their <i>identities</i> rather than
     * their contents.
     *
     * <p>The value returned by this method is equal to the value that would
     * be returned by {@code Arrays.asList(a).toString()}, unless {@code a}
     * is {@code null}, in which case {@code "null"} is returned.
     *
     * @param a the array whose string representation to return
     * @return a string representation of {@code a}
     * @see #deepToString(Object[])
     * @since 1.5
     */
    public static String toString(Object[] a) {
        if (a == null)
            return "null";

        int iMax = a.length - 1;
        if (iMax == -1)
            return "[]";

        StringBuilder b = new StringBuilder();
        b.append('[');
        for (int i = 0; ; i++) {
            b.append(String.valueOf(a[i]));
            if (i == iMax)
                return b.append(']').toString();
            b.append(", ");
        }
    }


그럼 toString()을 매번 재정의해서 사용해줘야 할까요? 생각보다 귀찮은 작업입니다.
특히 많이 쓰이는 Json 형태의 String으로 변환해주려면 직접 수기로 쓰는 방법밖에 없을까요?
인텔리제이에서 이 부분을 보고 더 찾아보게 됐습니다.

<div style="text-align:center">
    <img src="/images/java/toString.png" alt="documentdb-start" />
</div>

String concat과 StringBuffer, StringBuilder는 워낙 많이 사용하는 방식이니 이 글에서 설명하지는 않으려 합니다. 
다만 여기서 처음 본 것은 ToStringBuilder였습니다. 
찾아보니 Apache commons-lang에서 제공하고 있더라고요. 
저는 Object를 Json 형태의 String으로 저장하고 싶었는데, ToStringBuilder에 대해 내용을 보니 적합할 것이라는 판단이 들었습니다.



gradle 환경이어서 아래와 같이 build.gradle에 선언해주었습니다.

    dependencies {
            implementation 'org.apache.commons:commons-lang3:3.12.0'
    }


그리고 내가 사용할 클래스에 .toString() 메서드를 이렇게만 정의해줘서 사용하면 됩니다.

@Override
public String toString() {
return ToStringBuilder.reflectionToString(this, ToStringStyle.JSON_STYLE);
}
이렇게 하면 Json 형태로 Object의 내용을 String 형태로 편리하게 뽑아낼 수 있었습니다.

여기서 사용한 ToStringStyle에는 Json 말고도 여러 형태를 제공하고 있어서 본인이 원하는 스타일로 지정해 문자열을 출력할 수 있습니다. 
만약 원하는 형태가 없다면 CustomStyle을 만들 수도 있습니다. (ToStringStyle 상속)

```
ToStringStyle.DEFAULT_STYLE
ToStringStyle.MULTI_LINE_STYLE
ToStringStyle.NO_FIELD_NAMES_STYLE
ToStringStyle.SHORT_PREFIX_STYLE
ToStringStyle.SIMPLE_STYLE
ToStringStyle.NO_CLASS_NAME_STYLE
ToStringStyle.JSON_STYLE
```
여기서 사용한 reflectionToString은 필드명과 값을 일일이 append 해주지 않아도 static method 형태이기 때문에 편리하게 사용할 수 있었습니다. 
ToStringBuilder에 직접 append를 하는 것도 가능합니다. (필드명과 값)

ToStringBuilder, 내부적인 코드를 보니 이렇게 동작하고 있었습니다.

```
public ToStringBuilder append(final String fieldName, final boolean value) {
style.append(buffer, fieldName, value);
return this;
}
```

내부적으로 StringBuffer를 이용해 값을 저장하고 있는 것을 볼 수 있습니다.

처음 사용해본 후기로는 ReflectionToString을 사용하는 것이 가장 편리하게 구현할 수 있던 장점이었습니다. 
저는 Json 형태였기 때문에 제공하는 Style을 사용할 수 있어서 이점 또한 좋았습니다. 
만약 원하는 형태가 따로 있다면 CustomStyle도 할 수 있기 때문에 유용하게 사용할 수 있을 것 같습니다. 
우리 모두 반복 작업을 벗어나 봐요!