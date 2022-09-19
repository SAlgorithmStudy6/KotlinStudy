
<br>

# Set과 Map활용하기
--- 

Set은 정해진 순서가 없는 요소들의 집합, 중복값이 없음 즉, 모든 요소가 unique함

Map은 요소가 키와 값의 쌍 형태로 저장된다. 키는 중복될 수 없고 유일합니다. 하지만 값은 중복해서 사용할 수 있다.


<br>

---
## Set 활용

Set은 setOf()를 사용해서 Set을 생성하고, mutableSetOf()를 이용해서 가변형 Set을 생성할 수 있다.

**불변형 setOf()**

``` kotlin

    val mixedTypeSet = setOf("Hello", "5", 1, 3.14)
    println(mixedTypeSet)
    // [Hello, 5, 1, 3.14]



    val inSet: Set<Int> = setOf<Int>(1, 2, 5, 5, 5)
    println(inSet)
	// [1, 2, 5]

```

setOf에서는 자료형을 혼합하거나 특정 자료형을 지정할 수 있다.
중복요소를 허용하지 않기 때문에 intSet에서는 5가 한번만 나타난다.

<br>

**가변형 mutableSetOf()**
mutableSetOf()함수로 요소의 추가 삭제가 가능 mutableSetOf()는 MutableSet인터페이스 자료형을 반환하는데, 내부적으로 자바의 LinkedHashSet을 만들어냅니다.

``` kotlin

    val animals = mutableSetOf("String", "Dog", "Cat")
    println(animals)

    animals.add("Dog")
    animals.add("Dog2")
    println(animals)

    animals.remove("Python")
    println(animals)
    
//    [String, Dog, Cat]
//    [String, Dog, Cat, Dog2]
//    [String, Dog, Cat, Dog2]
   

```

<br>

---
### Set의 여러가지 자료구조

**hashSetOf()**

- 해시 테이블에 요소를 저장할 수 있는  HashSet 컬렉션을 만든다.
- hashSetOf()는 HashSet을 반환하는데 HashSEt는 불변성 선언이 없기 때문에 추가 및 삭제 등의 기능을 수행할 수 있습니다.


``` kotlin

    val intsHashSet: HashSet<Int> = hashSetOf(6, 3, 4, 8);
    intsHashSet.add(5)
    intsHashSet.remove(6)

    println(intsHashSet)
    // [8, 3, 4, 5]

```

<br>

**sortedSetOf()**

- 자바의 TreeSet컬렉션을 정렬된 상태를 반환한다.

``` kotlin

    val instSortedSet: TreeSet<Int> = sortedSetOf(4, 1, 2, 7)
    instSortedSet.add(6)
    instSortedSet.remove(6)
    println("instSortedSet : ${instSortedSet}")

    // instSortedSet : [1, 2, 4, 7]

```

<br>

**linkedSetOf()**

- 자바의 LinkedHashSet 자료형을 반환하는 헬퍼함수.
- 해시 테이블에 요소를 저장하는데, 저장된 순서에 따라 값이 저장되어 정렬된다.
- TreeSet, HashSet보다는 확실히 느리지만, 포인터 연결을 통해 메모리를 효율적으로 사용할 수 있다.

``` kotlin

    val instLinkedHashSet: LinkedHashSet<Int> = linkedSetOf(1, 2, 7, 2, 9)
    instLinkedHashSet.add(4)
    instLinkedHashSet.remove(2)

    println("instLinkedHashSet : ${instLinkedHashSet}")
    // instLinkedHashSet : [1, 7, 9, 4]

```



---
## Map 활용

**불변형 mapOf()**

- mapOf()는 불변형 Map컬렉션을 만들 수 있다.

``` kotlin

    val langMap = mapOf(11 to "Java", 22 to "Kotlin", 33 to "C++")
    println(langMap)
	// {11=Java, 22=Kotlin, 33=C++}


```


**Key와 Value 따로 출력하기**

방법1. 

``` kotlin

    for((key, value) in langMap) {
        println("key : ${key} , value = ${value}")
    }
//    key : 11 , value = Java
//    key : 22 , value = Kotlin
//    key : 33 , value = C++


```

<br>

방법2.

``` kotlin

    langMap.forEach {
        println("key : " + it.key + ", value : " + it.value)
    }
//    key : 11, value : Java
//    key : 22, value : Kotlin
//    key : 33, value : C++


```


![](https://velog.velcdn.com/images/lifeisbeautiful/post/d4071240-19c4-4a3b-bfde-4032001b4003/image.png)












