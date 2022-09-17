# Chapter07-1 추상 클래스와 인터페이스
* 발표일 : 2022-09-18
* 발표자 : 박채성   
<br/>

## 추상 클래스

**추상**
* '구체적이지 않은 것'. 필요한 기능이나 요소가 정의 되지 않아 이후에 추가적으로 구현할 필요가 있는 붙는 접두사<br>
* **abstract** 키워드를 통해 사용할 수 있다. <br>
  ex : 추상 클래스, 추상 프로퍼티, 추상 메소드 <br>

 **추상 클래스** : 하나 이상의 추상 메서드나 추상 프로퍼티를 가지는 클래스. 
  * 일반적 객체 생성 방식으로 Instance화 불가.<br>
  * 추상 프로퍼티와 추상 메소드는 이후 추상 클래스를 상속받는 하위 클래스에서 구체화 해주어야 함 <br>
  * 추상 프로퍼티와 추상 메소드는 open 키워드 없더라도 오버라이딩 가능.

 **추상 프로퍼티** : abstract 키워드를 사용하여 선언된 프로퍼티. 하위 클래스에서 구체화 필요 <br>
 **추상 메소드** : abstract 키워드를 사용하여 선언된 메서드. 하위 클래스에서 구체화 필요 <br><br>

```kotlin
abstract class Animal() { 
  // 상속받는 하위 클래스에서 추상메소드 eat, stop, 추상 프로퍼티 legsNum 구체화 필요.
  var hasWings = false;
  abstract var legsNum: Int

  abstract fun eat()
}

//Animal 하위 클래스들. 추상 메서드 eat, 추상 프로퍼티 legsNum 각각 override 하여 구체화.
class Rat(override var legsNum: Int) : Animal() {
  override fun eat() {
    println("eat grain")
  }
}
lass Cat(override var legsNum: Int) : Animal() {
 over ride fun eat() {
    println("eat Rat")
  }
}

```
<br>

> #### **추상클래스의 객체 인스턴스** <br>
> 추상 클래스의 하위 클래스를 생성하지 않고 단일 인스턴스로 객체를 생성할 경우 **object** 를 사용하여 지정할 수 있다. 

<br>

```kotlin
val unicorn = object: Animal() {
  override var legsNum: Int = 4
  override fun eat() {
    println("eat Grass")
}
```
   
## 인터페이스

 추상적인 활동이 적혀있는 일종의 설계도. 

### 인터페이스 장점
 > 구현에 의존적이지 않은 코드를 만들 수 있어 확장성이 좋음.

#### 추상 클래스와 차이
 * 프로퍼티를 통해 상태 저장 불가 => 단 val 선언 프로퍼티는 게터로 구현 가능 <br>
 * 다중 implements 가능 <br>
 * 클래스 상속보다 연관성이 약함 <br>
 * 추상 메소드, 추상 프로퍼티 선언시 abstract 사용하지 않아도 됨 <br>

<br>

 **자바 인터페이스 default 메소드와 코틀린**<br>
 자바 8 이후의 인터페이스는 default 키워드를 사용하여 인터페이스 내부에서 메소드 구현 내용을 넣을 수 있습니다. 코틀린은 이를 기본으로 제공하기 때문에 default 키워드가 필요하지 않습니다.
 
 > 코틀린에서는 클래스의 상속과 인터페이스의 구현을 모두 콜론(:)으로 정의함. 
 
 <br>
 
```
interface 인터페이스 이름 [: implements할 인터페이스 이름...] {
  추상 프로퍼티 선언
  추상 메소드 선언
  
  [일반 메소드 선언 및 구현 {}]
}
```

<br>

```kotlin
interface Animal { 
  // 상속받는 하위 클래스에서 추상메소드 eat, 추상 프로퍼티 legsNum 구체화 필요.
  
  //인터페이스는 일반 프로퍼티를 가질 수 없음.
  //var hasWings = false;
  val hasWings: Boolean
    get() = false;
  abstract var legsNum: Int

  abstract fun eat()
}

//Animal 하위 클래스들. 추상 메서드 eat, 추상 프로퍼티 legsNum 각각 override 하여 구체화.
class Rat(override var legsNum: Int) : Animal() {
  override fun eat() {
    println("eat grain")
  }
}
```

<br>

 > 인터페이스를 적절히 implements 하여 클래스의 독립성을 확보 할 수 있음.

<br>

#### 인터페이스 위임
인터페이스 또한 by 위임자를 사용할 수 있다.

```kotlin
interface A {
  fun functionA(){}
}

interface B {
  fun functionB(){}
}

//위임 사용 전 코드
class C(val a : A, val b : B) {
  fun functionC(){
    a.functionA()
    b.functionB()
  }
}
```

<br>

```kotlin
class C(a: A, b: B) A by a, B by b {
  fun functionC() {
    functionA()
    functionB()
  }
}
```
<br>

![image](https://user-images.githubusercontent.com/53904156/190856244-55bf623d-fb56-4609-b660-8b8bad5d98d9.png)

1. 매개변수에 해당 인터페이스 위임
2. 클래스 생성자 통해 객체 전달
3. 3번과 4번처럼 각 클래스 위임된 멤버에 접근

