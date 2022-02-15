![](https://media.vlpt.us/images/blucky8649/post/937d518f-c201-4d4b-898b-e965cb46a00a/%EC%A0%9C%EB%AA%A9%EC%9D%84-%EC%9E%85%EB%A0%A5%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94_-001%20(1).png)

지난 시간에는 Compose 프로젝트를 만들어보고 간단한 코드를 살펴보았는데요, 이번에는 직접 코드를 수정하여 텍스트를 출력해봅니다.

## Columns & Rows
일단 아래 코드처럼 Hello와 World를 출력해봅시다.
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Text("Hello")
            Text("World")
        }
    }
}
```

에뮬레이터를 실행하여 결과를 보면 다음과 같이 두 글자가 겹쳐져 있는 것을 보실 수 있습니다.
### ![결과 1](https://images.velog.io/images/blucky8649/post/1a081fa6-e482-4fba-acec-8077e74439d6/image.png)


이 합쳐진 문자열들을 세로, 혹은 가로로 분리하기 위해서는 `Row`, `Column`을 사용해야 합니다. 아래 코드와 같이 말이죠.

```kotlin
// 코드 생략..
super.onCreate(savedInstanceState)
setContent {
    Row{
        Text("Hello")
        Text("World")
    }
}
```
### ![](https://images.velog.io/images/blucky8649/post/d40f4972-745c-4e5c-be8b-c5a4ecdcd7ca/image.png)
결과를 실행해보면 두 문자열이 가로로 배치된 것을 보실 수 있습니다.

만약 `Row` 대신 `Column`을 사용한다면 세로로 배치 되겠죠? 

## 텍스트 중앙 배치
텍스트를 화면 중앙에 놓고자 할때는 `horizontalAlignment` 를 사용하시면 됩니다. 아래 코드를 입력하시고 결과 화면을 띄워봅시다.

```kotlin
super.onCreate(savedInstanceState)
setContent {
    Column(horizontalAlignment = Alignment.CenterHorizontally){
        Text("Hello")
        Text("World")
    }
}
```
### ![](https://images.velog.io/images/blucky8649/post/3ebab45b-5b26-4c47-bc3f-4c7192ecd491/image.png)

~~엥? 달리진게 없는데요 이 사기꾼~~
언뜻 보기에는 달라진 것이 없는 것 처럼 보입니다.  
그러나 문자열 하나 하나를 자세히 보면 **텍스트 상자안에서만 가운데 정렬**이 된 것을 볼 수 있습니다.

### ![](https://images.velog.io/images/blucky8649/post/6398cbaf-4279-40bc-a27e-3d3f90147b0f/image.png)

위 사진과 같이 텍스트 박스의 사이즈가 wrap content 되어 한정된 공간 안에서만 가운데 정렬이 된 것을 보실 수 있습니다.

그러면 텍스트가 전체화면의 가운데에 오게 하려면 어떻게 해야 할까요??

간단합니다. 텍스트 박스의 사이즈를 키워주면 되겠죠?
이렇게 텍스트 상자의 사이즈를 키워줄 때는 `modifier`를 사용합니다.  
다음 코드와 같이 `fillMaxSize`를 이용해주면 텍스트 박스의 크기는 화면을 꽉 채우게 됩니다.

```kotlin
setContent {
    Column(
        Modifier.fillMaxSize(), // xml에서의 match_parent와 같은 의미입니다.
        horizontalAlignment = Alignment.CenterHorizontally){
        Text("Hello")
        Text("World")
    }
}
```

### ![](https://images.velog.io/images/blucky8649/post/a9da15e0-327c-48ed-bc17-d74982d67ef7/image.png)

결과를 확인해 보면 전체화면에서 텍스트들이 가로 중앙에 배치된 것을 보실 수 있습니다.

### ![](https://images.velog.io/images/blucky8649/post/be2f2e3b-50af-40ef-95c1-01aba76f2ebb/image.png)

그외에도 Modifier의 속성을 살펴보면, padding을 활용한 여백지정, 배경 색 지정 등을 하실 수 있는 것을 확인하실 수 있습니다. 

다음 코드와 같이 입력하고, 결과를 출력해봅시다.

```kotlin
setContent {
    Column(
        Modifier.fillMaxSize()
            .background(Color.Green),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally){
        Text("Hello")
        Text("World")
    }
}
```

### ![](https://images.velog.io/images/blucky8649/post/45f3eec0-9b08-4132-b732-47834fa668ba/image.png)

텍스트가 화면의 정 중앙에 온 것을 확인할 수 있습니다.

여기서 참고해야할 것은 Column으로 지정하실때 수직 가운데 정렬은 `Arrangement`를 이용하고 수평 가운데 정렬은 `Alignment`를 이용했습니다.

그러나 Row로 지정하실 때에는 반대로 수평 가운데 정렬에는 `Arrangement`를 사용하시고, 수직 가운데 정렬에는 `Alignment`를 사용하여야 합니다.
