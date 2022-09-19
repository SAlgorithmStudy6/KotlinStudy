
---

## List 활용하기

List는 순서에 따라 정렬된 요소를 가지는 컬렉션으로 가장 많이 사용되는 컬렉션 중에 하나이다.

헬퍼 함수를 통해서 우리는 List를 만들어서 사용할 수 있다.
> 헬퍼 함수 : List와 같은 컬렉션은 특정 함수의 도움을 통해서 생성하는데 이때 사용하는 함수를 헬퍼함수라고 한다.


<br>

## 불변형 List 사용해보기.



```

    var list = listOf(1, 2, 3)
    println(list)
    // [1, 2, 3]

    list = arrayListOf(1, 2, 3)
    println(list)
    // [1, 2, 3]

    list = mutableListOf(1, 2, 3)
    println(list)
    // [1, 2, 3]


```
