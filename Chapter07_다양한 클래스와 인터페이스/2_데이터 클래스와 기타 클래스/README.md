# Chapter07-2 데이터 클래스와 기타 클래스
발표일 : 2022-09-18 <br>
발표자 : 박채성 <br>

 **DTO(Data Transfer Object)** : 데이터 전달을 위한 객체
  * 속성과 속성 접근에 필요한 getter/setter, toString()과 같은 표현 메소드, 및 비교 메소드를 가짐.

## 데이터 클래스
데이터 저장에 중점을 두어 고안된 코틀린의 특별한 클래스. DTO를 저장하기 용이. <br><br>

아래 메소드 자동 생성
 1. 프로퍼티 게터 / 세터
 2. equals() : 내용 같은지 비교
 3. hashCode() : 객체 고유값 생성
 4. toString() : 문자열 반환
 5. copy() : 특정 프로퍼티 변경해 객체 복사 ex) student.copy(name"홍길동")
 6. componentsN() : 객체 선언부 분해
 
 <hr/>
 
 ### 객체 디스트럭처링(Destructuring) <br>
 객체 선언부 분해하여 변수 선언 시 객체 지정
 * _ 사용하여 필요한 부분만 가져올 수 있음. ex) val (_, b1) = A
 
```kotlin
//대상 객체
val temp = A(a, b)

//디스트럭처링
val (a1, b1) = temp
//_ 사용으로 필요한 부분만 가져오기
val (_, b2) = temp
//componentN 사용
val a3 = temp.component1()
val b3 = temp.component2()
```
 
#### 함수 객체 반환시 사용
```kotlin
fun f(): A {
  return A(a, b)
}

val (a1, b1) = f();
```

#### 람다 식 사용
```kotlin
val l = {
  (a, b): A ->
  //람다식 내용
  ...
}
l(A(a, b))
```

<hr/>

## 내부 클래스 (Nested Class / Inner Class)

**내부 클래스 사용하는 경우** : 독립적으로 만들기 애매한 경우 or 다른 클래스에서 사용 안할 경우

**내부 클래스 주의할 점** : 의존성을 증가시키고 가독성이 떨어질 수 있음 <br>

 **자바 내부 클래스 종류**
 1. 정적 클래스(static class) : 인스턴스화 필요 x
 2. 멤버 클래스(member class) : 외부 클래스 필드 or 메서드와 연동
 3. 지역 클래스(local class) : 초기화 블록 or 메소드 내에서만 유효
 4. 익명 클래스(annonymous class) : 일회용 객체 인스턴스화해 사용됨
 
 **코틀린 내부 클래스**
 1. 중첩 클래스 : 객체 생성 없이 사용
 2. 이너 클래스 : 필드나 메서드와 연동. inner 키워드 필요
 3. 지역 클래스 : 블록 내에 클래스 선언부 존재, 외부 클래스 접근 가능
 4. 익명 객체 : object 키워드 사용해 선언, 인터페이스 구현체로 사용됨
 
 #### 자바 멤버 클래스 | 코틀린 내부 클래스
 ```kotlin
 //자바
 class A {
  class B {
    //외부 클래스 A 필드 및 메소드 접근 가능
  }
 }
 
 //코틀린
 class A{
  inner class B {
    //외부 클래스 A 필드 및 메소드 접근 가능
  }
 }
 ```
 
 
 ![image](https://user-images.githubusercontent.com/53904156/190860415-eeffd4f2-0f3d-4ddf-8232-28dbf2fb1627.png)

 
 #### 자바 정적 클래스 | 코틀린 중첩(Nested) 클래스
 ```kotlin
 //자바 정적 클래스
 class A {
  static class B { //static 키워드 사용
    ... //외부 클래스 A의 필드 및 메소드 접근 불가
  }
 }
 
 //코틀린
 class A {
  class B { //코틀린은 키워드가 없는 경우 중첩 클래스. => 자바의 정적 클래스처럼 사용됨
    ... //외부 클래스 A의 필드 및 메소드 접근 불가
  }
 }
 ```
 
 
 ![image](https://user-images.githubusercontent.com/53904156/190860390-eb0bc3f7-b9f5-4b73-b6dc-2c264e7b482f.png)
 
 ![image](https://user-images.githubusercontent.com/53904156/190860527-e40297da-bda9-4cc5-b450-c5dc307d651b.png)

 -> 중첩 클래스에서 바깥 클래스의 프로퍼티 및 메서드 접근 가능 이유 : 컴패니언 객체로 선언되었기 때문

 #### 익명객체
  인터페이스를 상속받는 일회성 객체 생성하고 사용할 경우 이용됨.
 
```kotlin
val unicorn = object: Animal() {
  override var legsNum: Int = 4
  override fun eat() {
    println("eat Grass")
}
```

<hr / >

