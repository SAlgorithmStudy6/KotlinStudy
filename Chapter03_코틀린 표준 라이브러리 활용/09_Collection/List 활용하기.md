
---

## List 활용하기

List는 순서에 따라 정렬된 요소를 가지는 컬렉션으로 가장 많이 사용되는 컬렉션 중에 하나이다.

헬퍼 함수를 통해서 우리는 List를 만들어서 사용할 수 있다.
> 헬퍼 함수 : List와 같은 컬렉션은 특정 함수의 도움을 통해서 생성하는데 이때 사용하는 함수를 헬퍼함수라고 한다.


<br>

# 불변형 List 사용해보기.


``` kotlin

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

<br>

**불변형 이지만 mixedTypes 형식 매개변수가 &#60;Any&#62;를 가진다.**
![image](https://user-images.githubusercontent.com/74912130/191020916-e03b022a-d2c1-4b05-a832-8039ca1a3927.png)


``` kotlin
var list2 = listOf("Hello", 1, 2.445, 's')
// [Hello, 1, 2.445, s]

println(list2.javaClass)
// class java.util.Arrays$ArrayList

```


<br>

### List 순환


``` kotlin

    list = mutableListOf(1, 2, 3, 4, 5, 6, 7)
    for(item in list) {
        println(item)
    }

//    1
//    2
//    3
//    4
//    5
//    6
//    7


```

<br>

**index를 통해서 list를 접근하기 위해서는 .indices멤버를 추가하면 됩니다.**

``` kotlin

    // 인덱스를 통한 접근
    for(index in list.indices) {
        println("list[$index] = ${list[index]}")
    }

//    list[0] = 1
//    list[1] = 2
//    list[2] = 3
//    list[3] = 4
//    list[4] = 5
//    list[5] = 6
//    list[6] = 7

```

<br>

### 비어있는 List생성


emptyList<>()를 사용할 수 있다. 이때는 반드시 형식 매개변수를 지정한다.











    
    
    
    
    
