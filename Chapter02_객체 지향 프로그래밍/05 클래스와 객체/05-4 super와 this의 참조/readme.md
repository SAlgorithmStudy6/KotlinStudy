# 05-4 super와 this의 참조

- super
  - 상위 클래스를 가리키는 특별한 키워드로, 상위 클래스의 프로퍼티, 메소드, 생성자 등을 사용할 수 있다
  ```kotlin
  open class Bird(var name: String, var wing: Int, var beak: String, var color: String) {
     fun fly() = println("Fly wing: $wing")
     open fun sing(vol: Int) = println("Sing vol: $vol")
  }

  class Parrot(name: String, wing: Int=2, beak: String, color: String, var language: String = "natural") : Bird(name, wing, beak, color) {
    override fun sing(vol: Int) {
      super.sing(vol) //상위 클래스의 sing()을 수행
    }
  }
  ```


- this
  - 현재 객체를 참조하는 키워드
  ```kotlin
  open class Person {
    constructor(firstNAme: String) {
      println("[Person] firstName: $firstNAme")
    }
    constructor(firstName: String, age: Int) {
      println("[Person] firstName: $firstName, $age")
    }
  }
  
  class Developer: Person {
    constructor(firstName: String): this(firstName, 10) { //인자가 2개인 아래 부생성자를 호출
      println("[Devloper] $firstNAme")
    }
    constructor(firstName: String, age: Int): super(firstName, age) { //인자가 2개인 부모 클래스의 생성자를 호출
      println("[Developer] $firstName, $age")
    }
  }
  
  fun main() {
    val sean = Developer("Sean")
  }
  ```


- inner class(바깥 클래스 호출하기)
  - 클래스 안에 선언된 클래스
  - inner class에서 바깥 클래스의 상위 클래스를 호출하려면 super 키워드와 함께 @기호 옆에 바깥 클래스 이름을 사용해야함
    ```kotlin
    open class Parents {
      open val x: Int = 1
      open fun f() = println("Parents Class f()")
    }
    
    class Child : Parents() {
      override val x: Int = super.x + 1
      override fun f() = println("Child Class f()")
      
      inner class Inside {
        fun f() = println("Inside Class f()")
        fun test() {
          f()
          child().f()
          super@Child.f()
          println("[Inside] x: ${super@Child.x}")
        }
      }
    }
    fun main() {
      val c1 = Child()
      // Child 클래스 안에 있는 Inside클래스 이므로
      c1.Inside().test()  // Inside Class f()
                          // Child Class f()
                          // Parents Class f()
                          // [Inside] x: 1
    }
    ```


- 인터페이스(인터페이스에서 참조하기)
  - 코틀린은 자바와 달리 한 번에 2개 이상의 클래스를 상속 받는 다중 상속이 불가능
  - 하지만 인터페이스는 다수의 인터페이스를 지정해 구현할 수 있음
  - 상속 받는 클래스 혹은 인터페이스의 프로퍼티나 메소드 이름이 중복되면 앵클 브래킷( <> )을 활용하여 접근할 수 있음
  ```kotlin
  open class A {
    open fun f() = println("A class f()")
    fun a() = println("A Class a()")
  }
  
  interface B {
    fun f() = println("B interface f()")
    fun b() = println("B interface b()")
  }
  
  class C : A(), B {
    override fun f() = println("C Class f()")
    
    fun test() {
      f()
      b()
      super<A>.f()
      super<B>.f()
    }
  }
  
  fun main() {
    val.c = C()
    c.test()  // C Class f()
              // B Interface b()
              // A Class f()
              // B Interface f()
  }
  ```
 
