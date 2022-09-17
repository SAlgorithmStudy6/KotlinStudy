# 05-3 상속과 다형성

- 상속 : 상위 클래스의 속성과 기능을 자식 클래스가 물려받아 사용
- 상속을 통해 하위클래스는 **상위 클래스의 내용을 다시 만들지 않아도 됨**
- 부모 클래스에서 *open* 키워드를 사용해야 하위클래스에서 상속받을 수 있음
  ```kotlin
  val someVal: Int
  open class Bird(var name: String, var wing: Int, var beak: String, var color: String) {
    fun fly() = println("Fly wing: $wing")
  }
  class Parrot : Bird {
    val language: String
    
    constructor(name: String, wing: Int, beak: String, color: String, language: String) : 
      super(name, wing, beak, color){ //상속 받은 프로퍼티
        this.language = language
      }
  }
  ```
  - 클래스를 상속 받을 때 클래스명과 : 을 띄어서 씀. 붙여도 문제는 없지만 *선언과 클래스 선언을 구분하기 위한 일종의 관례*
- 오버라이딩 : 상위 클래스의 메소드의 내용을 새로 만들어 다른 기능을 수행하게 함
  ```kotlin
  open class Bird {
  /* 중략 */
    fun fly() { /*중략*/ }
    open fun sing() { /*중략*/ }
  }
  
  class Lark() : Bird() {
    fun fly(){ /*재정의*/ }  //오류 발생!!!!! 오버라이딩 또한 부모클래스에서 open키워드를 사용한 메소드만 가능
    override fun sing() { /*재정의*/ }
  }
  ```
- 다형성 : 매소드의 이름은 같으나 구현 내용 혹은 매개변수를 달리 생성하여 하나의 이름으로 다양한 기능을 수행
- 오버로딩 : 다형성을 활용하여 동일한 클래스 안에서 동일한 이름의 메소드로 매개변수만 달리하여 여러번 정의될 수 있는 개념
   ```kotlin
    fun add(x: Int, y: Int): Int { return x+y }
    fun add(x: Double, yL Double): Double{ return x+y }
   ```
