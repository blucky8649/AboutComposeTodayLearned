![](https://media.vlpt.us/images/blucky8649/post/de406d96-a9cd-42d3-91ab-59baa0979f20/%EC%A0%9C%EB%AA%A9%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94_-001%20(3).png)

#### ![](https://images.velog.io/images/blucky8649/post/7f2e3ac5-7b52-445b-a152-fcdad390073a/image.png)
안녕하세요 이번에는 위 사진 처럼 텍스트 스타일링을 해볼거에요!

## 폰트 다운 받기
https://fonts.google.com/ 에 들어가셔서 원하시는 폰트를 다운받아주세요!

그 다음, res 폴더에 font 리소스 폴더를 만들고 다운 받은 font family를 넣어주세요.

붙여 넣으실때 **네이밍 컨벤션**을 준수하셔야 합니다. (`소문자`, `-` 대신 `_` 사용)

#### ![](https://images.velog.io/images/blucky8649/post/de693fac-9979-4aec-abe8-ee652aa4e2ef/image.png)


다음, `onCreate`안에 다음과 같이 폰트를 정의해줍니다.
```kotlin
<생략..>
super.onCreate(savedInstanceState)
val fontFamily = FontFamily(
    Font(R.font.roboto_thin, FontWeight.Thin),
    Font(R.font.roboto_black, FontWeight.Black),
    Font(R.font.roboto_bold, FontWeight.Bold),
    Font(R.font.roboto_medium, FontWeight.Medium)
)
<생략..>
```

## Text 꾸미기

일단, 텍스트를 채워넣을 검은 배경의 `Box`를 선언해주고, "Jetpack Compose"라는 텍스트를 입력해봅시다.  

```kotlin
setContent {
    Box(
        Modifier
            .fillMaxSize()
            .background(Color(0xFF101010))) {
        Text("Jetpack Compose",
            textAlign = TextAlign.Center,
            fontFamily = fontFamily,
            fontSize = 30.sp,
            color = Color.White,
            fontStyle = FontStyle.Italic,
            textDecoration = TextDecoration.Underline

        )
    }
}
```

#### ![](https://images.velog.io/images/blucky8649/post/2672138e-dbb5-4bb4-b99b-b52ffbfa0035/image.png)

결과 화면입니다. 다소 밋밋하므로 각 문자열의 첫글자만 다르게  스타일링을 해볼게요. 특정 문자열만 꾸미고 싶다 하시면 `buildAnnotatedString`과 `withStyle`을 사용하시면 됩니다. "Jetpack Compose" 자리에 다음 코드처럼 `buildAnnotatedString`을 넣어봅시다!

```kotlin
setContent {
    Box(
        Modifier
            .fillMaxSize()
            .background(Color(0xFF101010))) {
        Text(
            text = buildAnnotatedString {
                withStyle(
                    style = SpanStyle(
                        Color.Green,
                        fontSize = 50.sp
                    ),
                ) {
                    append("J")
                }
                append("etpack")
            },
            textAlign = TextAlign.Center,
            fontFamily = fontFamily,
            fontSize = 30.sp,
            color = Color.White,
            fontStyle = FontStyle.Italic,
            textDecoration = TextDecoration.Underline

        )
    }

}
```
약간 `StringBuilder`가 생각나네요 ㅎㅎ

#### ![](https://images.velog.io/images/blucky8649/post/b5494909-66d9-4273-9977-6f9b2ed918f7/image.png)

결과 화면입니다. J의 Style이 바뀐 것을 보실 수 있습니다.

이제 나머지 Compose도 위 코드와 똑같이 활용 하시면 됩니다.

```kotlin
setContent {
    Box(
        Modifier
            .fillMaxSize()
            .background(Color(0xFF101010))) {
        Text(
            text = buildAnnotatedString {
                withStyle(
                    style = SpanStyle(
                        Color.Green,
                        fontSize = 50.sp
                    ),
                ) {
                    append("J")
                }
                append("etpack ")

                withStyle(
                    style = SpanStyle(
                        Color.Green,
                        fontSize = 50.sp
                    )
                ) {
                    append("C")
                }
                append("ompose")
            },
            textAlign = TextAlign.Center,
            fontFamily = fontFamily,
            fontSize = 30.sp,
            color = Color.White,
            fontStyle = FontStyle.Italic,
            textDecoration = TextDecoration.Underline

        )
    }
}
```

#### ![](https://images.velog.io/images/blucky8649/post/ebc3ccdc-a489-4dc7-b331-4adeecedaf20/image.png)

최종 결과입니다. ㅎㅎ
