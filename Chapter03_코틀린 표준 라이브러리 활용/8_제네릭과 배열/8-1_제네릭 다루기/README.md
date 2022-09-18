# 08-1 제네릭 다루기

## 1-1. 제네릭이란?

- **제네릭(Generic)**은 클래스 내부에서 사용할 자료형을 나중에 인스턴스를 생성할 때 확정한다.
- 제네릭을 사용하면 객체의 자료형을 컴파일할 때 체크하기 때문에 객체 자료형의 안전성을 높이고 형 변환의 번거로움이 줄어듬.

## 1-2. 제네릭의 일반적인 사용 방법

사용방법

- 제네릭을 사용하기 위해 **앵글 브래킷(<>)** 사이에 형식 매개변수를 넣어 선언하며 이때 형식 매개변수는 하나 이상 지정할 수 있다.

장점 

- 의도하지 않은 자료형의 객체를 지정하는 것을 막고 객체를 사용할 때 원래의 자료형에서 다른 자료형으로 형 변환 시 발생할 수 있는 오류를 줄여 준다.

간단한 제네릭의 예 

```kotlin
class Box<T> (t: T){ //형식 매개변수로 받은 인자를 name에 저장
    var name = t
}

fun main(){
    val box1 : Box<Int> = Box<Int>(1)
    val box2 : Box<String> = Box<String>("Hello")
println(box1.name)
println(box2.name)
}
```

실행 결과

 1

 Hello

- 제네릭에서 사용하는 형식 매개변수 이름 (Box<T>에서 T가 바로 형식 매개변수 이름)

|  형식 매개변수 이름 |                                   의미 |
| --- | --- |
|  E |  요소(Element) |
|  K |  키(key) |
|  N  |  숫자(Number) |
|  T |  형식(Type) |
|  V |  값(Value) |
|  S, U, V etc. |  두 번째 , 세 번째, 네 번째 형식 |
- 객체 생성시 만일 생성자 에서 유추될 수 있는 자료형이 있다면 선언된 자료형인 <String>이나 <Int>는 생략이 가능

```kotlin
val box3 = Box(1) // 1은 int형이므로 Box<Int>로 추론
val box4 = Box("Hello") // Hello는 String형이므로 Box<String>으로 추론
```

제네릭을 사용하면 인자의 자료형을 고정할 수 없거나 예측할 수 없을 때 형식 매개변수인 T를 이용해 실행 시간에 자료형을 결정할 수 있게 되므로 매우 편리

- **제네릭 클래스** : 형식 매개변수를 1개 이상 받는 클래스. 클래스를 선언할 때 자료형을 특정하지 않고 인스턴스를 생성하는 시점에서 클래스의 자료형을 정하는 것
    
    ex)  Box<T>
    

- 자료형이 특정되지 못하므로 인스터를 생성할 수 없음 → 만일 형식 매개변수를 클래스의 프로퍼티에 사용하는 경우 클래스 내부에서는 사용할 수 없다.

```kotlin
class Myclass<T>{
    var myProp : T // 오류) 프로퍼티는 초기화되거나 abstarct로 선언되어야 한다.
}
```

다음과 같이 주 생성자나 부 생성자에 형식 매개변수를 지정해 사용할 수 있다.

```kotlin
class MyClass<T>(val myProp : T){} //주 생성자의 프로퍼티
```

```kotlin
class MyClass<T>{
    val myProp : T // 프로퍼티
    constructor(myProp : T){ //부 생성자 이용
        this.myProp = myProp
    }
}
```

그러면 객체 인스턴스를 생성할 때 명시적으로 자료형을 지정할 수 있다.

```kotlin
var a = MyClass<Int>(12) //주 생성자 myProp에는 12가 할당되며 int형으로 결정됨
println(a.myProp) //12
println(a.javaClass) //MyClass
```

- 제네릭 클래스의 자료형 변환하기

```kotlin
open class Parent

class Child : Parent()

class Cup<T>

fun main() {
    val obj1: Parent = Child() // Parent 형식의 obj1은 Child의 자료형으로 변환될 수 있음
    val obj2: Child = Parent() // 오류! 자료형 불일치

    val obj3: Cup<Parent> = Cup<Child>() // 오류! 자료형 불일치
    val obj4: Cup<Child> = Cup<Parent>() // 오류! 자료형 불일치

    val obj5 = Cup<Child>() // obj5는 Cup<Child>의 자료형이 됨
    val obj6: Cup<Child> = obj5 // 자료형이 일치하므로 OK!
}
```

이처럼 상,하위 클래스를 형식 매개변수에 지정해 서로의 관계에 따라 변환이 가능하게 하려면 제네릭의 가변성을 주기 위해 in,out에 대해 이해해야 한다.

- 제네릭의 형식 매개변수는 기본적으로 null 가능한 형태로 선언됨.

```kotlin
class GenericNull<T> { //기본적으로 null이 허용되는 형식 매개변수
    fun eqaulityFunc(arg1 : T, arg2 : T){
println(arg1?.equals(arg2))
    }
}

fun main() {
    val obj = GenericNull<String>() // non-null로 선언
    obj.eqaulityFunc("Hello","World") //null이 허용되지 않음

    val obj2 = GenericNull<Int?>() // null이 가능한 형식으로 선언
    obj2.eqaulityFunc(null,10)
}
```

실행 결과

false

null

- 형식 매개변수 T는 기본적으로 null을 허용 (자료형에 ?  기호를 사용). 그래서 obj2에서 equals()로 비교하지 않고 null인 경우 null을 반환함.

- 형식 매개변수에 특정 자료형을 지정 (null 허용을 제한)

```kotlin
class GenericNull<T : Any> { //자료형 Any가 지정되어 null을 허용하지 않음
}

fun main() {
    val obj2 = GenericNull<Int?>() // 오류! null이 허용되지 않음
}
```

- **제네릭 함수 혹은 메서드** :  형식 매개변수를 받는 함수나 메서드
    
    fun<형식 매개변수[,…]> 함수 이름(매개변수 : <매개변수 자료형>[,….]): <반환 자료형>
    
    ex)
    
    ```kotlin
    fun<T> genericFunc(arg: T):T? {....} // 매개변수와 반환 자료형에 형식 매개변수 T가 사용됨
    
    fun<K,V> put(key:K, value: V): Unit{....} // 형식 매개변수가 2개인 경우
    ```
    

- 형식 매개변수로 선언된 함수의 매개변수를 연산할 경우 자료형을 결정할 수 없기 때문에 오류

```kotlin
fun <T> add(a: T, b: T): T {
    return a + b //오류!
}
```

→ 람다식을 매개변수로 받으면 손쉽게 해결

```kotlin
fun <T> add(a: T, b: T, op: (T, T) -> T): T {
    return op(a, b)
}

fun main() {
    val result =add(2, 3,{a, b->a + b}) //add(2,3){a,b -> a+b}와 같음
		println(result) //5
}
```

→ 람다식만 변수로 따로 정의해 add() 함수의 인자로 넣은 경우

```kotlin
var sumInt: (Int,Int) -> Int ={a,b->a+b}// 변수 선언부가 있는 경우 표현식의 자료형 생략
var sumInt2 ={a: Int, b: Int->a+b}// 변수 선언부가 생략된 경우에는 표현식에 자료형 표기

println(add(2,3,sumInt))
println(add(2,3,sumInt2))
```

→ 람다식 매개변수를 typealias를 사용해 다른 이름으로 준 경우

```kotlin
typealias arithmetic<T> = (T, T) -> T

fun <T> addAux(a: T, b: T, op: arithmetic<T>): T {
    return op(a, b)
}
```

## 1-3. 자료형 제한하기

자바에서는 extends나 super를 사용 → 코틀린에서는 콜론(:)을 사용해 제네릭 클래스나 메서드가 받는 형식 매개변수를 특정한 자료형으로 제한

- **클래스에서 형식 매개변수의 자료형 제한하기**

```kotlin
class Calc<T: Number>{// 클래스의 형식 매개변수 제한
    fun plus(arg1: T, arg2: T): Double{
        return arg1.toDouble() + arg2.toDouble()
    }
}

fun main() {
    val calc = Calc<Int>()
println(calc.plus(10,20)) //30.0

    val calc2 = Calc<Double>()
    val calc3 = Calc<Long>()
    val calc4 = Calc<String> //제한된 자료형으로 인해 오류 발생

println(calc2.plus(2.5,3.5)) //6.0
println(calc3.plus(5L,10L)) //15.0
}
```

- **함수에서 형식 매개변수의 자료형 제한하기**

```kotlin
fun <T : Number> addLimit(a: T, b: T, op: (T, T) ->): T {
    return op(a, b)
}

valresult=addLimit("abc", "def",{a, b->a + b}) //제한된 자료형이므로 오류
```

- **다수 조건의 형식 매개변수 제한하기(where 키워드 사용) : 클래스**

```kotlin
interface  InterfaceA
interface  InterfaceB

class HandlerA : InterfaceA, InterfaceB
class HandlerB : InterfaceA

class ClassA<T> where T:InterfaceA, T:InterfaceB // 2개의 인터페이스를 구현하는 클래스로 제한

fun main() {
    val obj1 = ClassA<HandlerA>() // 객체 생성 가능
    val obj2 = ClassA<HandlerB>() // 범위에 없으므로 오류 발생(1개의 인터페이스)
}
```

- **다수 조건의 형식 매개변수 제한하기(where 키워드 사용) : 함수**

```kotlin
//myMax의 인자 a,b에 들어갈 자료형을 숫자형과 비교형만으로 제한
fun <T> myMax(a: T, b: T): T where T : Number, T : Comparable<T> { 
    return if (a > b) a else b
}
```

## 1-4. 상 · 하위 형식의 가변성

**가변성** : 형식 매개변수가 클래스 게층에 영향을 주는 것 

**하위 형식 :** 형식 A의 값이 필요한 모든 클래스에 형식 B의 값을 넣어도 아무 문제가 없는 경우 B는 A의 하위 형식(SubType)이 됨.

- **가변성의 세가지 유형**

|                 용어 |                                        의미 |
| --- | --- |
| 공변성(Covariance) | ‘T’가 T의 하위 자료형이면, C<’T’>는 C<T>의 하위 자료형이다. 생산자 입장의 out 성질 |
| 반공변성(Contravariance) | ‘T’가 T의 하위 자료형이면, C<’T’>는 C<T>의 하위 자료형이다. 소비자 입장의 in 성질 |
| 무변성(Invariance) | C<T>와 C<’T’>는 아무 관계가 없다. 생산자 + 소비자 |

![Untitled](08-1%20%E1%84%8C%E1%85%A6%E1%84%82%E1%85%A6%E1%84%85%E1%85%B5%E1%86%A8%20%E1%84%83%E1%85%A1%E1%84%85%E1%85%AE%E1%84%80%E1%85%B5%20b3889203f2144a299cf47823a13b740c/Untitled.png)

- **무변성 :** 제네릭 클래스를 인스턴스화할 때 서로 다른 자료형을 인자로 사용하려면 자료형 사이의 상,하위 관계를 잘 따져야 한다. (in,out 키워드 사용)

```kotlin
class Box<T> (val size: Int) // 무변성(Invariance) 선언

fun main() {
    val anys: Box<Any> = Box<Int>(10) // 오류! 자료형 불일치
    val nothings: Box<Nothing> = Box<Int>(20) // 오류! 자료형 불일치
}
```

- **공변성 :** 형식 매개변수의 상하 자료형 관계가 성립하고, 그 관계가 그대로 인스턴스 자료형의 관계로 이어지는 경우. out 키워드를 사용

```kotlin
class Box<out T> (val size: Int) // 공변성(Invariance) 선언

fun main() {
    val anys: Box<Any> = Box<Int>(10) // 관계 성립으로 객체 생성 가능
    val nothings: Box<Nothing> = Box<Int>(20) // 오류! 자료형 불일치
}
```

- **반공변성 :** in 키워드를 사용

```kotlin
class Box<in T> (val size: Int) // 반공변성(Invariance) 선언

fun main() {
    val anys: Box<Any> = Box<Int>(10) // 오류! 자료형 불일치
    val nothings: Box<Nothing> = Box<Int>(20) // 관계 성립으로 객체 생성 가능
}
```

```kotlin
// 주생성자에서 out을 사용하는 경우에 형식 매개변수를 갖는 프로퍼티는 var로 지정될 수 없음
(val)사용
class Box<out T : Animal> (val elem: T)
class Box2<out T: Animal> (private var elem: T) //private로 지정하면 사용가능
```

## 1-5. 자료형 프로젝션

- **가변성의 2가지 방법**
    - **선언 지점 변성**
        - 클래스를 선언하면서 클래스 자체에 가변성을 지정하는 방식(클래스에 in/out을 지정할 수 있다) → 전체적으로 지정하는 것이 되기 때문에 클래스를 사용하는 장소에서는 따로 자료형을 지정해 줄 필요가 없어 편리하다.
    
    ```kotlin
    class Box<in T: Animal>(var item:  T)
    ```
    
    - **사용 지점 변성**
        - 메서드 매개변수에서 또는 제네릭 클래스를 생성할 때와 같이 사용 위치에서 가변성을 지정하느 방식
        
        ```kotlin
        class Box<T : Animal>(var item: T) // in, out을 지정하지 않아 무변성
        
        fun <T> printObj(box: Box<out Animal>) { //자료형 프로젝션(특정 자료형에 in 혹은 out을 지정해 제한
            val obj: Animal = box.item // item의 값을 얻음(get)
            println(obj)
        }
        ```
        
        - 자료형 안전성을 보장, 그리고 out에 의한 게터만 허용하고 in에 의한 세터는 금지
        
        ```kotlin
        fun main() {
            val animal : Box<Animal> = Box(Animal())
            val cat : Box<Cat> = Box(Cat())
        
        println(animal) // 가능!
        println(cat) //오류 Box<>는 무변성으로 지정되었기 때문에 cat은 animal의 하위타입
        }
        ```
        

- **스타 프로젝션 :** in과 out을 정하지 않고 *을 통해 지정하는 방식
    - Box<Any?> 차이점 : Any타입은 모든 자료형의 요소를 담을 수 있음을 의미하는 반면, *는 어떤 자료형이라도 들어올 수 있으나 구체적으로 자료형이 결정되고 난 후에는 그 자료형과 하위 자료형의 요소만 담을 수 있도록 제한.
    
    ```kotlin
    class InOutTest<in T, out U>(t: T, u: U){
        val propT: T = t // 오류! T는 in 위치이기 때문에 out 위치에 사용 불가
        val propU: U = u // U는 out 위치로 가능
    
        fun func1(u: U) // 오류! U는 out 위치이기 때문에 in 위치에 사용 불가
        fun func2(t: T){ // T는 in 위치에 사용됨
    print(t)
        }
    }
    
    fun starTestFunc(v: InOutTest<*,*>){
        v.func2(1) // in으로 정의되어 있는 형식 매개변수를 *로 받으면 in Nothing으로 간주 out은 Any?로 간주
    print(v.propU)
    }
    ```
    
    - **Nothing 클래스 :** 코틀린의 최하위 자료형으로 아무것도 가지지 않는 클래스

- **자료형 프로젝션의 정리**

|  종류 | 예 | 가변성 | 제한 |
| --- | --- | --- | --- |
| out 프로젝션 | Box<out Cat> | 공변성 | 형식 매개변수는 세터를 통해 값을 설정하는것이 제한된다. |
| in 프로젝션 | Box<in Cat> | 반공변성 | 형식 매개변수는 게터를 통해 값을 읽거나 반환할 수 있다. |
| 스타 프로젝션 | Box<*> | 모든 인스턴스는 하위 형식이 될 수 있다. | in과 out은 사용 방법에 따라 결정된다. |

## 1-6. reified 자료형

```kotlin
// T 자료형은 자바처럼 실행 시간에 삭제되기 때문에 T 자체에 그래도 접근할 수 없음
// <T>처럼 결정되지 않은 제네릭 자료형은 컴파일 시간에는 접근 가능
// 함수 내부에서 사용하려면 함수의 매개변수를 넣어 c:Class<T>처럼 지정해야만 실행 시간에 삭제되지 않고 접근 가능
fun <T> myGenericFun(c: Class<T>) // Class<T> : .class 형태로 반환 받는 객체
코틀린 : Object::class //KClass
자바 : Object::class.java //Class
```

→ reified로 형식 매개변수 T를 지정하면 실행 시간에 접근할 수 있다.

```kotlin
inline fun <reified T> myGenericFun() //인라인 함수에서만 사용 가능
```

ex) 

```kotlin
fun<T> String.toKotlinObject(): T {
    val mapper = jacksonObjectMapper()
    return mapper.readValue(this, T::class.java) //오류!
} //readValue()는 JsonObject로 변환해 읽을 수 있게 하는 함수
```

→ 명시적으로 클래스 매개변수를 이용한 경우

```kotlin
fun <T:Any> String.toKotlinObject(c:KClass<T>): T {
    val mapper = jacksonObjectMapper()
    return mapper.readValue(this, c.java) //오류!
}
```

```kotlin
data class MyJsonType(val name: String)

fun main() {
    val json = """{"name":"example"}"""
    json.toKotlinObject(MyJsonType::class)
}
```

→ reified 사용

```kotlin
inline fun <reified T:Any> String.toKotlinObject(): T {
    val mapper = jacksonObjectMapper()
    return mapper.readValue(this, T::class.java)
}

data class MyJsonType(val name: String)

fun main() {
    val json = """{"name":"example"}"""
    json.toKotlinObject<MyJsonType>()
}
```