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


