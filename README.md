### ![](https://images.velog.io/images/blucky8649/post/1d18daa8-2e54-43ee-914c-28627f860711/image.png)

안녕하세요. 이번 포스팅은 Compose로 어떻게 리스트를 구성하는지 같이 알아보는 시간을 가져보겠습니다.

## 단순 반복문을 이용한 스크롤 구현
for loop를 이용하여 리스트를 반복적으로 생산하여 리스트를 구성해보겠습니다. 스크롤 상태 저장 변수인`scrollState`을 선언해주고, **verticalScroll** 속성을 이용해줍니다.

```kotlin
setContent {
    val scrollState = rememberScrollState()
    Column(
        Modifier.verticalScroll(scrollState)
    ) {
        for (i in 1..50) {
            Text(
                text = "${i}번 아이템",
                textAlign = TextAlign.Center,
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(vertical = 24.dp)
            )
        }
    }
}
```

### 실행 결과
이런 식으로 스크롤이 가능한 리스트 뷰가 만들어졌습니다.
#### ![](https://images.velog.io/images/blucky8649/post/ee81c7ee-fa41-4ac5-b0ef-f4a30204e1d4/image.png)

그러나 위 코드에서 for-loop을 이용하여 데이터를 5000개 만든다 하면 어떻게 될까요? 5000개의 데이터를 한꺼번에 읽어들이기 때문에 **성능 저하가 발생합니다.**

## LazyColumn을 이용한 스크롤
위와 같은 성능 저하 문제를 해결하기 위하여 `LazyColumn`을 사용합니다.
이는 화면에서 보이는 부분을 **재활용**하여 다음, 혹은 이전 데이터의 값을 재 갱신 해주는 방식입니다. 네 맞습니다. 그 **리사이클러뷰**.
`LazyColumn`은 스크롤 상태, 아이템의 개수 및 인덱스를 관리해주기 때문에 상태 저장 변수를 사용하지 않아도 됩니다.

다음 코드는 아이템 리스트 5000개를 생성하는 코드입니다.

```kotlin
setContent {
    LazyColumn {
        items(5000) {
            Text(
                text = "${it}번 아이템",
                textAlign = TextAlign.Center,
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(vertical = 24.dp)
            )
        }
    }
}
```
코드의 가독성 향상을 위해 다음과 같이 `it`을 다음과 같이 커스텀해주실 수도 있습니다.
```kotlin
items(5000) {index ->
    Text(
        text = "${index}번 아이템",
        textAlign = TextAlign.Center,
        fontSize = 24.sp,
        fontWeight = FontWeight.Bold,
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 24.dp)
    )
}
```

### 실행 결과
#### ![](https://images.velog.io/images/blucky8649/post/8a8399ab-8a28-48f7-9d5d-b5443acd3f1b/image.png)

## 배열에 담긴 값을 리스트로 맵핑하기
배열값을 리스트로 나타내고 싶다면 `itemsIndexed`를 사용하여 다음 코드와 같이 사용합니다.

```kotlin
setContent {
    LazyColumn {
        itemsIndexed(
            arrayOf("This", "is", "Jetpack", "Compose", "I", "Learned")
        ) { index, item ->
            Text(
                text = "$item",
                textAlign = TextAlign.Center,
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(vertical = 24.dp)
            )
        }
    }
}
```

### 실행 결과
#### ![](https://images.velog.io/images/blucky8649/post/62388653-2337-46be-af9c-cfb71c0bf22c/image.png)

여기에 3편에서 배운 `Card`를 사용하시면 `RecyclerView` + `CardView`의 형태가 됩니다.
```kotlin
setContent {
    LazyColumn {
        itemsIndexed(
            arrayOf("This", "is", "Jetpack", "Compose", "I", "Learned")
        ) { index, item ->
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(8.dp),
                elevation = 5.dp,
                shape = RoundedCornerShape(15.dp)
            ) {
                Text(
                    text = "$item",
                    textAlign = TextAlign.Center,
                    fontSize = 24.sp,
                    fontWeight = FontWeight.Bold,
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(vertical = 24.dp)
                )
            }

        }
    }
}
```

### ![](https://images.velog.io/images/blucky8649/post/1d18daa8-2e54-43ee-914c-28627f860711/image.png)

xml로 RecyclerView를 구현해야할 때는 카드뷰 레이아웃, 어댑터를 작성해야하다보니 여간 귀찮은 일이 아닐수가 없었습니다.
그러나 Compose는 그러한 스트레스없이 코틀린 코드로 간단하게 구현가능하다는 점이 정말 좋네요.
