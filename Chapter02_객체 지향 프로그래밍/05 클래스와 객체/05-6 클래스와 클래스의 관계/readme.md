# 05-6 클래스와 클래스의 관계

- 클래스 혹은 객체 간의 관계는 연관, 의존, 집합, 구성이 있음.
- '두 클래스가 서로를 참조하는지'를 먼저 확인하고, 그 다음 '두 클래스가 생명주기에 영향을 주는지'를 나눔
   <img width="300" alt="화면 캡처 2022-09-17 172054" src="https://user-images.githubusercontent.com/43957736/190847591-99567a13-d4ee-4466-afcc-f704877fb926.png">
    <img width="300" alt="화면 캡처 2022-09-17 172906" src="https://user-images.githubusercontent.com/43957736/190847894-c046e541-cd7e-4371-9260-62a11b8cca24.png">
 
- 연관 관계
  - **2개의 서로 분리된 클래스가 연결을 가지는 것**
  - 단방향 혹은 양방향으로 연결될 수 있음 
  - 두 요소는 서로 다른 생명주기를 갖고 있음
    ```kotlin
    class patient(val name: String){
      fun doctorList(d: Doctor){
        println("Patient : $name, Doctor : ${d.name}")
      }
    }
    
    class Doctor(val name: String) {
      fun parientList(p: Patient) {
        println("Doctor: $name, Patient: ${p.name}")
      }
    }
  ```
 - Doctor와 Patient 클래스는 따로 생성되며 독립적인 생명주기를 가짐
 - **서로의 객체를 참조하고 있지만 생명주기에 영향을 주지 않으면 연관관계라고 함**

- 의존 관계
  - 한 클래스가 다른 클래스에 의존되어 영향을 주는 경우
    ```kotlin
    class patient(val name: String){
      fun doctorList(d: Doctor){
        println("Patient : $name, Doctor : ${d.name}")
      }
    }
    
    class Doctor(val name: String, val p: Patient) {  //Doctor 클래스를 생성하려면 Patient 클래스가 필요함 -> 서로 영향을 줌
      fun parientList(p: Patient) {
        println("Doctor: $name, Patient: ${p.name}")
      }
    }
  ```
  
- 집합 관계
  - 연관 관계와 거의 동일하지만 특정 객체를  **소유** 한다는 개념이 추가됨
    ```kotlin
    class Pond(_name: String, _members: MutableList<Duck>){
      val name: String = _name
      val members: MutableList<Duck> = _members
      constructor(_name: String): this(_name, mutableListOf<Duck>()}
    }
    
    class Duck(val name: String) {
      // 두 객체가 서로의 생명주기에 영향을 주지는 않음
      val pond = Pond("myFavor")
      val duck1 = Duck("Duck1")
      val duck2 = Duck("Duck2")
      
      pond.members.add(duck1)
      pond.members.add(duck2)
    }
  ```
  
- 구성 관계
  - 집합 관계와 거의 동일하지만 **특정 클래스가 어느 한 클래스의 부분이 되는 개념**
    ```kotlin
    class Car(val name: String, val power: String) {
      private var engine = Engine(power)
      
      fun startEnfine() = engine.start()
    }
    
    class Engine(power: String) {
      fun start() = println("Engine start")
    }

