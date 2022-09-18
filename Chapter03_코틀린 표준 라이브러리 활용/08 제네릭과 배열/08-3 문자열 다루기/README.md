# 08-3 문자열 다루기

## 3-1. 문자열의 기본 처리

```kotlin
val hello : String = "Hello World"
println(hello[0]) //첫 번째 요소 출력

hello[0] = 'K' //오류! var나 val에서 하나의 요소에 새 값을 할당할 수 없음
var s = "abcdef"
s = "xyz" //새로운 메모리 공간이 생성 기존 사용하던 메모리 공간은 GC(Garbage Collector)에 의해 제거

GC : 더 이상 사용하지 않게 된 객체를 메모리에서 자동적으로 제거하는 역할
```

- **문자열 추출하고 병합하기**

```kotlin
var s = "abcdef"
println(s.substring(0..2)) // abc
s = s.substring(0..1) + "x" + s.substring(3..s.length-1) // abxdef
```

- **문자열 비교하기**

```kotlin
var s1 = "Hello Kotlin"
var s2 = "Hello KOTLIN"
//같으면 0, s1 < s2이면 양수, 반대면 음수를 반환
println(s1.compareTo(s2)) //32
println(s1.compareTo(s2,true)) //대소문자 무시 0
```

- **StringBulder 사용하기**

StringBuilder를 사용하면 문자열이 사용할 공간을 좀 더 크게 잡을 수 있기 때문에 특정 요소를 변경할 수 있다. 단 기존의 문자열보다는 처리 속도가 좀 느리고, 만일 단어를 변경하지 않고 그대로 사용하면 임시 공간인 메모리를 조금 더 사용하게 되므로 낭비된다는 단점도 존재. 

→ 문자열이 자주 변경되는 경우에 사용하면 좋다.

```kotlin
var s = StringBuilder("Hello")
s[2] = 'x'
```

```kotlin
s.append("World") //HexloWorld
s.insert(10,"Added") //HexloWorldAdded
s.delete(5,10) //인덱스 5번부터 10번 까지 삭제 HexloAdded
```

- **기타 문자열 처리**

```kotlin
var deli = "Welcome to Kotlin"
val sp = deli.split(" ") //공백을 기준으로 split : Welcome, to, Kotlin

str.split("=","-") //하나 이상의 분리 문자 지정
```

- **문자열을 정수로 변환하기**

```kotlin
val number : Int = "123".toInt() // 자바에서는 Integer.parseInt(number)
toIntorNull() : 숫자가 아닌 문자열이 들어왔을 때 null로 반환
```

## 3-2. 리터럴 문자열

- **이스케이프 문자의 종류**

|  종류 |  |  |
| --- | --- | --- |
| \t 탭 | \r 캐리지 리턴 | \\ 백슬래시 |
| \b 백스페이스 | \’ 작은따옴표 | \$ 달러 기호 |
| \n 개행 | \” 큰따옴표 |  |

```kotlin
val str = "\tYou're just too \"good\" to be true\n\tI can't take my eyes off you."
//You're just too "good" to be true
//I can't take my eyes off you.
val uni = "\uAC00" //가
```

```kotlin
//3중 따옴표를 사용하면 이스케이프 문자 중 개행 문자를 넣지 않고도 개행 표시 가능
val text = """ /
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
""".trimMargin() //trim default는 | : | 기준으로 개행문자
```

- **형식 문자 사용하기**
    - 형식 문자의 종류
    
    | 종류 |  |  |
    | --- | --- | --- |
    | %b 참과 거짓의 Boolean 유형 | %o 8진 정수 | %g 10진 혹은 E 표기법의 실수 |
    | %d 부호 있는 정수 | %t 날짜나 시간 | %n 줄 구분 |
    | %f 10진 실수 | %c 문자 | %s 문자열 |
    | %h 해시 코드 | %e E 표기법의 실수 | %x 16진 정수 |

```kotlin
val pi = 3.1415926
val dec = 10
val s = "hello"
println("pi = %.2f, %3d, %s".format(pi,dec,s)) //3.14 ( 10) hello
```