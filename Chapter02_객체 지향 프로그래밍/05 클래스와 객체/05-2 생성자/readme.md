# 05-2 생성자

- 생성자는 주생성자와 부생성자로 나뉨
- 부 생성자 -> 매개변수를 다르게 여러번 정의 가능
- kotlin에서 부생성자는 **constructor 키워드를 사용**
    ```kotlin
    class Bird{
      //프로퍼티(속성)
      var name: String
      var wing: Int
      var beak: String
      var color: String
      
      //부생성자를 선언하고 매개변수를 통해 초기화할 프로퍼티에 지정
      constructor(name: String, wing: Int, beak: String, color: String) {
        this.name = name
        this.wing = wing
        this.beak = beak
        this.color = color
      }
      
      //여러개의 부생성자를 선언할 수 있음
      //this 키워드를 사용하지 않으려면 언더스코어(_) 사용
      constructor(_name: String, _wing: Int, _beak: String, _color: String) {
        name = _name
        wing = _wing
        beak = _beak
        color = _color
      }
      
      fun fly() = println("Fly wing: $wing")
      fun sing(vol: Int) = println("Sing vol: $vol")
    }
    
    fun main() { 
      val coco = Bird("MyBird",2,"short", "blue") //객체 생성과 동시에 초기화
      coco.color = "yellow" //객체의 프로퍼티에 값 할당
      println("coco.color: ${coco.color}"}  // coco.color: yellow
      coco.fly()  // Fly wing: 2
      coco.sing(3)  //Sing vol: 3
    }
    ```
    
- 주 생성자 : 클래스 이름과 블록 시작 부분 사이에 선언
  ```kotlin
  // 주생성자는 contructor 키워드 생략 가능(생략해도 사용법은 동일)
  class Bird constructor(_name: String, _wing: Int, _beak: String, _color: String) {
        name = _name
        wing = _wing
        beak = _beak
        color = _color
        
        fun fly() = println("Fly wing: $wing")
        fun sing(vol: Int) = println("Sing vol: $vol")
   }
   ```
   
  - 내부 프로퍼티를 생략하고 주 생성자의 매개변수에 프로퍼티를 할당하면 코드가 간결해짐
    ```kotlin
      class Bird constructor(var name: String, var wing: Int, var beak: String, var color: String) {
      
        fun fly() = println("Fly wing: $wing")
        fun sing(vol: Int) = println("Sing vol: $vol")
    }
   
  - 초기화 블록 사용 : 코드가 간결해짐, 반드시 클래스 선언부에 넣어주어야 함
  - 생성자 매개변수에 기본값을 사용하면 객체를 생성할 때 기본값이 있는 인자는 생략 가능
    ```kotlin
    class Bird constructor(var name: String = "PAEHA", var wing: Int = 2, var beak: String, var color: String) {
        init {
        println("초기화 블록입니다")
        this.sing(3)
        }
        
        fun fly() = println("Fly wing: $wing")
        fun sing(vol: Int) = println("Sing vol: $vol")
      }
   
      fun main() { 
        val coco = Bird(beak = "long", color="red") // 기본값으로 지정해준 것은 생략
      
        println("coco.color: ${coco.color}"}  // coco.color: yellow
      }
     ```
