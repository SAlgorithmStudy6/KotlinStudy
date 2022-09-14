# Chapter01-3. 함수와 함수형 프로그래밍
- 발표일 : 2022/09/07
- 발표자 : 이지윤
<br><br>

# 1. 함수 선언과 호출
### 1-1. 함수의 정의와 용도
- 정의 : 함수는 여러 값(인자)을 입력 받아, 기능을 수행하고 결괏값을 반환하는 코드의 모음
- 용도 : 코드의 재사용

<br>

### 1-2. 함수의 구조
1. 함수 구조 - 기본 표기 
- [필수] fun키워드로 함수 선언을 시작.
- [필수] 함수 이름 네이밍 -> 카멜 표기법 권장.
- [옵션] 매개변수 및 반환값 정의 -> 콜론(:)과 함께 자료형 반드시 명시.
- [옵션] 함수 본문 코드 작성 및 값 반환.
``` kotlin 
fun sum(a : Int, b:Int) : Int{
  var sum = a + b
  return sum
}

fun sum() {
  var sum = 10 + 20
  print(sum)
}
```

2. 함수 구조 - 간략 표기
- 매개변수를 바로 반환값에 사용할 수 있다면, 함수 내 별도의 변수 정의 코드 생략 가능.
- 본문 코드가 한 줄일 경우, 중괄호 및 return문 생략 가능.
- 본문 코드가 한 줄이고 반환값과 매개변수의 자료형이 같은 경우, 반환값 타입 생략 가능. (=추론 가능시)
``` kotlin 
fun sum(a : Int, b:Int) : Int{
  return a + b
}

fun sum(a : Int, b:Int) : Int = a + b

fun sum(a : Int, b:Int) = a + b

```

<br>

### 1-3. 함수의 호출
<details>
  <summary> [※참고] 인자와 매개변수 </summary>
  <div>
    &nbsp;&nbsp; : 매개변수와 인자는 같은 역할을 하는 것처럼 보이지만 다른 개념이다.<br> &nbsp;&nbsp;&nbsp; 함수를 선언할 때는 매개변수(parameter), 호출할 때는 인자(argument)라 한다.
  </div>
</details>  

-  정의 : 함수를 사용하는 것
 <br>

1. 함수의 호출과 프로그램 동작 순서  
i. 프로그램의 진입점 'main() 함수' 호출  
ii. 함수의 호출과 인자 사용  
iii. 함수의 호출과 매개변수 사용  
iv. 함수의 반환  

``` kotlin 
fun sum(a : Int, b:Int) : Int{
  var sum = a+b
  return sum
}

fun main(){
  val result = sum(3,2)
  print(result)
}
```

2. 함수의 호출과 메모리
``` kotlin 
fun main(){       // 최초의 스택 프레임
  val num1 = 10   // 임시 변수 혹은 지역 변수
  val num2 = 3    // 임시 변수 혹은 지역 변수
  val result : Int

  result = max(num1, num2)  // 두 번째 스택 프레임
}
```
<img src=./assets/memory_Structure.JPG width="70%" height="250px"></img><br>
- 함수의 각 정보는 프레임(Frame)이라는 정보로 스택(Stack) 메모리의 높은 주소부터 거꾸로 채워짐.
- 즉, 함수를 호출 시 위와 같은 방식으로 스택 프레임이 생성되며 소멸은 생성된 순서의 반대 순서로 소멸된다.
- 만약, 스택 프레임 생성이 많이 되어 스택 영역의 경계를 넘어서게 되면 힙 영역과 겹치게 되는데 이를 스택 오버플로우(Stack Overflow)라고 한다.

<br>

3. 함수와 그 이외의 특징들
- 반환값이 없는 함수 : Unit
``` kotlin 
fun printSum(a:Int, b:Int) : Unit{
  print("sum of $a and $b is ${a+b}")
}

fun printSum(a:Int, b:Int) {            // 추론이 가능하므로 자료형 생략 가능
  print("sum of $a and $b is ${a+b}")
}

// [※참고] Unit과 Java의 Void
// Unit은 자바의 Void와 대응한다. 하지만 Void는 정말 아무것도 반환하지 않지만 Unit은 특수한 객체를 반환한다는 차이를 가진다.

```


<br><br>

# 2. 함수형 프로그래밍
### 2-1. 다중 패러다임 언어, 코틀린
코틀린은 함수형 프로그래밍(Functional Programming)과 객체 지향 프로그래밍(Object-Oriented Programming)을 모두 지원하는 '다중 패러다임 언어'이다. 다중 패러다임 언어란 한 가지 구현 규칙에 얽매이지 않고 다양한 문법과 형식을 지원하는 언어를 말한다.  

함수형과 객체 지향의 장점은 코드를 간략하게 만들 수 있다는 것이다. 이는 대규모 프로그램 설계에도 적합하여 현대 프로그래밍 언어가 지향하는 주특징이다.  

 특히 함수형 프로그래밍은 **코드가 간략화되고 테스트나 재사용이 더 좋아져 개발 생산성이 늘어나는 장점** 때문으로 특히나 주목받고 있다. 

<br>

### 2-2. 함수형 프로그래밍
- 정의 : 순수 함수를 작성하여 프로그램의 부작용을 줄이는 프로그래밍 기법을 말한다. 
- 특징  
i. 순수 함수를 사용해야 한다.  
ii. 람다식을 사용할 수 있다.  
iii. 고차 함수를 사용할 수 있다.

<br>

- 순수 함수(Pure Function)란?  
: 같은 인자에 대해서 항상 같은 값을 반환하는 함수  
: 함수 외부의 어떤 상태도 바꾸지 않는 함수  
: 스레드에 사용해도 안전하고 코드를 테스트하기도 쉬워 '부작용이 없는 함수'라고도 부른다.
``` kotlin 
fun sum(a : Int, b:Int) : Int{
  return a+b    //동일한 인자인 a,b를 입력받아 항상 a+b를 출력(부작용이 없음)
}

fun check(){                      // [순수함수가 아닌 경우]
  val test = User.grade()        //check() 함수에 없는 외부의 User 객체를 사용
  if (test!=null) process(test) // 변수 test는 User.grade()의 실행 결과에 따라 달라짐
}
```

<br>

- 람다식(Lambda Expressions)이란?  
: 람다 대수(Lambda Calculus)에서 유래된 것으로, 아래와 같은 형태를 지니는 함수.  
: 함수의 이름이 없으며 화살표(→)가 사용되는 형태를 지닌다.  
: 수학에서의 람다 대수는 이름이 없는 함수로 2개 이상의 입력을 1개의 출력으로 단순화하는 개념이나, 프로그래밍에서 람다식은 '다른 함수의 인자로 넘기는 함수', '함수의 결괏값으로 반환하는 함수', '변수에 저장하는 함수'를 말한다.
``` kotlin 
{x, y -> x+y }                // 람다식의 예(이름이 없는 함수 형태)
```
<br>

- 일급 객체(First Class Citizen)이란?  
: 일급 객체는 함수의 인자로 전달될 수 있다.  
: 일급 객체는 함수의 반환값에 사용될 수 있다.  
: 일급 객체는 변수에 담을 수 있다.  
: 함수형 프로그래밍에선 함수를 일급 객체로 생각하며, 람다식 또한 일급 객체의 특징을 지닌다.  
: 함수가 일급 객체면 '일급 함수'라 하고, 일급 함수에 이름이 없는 경우 '람다식 함수'라 한다.

<br>

- 고차 함수(High-order Function)란?  
: 다른 함수를 인자로 사용하거나 함수를 결괏값으로 반환하고자 하는 함수를 말한다. 
: 즉, 일급 객체/함수를 주거나 받을 수 있는 함수를 말한다.
``` kotlin 
fun main(){
  print(highFunc({x, y -> x+y }, 10, 20)) //람다식 함수를 인자로 넘김
}              

fun highFunc(sum: (Int, Int) -> Int, a:Int, b:Int): Int = sum(a,b) 
//sum은 람다식 매개변수로 함수이다. 
//(이는 자료형이 람다식이므로 {x,y->x+y} 형태로 인자를 받는 것이 가능)
```
<br>


<br><br>

# 3. 고차 함수와 람다식 함수
### 3-1. 고차 함수의 형태
1. 일반 함수를 인자나 반환값으로 사용하는 고차 함수
``` kotlin 
// [1-1] 일반 함수를 인자로 사용하는 고차 함수
fun sum(a:Int, b:Int) = a+b
fun mul(a:Int, b:Int) = a*b

fun main() {
  val res1 = sum(3,2)         // 일반 인자
  val res2 = mul(sum(3,3), 3) // 함수 인자(결국 일반 인자化), 일반 인자

  print("res1=$res1, res2=$res2")
}
// 결과 : res1=5, res2=18
```
``` kotlin 
// [1-2] 일반 함수를 반환값으로 사용하는 고차 함수
fun main() {
  print("funcFunc: ${funcFunc()}")
}

fun sum(a:Int, b:Int) = a+b

fun funcFunc(): Int{  
  return sum(2,2)              // 함수의 반환값으로 함수를 사용
}
// 결과 : funcFunc: 4
```
2. 람다함수를 인자나 반환값으로 사용하는 고차 함수
``` kotlin 
// [참고] 람다 함수를 변수에 할당해보기
// [2-0-1] 람다 함수를 변수에 할당 
fun main() {
  var result : Int
  val multi = {x:Int, y:Int -> x*y} // 일반 변수에 람다식 할당
  result = multi(10,20)             // 람다식이 할당된 변수는 함수처럼 사용 가능

  print(result)
}
// 결과 : 200

// [추가 설명]
// multi 변수는 자료형이 생략되어 있으나 (Int, Int) -> Int에 의해 Int형으로 추론 가능.
// 즉, 람다식이 변수에 할당돼, 변수 이름이 multi()와 같이 함수 형태로 사용할 수 있음.
// (Ex. multi(10,20)에 전달된 10과 20은 람다식 x,y에 전달됨. 이후 화살표(→) 기호의 오른 쪽 연산 x*y에 의해 두 값을 곱하여 반환)


// [2-0-2] 람다 함수를 변수에 할당 - 람다 표현식 2줄 이상인 경우
val multi2 : (Int, Int) -> Int = { x:Int, y:Int ->
  print("x*y")
  x*y           // 마지막 표현식이 반환
}

// [2-0-3] 람다함수를 변수에 할당 - 다양한 표기법
val multi : (Int, Int) -> Int = {x:Int, y:Int -> x*y}   // 생략 없는 전체 표현식
val multi : {x:Int, y:Int -> x*y}                       // 선언 자료형 생략
val multi (Int, Int) -> Int = {x, y -> x*y}             // 람다식 매개변수 자료형의 생략
val multi : {x, y -> x*y}                               // ERROR! 자료형 추론 불가

// [2-0-4] 람다함수를 변수에 할당 - 반환 값 X, 매개변수가 한개 일 경우
val greet : () -> Unit = {print("Hello World!")}
val square : (Int) -> Int = {x -> x*x}

// [2-0-5] 람다함수를 변수에 할당 - 중첩된 경우
val nestedLambda : () -> () -> Unit = {{print("nested")}}

// [2-0-6] 람다함수를 변수에 할당 - 4,5의 표기 간략화
val greet = {print("Hello World!")}
val square = {x : Int -> x*x}
val nestedLambda = {{print("nested")}}

```
``` kotlin 
// [2-1] 람다 함수를 인자/매개변수로 사용하는 고차 함수

fun main() {
  val result:Int
  result = highOrder({x,y -> x+y}, 10, 20)  // 람다식을 매개변수와 인자로 사용한 함수
  print(result)
}

fun highOrder(sum:(Int, Int) -> Int, a:Int, b:Int):Int{
  return sum(a,b)
}
// 결과 : 30
```

### 3-2. 고차 함수 호출과 람다식
1. 값에 의한 호출
``` kotlin 
fun main() {
  val result = callByValue(lambda())    // 람다식 함수를 호출
  print(result)
}
fun callByValue(b: Boolean) : Boolean{
  print("callByValue function")
  return b
}

val lambda: () -> Boolean = {  
  print("lambda function")
  true                        // 마지막 표현식 문장의 결과가 반환
}

// 결과 : lambda function     // 즉, 즉시 호출되어 함수가 사용된 후 값 반환되더라
//        callByValue function
//        true
```
2. 이름에 의한 람다식 호출
``` kotlin 
fun main() {
  val result = callByValue(otherLambda)    // 람다식 이름으로 호출
  print(result)
}
fun callByValue(() -> Boolean) : Boolean{  // 람다식 자료형으로 선언된 매개변수
  print("callByValue function")
  return b
}

val otherLambda: () -> Boolean = {  
  print("otherLambda function")
  true                          // 마지막 표현식 문장의 결과가 반환
}

// 결과 : callByValue function  
//        otherLambda function  // 즉, 필요할 때 호출되어 함수가 사용되더라.
//        true
```

### 3-3. 고차 함수 호출과 일반함수의 람다화
``` kotlin 
fun sum(x:Int, y:Int) = x+y   // 일반 함수

funParam(3, 2, sum)           // ERROR! sum함수는 람다식 함수가 아님
funParam(3, 2, ::sum)

fun funParam(a:Int, b: Int, c:(Int, Int) -> Int) : Int{
  return c(a,b)
}
```

<br><br>

# 4. 고차 함수와 람다식 함수의 사례들
Need More Time
<br><br><br><br>

# 5. 코틀린의 다양한 함수들
코틀린에는 일반 함수, 고차 함수, 람다식 함수 이외에도 다양한 형태의 함수가 존재한다.  
예로 익명 함수, 인라인 함수, 확장 함수, 중위 함수 등이 그 예이다. 이를 알아보자.

### 5-1. 익명함수
익명함수(Anonymous Function)란 일반 함수이지만 이름이 없는 함수를 말한다.
물론 람다식 함수도 이름 없이 구성은 가능하나, 익명 함수는 일반 함수의 이름을 생략하고 사용하는 것을 뜻한다.  
기능적으로 동일하고 사용법이 유사함에도 익명함수가 필요한 이유는, 람다식에서는 return이나 break,continue처럼 제어문을 사용하기 어렵기 때문이다. 따라서 함수 본문 조건식에 따라 함수를 중단하고 반환해야 하는 경우에는 익명 함수를 활용한다.
``` kotlin 
fun(x:Int, y:Int) : Int = x+y // 함수 이름이 생략된 익명 함수 add

val add : (Int, Int) -> (Int) = fun(x,y) = x+y  // 익명함수를 사용한 add 선언
val result = add(10, 2)                         // add의 사용
```
``` kotlin 
val add = fun(x:Int, y:Int) = x+y           // 익명 함수 또한 자료형 추론이 가능하면 축약 가능.
```
``` kotlin 
val add = {x:Int, y:Int -> x+y}           // 동일한 기능하는 람다식 표현 함수
```
<br>

### 5-2. 인라인 함수
<img src=./assets/inline_Function.JPG width="90%" height="250px"></img><br>
인라인 함수(Inline Function)란 함수가 호출되는 곳에 함수 본문의 내용을 모두 복사해 넣어 함수의 분기 없이 처리되기 때문에 코드의 성능을 높이는 함수이다.  기존 함수의 경우 호출되었을 때 다른 코드로 분기해야 하기 때문에 내부적으로 기존 내용을 저장했다가 다시 돌아올 때 복구하는 작업에 CPU와 메모리 비용을 꽤 들이나, 인라인 함수는 그렇지 않기에 비용을 줄일 수 있다. 허나, 인라인 함수는 코드가 복사되어 들어가기에 내용은 대게 짧게 작성합니다.

### 5-3. 확장 함수
### 5-4. 중위 함수
### 5-5. 꼬리 재귀 함수

<br><br>

