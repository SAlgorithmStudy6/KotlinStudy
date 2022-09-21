
---

# List 활용하기

List는 순서에 따라 정렬된 요소를 가지는 컬렉션으로 가장 많이 사용되는 컬렉션 중에 하나이다.

헬퍼 함수를 통해서 우리는 List를 만들어서 사용할 수 있다.
> 헬퍼 함수 : List와 같은 컬렉션은 특정 함수의 도움을 통해서 생성하는데 이때 사용하는 함수를 헬퍼함수라고 한다.


<br>

## 불변형 List 사용해보기.


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


**1. 일반적인 for문 in 사용하기**

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

**2. indices 사용**

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


**emptyList<>()**를 사용할 수 있다. 이때는 반드시 형식 매개변수를 지정한다.

``` kotlin

    val emptyList : List<String> = emptyList<String>();
    println(emptyList)
    // []

```

<br>

listOfNotNull()로 초기화하면 null을 제외한 요소만 반환해 List를 구성할 수 있다.

``` kotlin

    // null이 아닌 요소만 출력
    val nonNullsList: List<Int> = listOfNotNull(2, 45, null, 5, null)
    println(nonNullsList)
    // [2, 45, 5]

```

<br>


**Collection 인터페이스의 메소드를 오버라이딩 해서 구현을 하고있다.**

``` kotlin

    var names: List<String> = listOf("one", "two", "three")

    println(names.size) // 리스트 사이즈
    println(names.get(0)) // 리스트 요소 가져오기
    println(names.indexOf("three")) // 해당 value와 일치하는 인덱스를 반환
    println(names.contains("two")) // 해당 value를 포함하고 있는지 여부 확인

//    3
//    one
//    2
//    true


```

코틀린에서는 get()을 굳이 쓰지 않고 배열처럼 []를 통해서도 값을 집어넣거나 불러올 수 있습니다.

``` kotlin

    println(names[0]) 
    //    one

```

<br>

---

## 가변형 List 생성하기


**가변형 arrayListOf()**



``` kotlin

    val stringList = arrayListOf<String>("Hello", "Kotlin", "Java")
    stringList.add("Java")
    stringList.remove("Hello")
    println("stringList : ${stringList}")
//  stringList : [Kotlin, Java, Java]

```

- 가변형, 크기가 정해져있지 않기 때문에 add, remove를 통해서 요소를 추가하거나 삭제할 수 있다.
- 컴파일할 때, 반환되는 자료형은 List가 아닌 자바의 ArrayList이다.

<br>

**가변형 mutableListOf()**

``` kotlin

    mutableList.add("Ben")
    mutableList.removeAt(1)
    mutableList[0] = "Sean"
    println(mutableList)
    // [Sean, Cheolsu, Ben]

    val mutableListMixed = mutableListOf("Android", "Apple", 5, 6, 'X')
    println(mutableListMixed)
    // [Android, Apple, 5, 6, X]

```

<br>


- mutableListOf는 ArrayList가 아닌 MutableList를 반환하고 있다.
- add(), removeAt()을 통해서 추가 삭제를 할 수 있다.
- set()을 통해서 요소를 바로 변경할 수 있다.
- 기존의 불변형 List를 가변형으로 변경하려면 toMutableList()를 사용할 수 있는데, 이렇게 하면 기존의 List는 그대로 두고 새로운 공간을 만들어 낸다.

``` kotlin

    val names = listOf("one", "two", "three")
    val mutableNames = names.toMutableList()
    mutableNames.add("four")
    println("mutableNames : ${mutableNames}")
    // mutableNames : [one, two, three, four]

```

<br>

**List와 배열 array의 차이**

- List는 배열을 위해 사용한 Array<T>와 사용 방법이 비슷하다.
- Array 클래스에 의해 생성된 배열 객체는 내부 구조상 고정된 크기의 메모리를 가지고 있다.
- 코틀린에서는 List<T>와 MutableList<T>는 인터페이스로 설계되어 있고 따라서 특정한 자료구조에 따라 성능이 달라진다.
  	
  - List<T>로 구현한 LinkedList<T>와 ArrayList<T>는 성능이 다르다.
  
  
<br>

  
- List<T>는 Array<T>처럼 메모리 크기가 고정된 것이 아니기 때문에 자료구조에 따라 늘리거나 줄이는 것이 가능하다.
  
- Array<T>는 제네릭 관점에서 상, 하위 자료형 관계가 성립하지 않는 무변성이기 때문에 Array<Int>는 Array<Number>와 무관하다.
  
- 코틀린의 MutableList<T>도 이와 동일하다.
	
  - 하지만, List<T>는 공변성이기 때문에 하위인 List<Int>가 List<Number>에 지정될 수 있다.


    
    
