# Chapter07-3 연산자 오버로딩
* 발표일 : 2022-09-18
* 발표자 : 박채성   
<br/>

##### 연산자 우선순위

![image](https://user-images.githubusercontent.com/53904156/190867909-d98131bf-43cd-4670-ac47-64a95d77b954.png)

<br>

##### 연산자 동작 방식
연산자 사용은 멤버 메서드 호출하는 것과 유사함. a + b => a.plus(b) 호출 <br>

<br>

###### 다양한 오버로딩된 연산자
![image](https://user-images.githubusercontent.com/53904156/190868005-37ac8aba-0214-44b4-bad6-c34800907231.png)

#### 연산자 오버로딩 키워드 : **operator**
operator를 사용하여 연산자를 오버로딩 할 수 있습니다.<br><br>


plus 연산자 오버로딩 예제

![image](https://user-images.githubusercontent.com/53904156/190868231-df2fc5c4-b27a-4d81-9588-ba78209f7a4b.png)


## 연산자의 종류

#### 산술 연산자
![image](https://user-images.githubusercontent.com/53904156/190868279-038bbdd4-f53c-417f-859b-3064a225cac3.png)


#### 호출 연산자
함수 호출을 돕는데 사용, 람다식에는 기본적으로 정의됨 <br>

```kotlin
class A {
  operator fun invoke(v: String) {
    println(v)
  }
}

fun main() {
  val a = A();
  a("Hello world!") //Hello world! 출력, invoke가 생략됨
}
```

#### 인덱스 접근 연산자 (Indexed Access Operator) => 게터 / 세터 다루기 위한 [] 연산자 제공 <br>

![image](https://user-images.githubusercontent.com/53904156/190868481-085040cd-b094-4d41-8fb0-2a825f5070a2.png)

#### 단일 연산자

![image](https://user-images.githubusercontent.com/53904156/190868524-3cad1092-c1bf-4be8-a9b8-0c5f53100c9e.png)


```kotlin
data class Point(val x: Int, val y : Int, val z: Int)

operator fun Point.unaryMinus( ) = Point(-x, -y, -z)

val p = Point(10, 10, 10)
println(-p)
```

#### 대입 연산자
연산 결과 할당하는 연산자 <br>
 * plus를 오버로딩하면 +=는 자동 구현됨. 이때 plusAssign도 오버로딩시 오류 발생.
 
 ![image](https://user-images.githubusercontent.com/53904156/190868678-8b0f589f-b1d7-440c-b7c0-43fd84d314fc.png)


#### 동등성 연산자
일치, 불일치 연산자 <br>
 * 인자가 null 이어도 동작하여 둘 다 null일 경우 true 반환

![image](https://user-images.githubusercontent.com/53904156/190868787-052cdae8-8c2a-451e-9cd6-3f2282948576.png)


#### 비교 연산자
compareTo를 호출해 반환되는 정수를 기준으로 비교.

![image](https://user-images.githubusercontent.com/53904156/190868857-84f7f75f-d583-45c4-ab0f-5981ba3ef142.png)
