# 08-2 배열 다루기

## 2-1. 배열을 사용하는 방법

- **기본적인 배열 표현**

```kotlin
val numbers =arrayOf(4,5,6,7) //정수형으로 초기화된 배열
val animals =arrayOf("Cat","Dog","Lion") //문자열형으로 초기화된 배열
```

- **다차원 배열**

```kotlin
val array1 =arrayOf(1,2,3)
val array2 =arrayOf(4,5,6)
val array3 =arrayOf(7,8,9)

val arr2d =arrayOf(array1,array2,array3)

val arr2d = *arrayOf*( *arrayOf*(1,2,3),*arrayOf*(4,5,6),*arrayOf*(7,8,9)) //간결한 표현

var arr3d = arrayOf(arrayOf(arrayOf(....),....),...) //3차원 배열
```

- **배열에 여러 가지 자료형 혼합하기**

```kotlin
val mixArr =arrayOf(4, 5, 7, 3, "Chike", false)

//데이터를 혼합해서 사용할 수 없는 경우
val intOnlyArr1 =arrayOf<Int>(4, 5, 7, 3)
val intOnlyArr2 =intArrayOf(4, 5, 7, 3)
```

- **배열 요소에 접근하기**
    - 배열 요소에 접근 : arr.get() = arr[]
    - 배열 요소에 값 설정 : arr.set(인덱스 위치, 값), arr[] = ?
    - 배열의 추가 메소드
    
    ```kotlin
    val arr =intArrayOf(1,2,3,4,5)
    
    println(Arrays.toString(arr)) //배열의 내용을 문자열로 변환 : 자바와 동일
    println(arr.size) //배열의 크기
    println(arr.sum()) //배열의 합
    ```
    

- **표현식을 통해 배열 생성하기**

```kotlin
//val|var 변수 이름 = Array(요소 개수, 초깃값)
val arr3 = Array(5,{i->i*2}) //배열의 크기 5 i번째 인덱스 * 2 값을 배열에 저장
```

- **요소 개수가 많은 배열 생성**

```kotlin
var a = arrayOfNulls<Int>(1000) //1000개의 null로 채워진 정수 배열

var b = Array(1000){0}//0으로 채워진 배열
```

## 2-2. 배열 제한하고 처리하기

- **배열에 요소 추가하고 잘라내기**

```kotlin
val arr1 =intArrayOf(1, 2, 3, 4, 5)
val arr2 = arr1.plus(6) //하나의 요소를 추가한 새 배열 생성 (1, 2, 3, 4, 5, 6)
val arr3 = arr1.sliceArray(0..2) //1번째부터 3번째까지의 인덱스 (1, 2, 3)
```

- **기타 배열 관련 API 사용하기**

```kotlin
println(arr.first()) //첫 번째 요소
println(arr.last()) //마지막 요소

println(arr.indexOf(3)) //3번째 인덱스 요소

println(arr.average()) //배열의 평균값

println(arr.count) //요소 개수

println(arr.contains(4)) //해당 value가 배열에 포함되어 있는지 boolean형 반환
// arr.contains(4) = 4 in arr
```

- **다양한 자료형을 위한 Any로 선언된 배열**
    
    Any로 만들어진 배열은 기존 자료형을 다른 자료형으로 지정할 수 있다.
    
    ```kotlin
    val b = Array<Any>(10,{0})
    b[0] = "Hello World"
    b[1] = 1.1
    println("${b[0]} ${b[1]} ${b[2]}") //Hello World 1.1 0
    ```
    
- **멤버 메서드를 통한 배열 순환하기**

```kotlin
arr.forEach{element->print("$element")}//foreach 문
arr.forEachIndexed({i,e->println("arr[$i] = $e")}) //foreach문에서 인덱스값도 표시
```

```kotlin
//iterator() 사용
val iter: Iterator<Int> = arr.iterator()
while (iter.hasNext()) { //배열에서 참조할 다음 요소가 있는지 확인
    val e = iter.next()
		print("$e ")
}
```

## 2-3. 배열 정렬하기

- **기본 배열 정렬하고 반환하기**

```kotlin
val arr =arrayOf(8, 4, 3, 2, 5, 9, 1)

val sortedNums = arr.sortedArray() //오름차순 정렬
val sortedNumsDesc = arr.sortedArrayDescending() //내림차순 정렬렬

arr.sort(1, 3) //1번째 인덱스부터 2번째 인덱스까지 정렬 (8,3,4,2,5,9,1)
arr.sortDescending() //내림차순 정렬

//List로 반환
val listSorted: List<Int> = arr.sorted()
val listDesc: List<Int> = arr.sortedDescending()

//SortBy를 이용한 특정 표현식에 따른 정렬
val items =arrayOf<String>("Dog","Cat","Lion","Kangaroo","Po")
items.sortBy{items->items.length}//문자열 길이 기준으로 정렬
```

- **sortBy()로 데이터 클래스 정렬하기**

```kotlin
val product =arrayOf(
    Product("Sonw Ball",870),
    Product("Smart Phone",999),
    Product("Drone",240),
    Product("Mouse",333),
    Product("Keyboard",125),
    Product("Monitor",1500),
    Product("Tablet",512)
)

product.sortByDescending{ it.price}//가격을 기준으로 내림차순 정렬
```

- **sortWith() 비교자로 정렬하기**

```kotlin
//Comparator 사용
product.sortWith(Comparator{p1, p2->
when{
        p1.price > p2.price -> 1
        p1.price == p2.price -> 0
        else -> -1
    }
})

//compareBy 사용
//이름을 기준으로 정렬하고 만약 같으면 가격 기준으로 정렬
product.sortWith(compareBy<Product>{ it.name}.thenBy{ it.price})
product.sortWith(compareBy({it.name},{it.price}))
```

- **배열 필터링하기**

```kotlin
val arr =arrayOf(1, -2, 3, 4, -5, 0)
arr.filter{e->e > 0}.forEach{e->print("$e")}//0보다 큰 수 골라내기
```

```kotlin
val fruits =arrayOf("banana", "avocado", "apple", "kiwi")
fruits //메서드 체이닝 : 메서드를 게속 연속해서 호출하는 방법
    .filter{ it.startsWith("a")}//a로 시작하는
    .sortedBy{ it }//이름순으로 정렬
    .map{ it.uppercase()}//대문자로 변경
    .forEach{println(it)}

when{ //when문을 통해 배열에서 특정 요소가 있는지 확인하는 방법 위에 메서드 체이닝기법을 사용했을 경우 특정 메서드에서 디버깅이 어렵기때문에 사용
	"apple" in fruits -> println("Apple!")
}
```

```kotlin
println(products.minBy{it.price}) //최솟값
println(products.maxBy{it.price}) //최대값
```

## 2-4. 배열 평탄화하기

```kotlin
val numbers =arrayOf(1, 2, 3)
val strs =arrayOf("one", "two", "three")
val simpleArray =arrayOf(numbers, strs) //2차원 배열

val flatternSimpleArray = simpleArray.flatten() //단일 배열로 변환하기
println(flatternSimpleArray) //Ljaba.lang.~~가 출력 X -> 1,2,3,one,two,three
```