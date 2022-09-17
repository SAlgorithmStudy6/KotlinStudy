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

## sealed 클래스
 * sealed class 키워드로 선언
 * 추상 클래스와 같음
 * private 생성자 외 허용 안됨.
 * 블록 내 클래스는 다른 파일에서 상속 불가 open 키워드 사용하여 상속 가능

 => when 문 사용하여 객체 자료형 따라 특정 기능 실행 가능 중첩 클래스 or 이너 클래스는 경우의 수를 판단할 수 없지만 sealed 클래스는 경우의 수를 직접 지정 가능.
 
 ![image](https://user-images.githubusercontent.com/53904156/190861399-2252e363-6949-4944-abb8-e0da0f7c3fba.png)


![image](https://user-images.githubusercontent.com/53904156/190861435-6f1bdf5f-30bb-4eea-9a4a-954f6dc53057.png)


![image](https://user-images.githubusercontent.com/53904156/190861490-f0285131-092f-4d61-83b9-7643ee21e8e0.png)


<hr / >

## 열거형 클래스
 여러 상수를 선언하고 사용할 수 있는 특수한 클래스
 * 열거형 이름 및 번호 등을 얻어 올 수 있다. enum.name, enum.ordinal, enum.toString() 등..


#### 선언부
```kotlin
enum class 클래스 이름 [(생성자)] {
 상수1[(값)], 상수2[값]
 [; 메소드 or 프로퍼티]
}
```
 
```kotlin
enum class ArrowKey {
 UP, DOWN, LEFT, RIGHT
}
```

#### when 사용
```kotlin
 when(ArrowKey) {
  UP -> println("up")
  down -> println("down")
  left -> println("left")
  right -> println("right")
 }
```

#### 값 여러개, 메서드 포함
![image](https://user-images.githubusercontent.com/53904156/190862476-db47b03e-3aa7-422c-a71d-945543c36d58.png)


#### 열거형 기본 제공 메소드
![image](https://user-images.githubusercontent.com/53904156/190862647-27e6a740-2d62-4a0d-b4bc-92543f259722.png)
 * name : 이름
 * ordinal : 순서번호
 * toString : 문자열 변환
 * 그냥 => 기본값(문자열)
 * values : 모든 값 반환

#### 인터페이스 사용
![image](https://user-images.githubusercontent.com/53904156/190862697-e2ef2039-9ed5-48aa-9ba2-a0147f43158a.png)

<hr />

## 애노테이션(Annotation) 클래스
 @ 기호와 함께 사용되며 컴파일러나 런타임 시 사전 처리를 위해 사용됨.
 
#### 애노테이션 선언
```kotlin
//선언 형식
annotation class 애노테이션 이름

annotation class A // 선언

...

@A class BClass{} //사용
```

#### 애노테이션 속성

![image](https://user-images.githubusercontent.com/53904156/190862981-f7a1c527-18ea-4538-a1e5-ee173a8f060a.png)

 * @Retention을 Source 사용시 컴파일 시 제거. Binary 사용시 클래스 포함되지만 리플렉션에 의해 나타나지 않음. Runtime 사용시 클래스 파일에 저장 및 리플렉션에 의해 나타남
 * 리플렉션 : 프로그램 실행 시 특정 구조 분석해내는 기법. 

![image](https://user-images.githubusercontent.com/53904156/190863253-881e00e9-e0a8-405e-810b-40a0a05d4498.png)


#### 애노테이션 위치
```kotlin
@Anno class MClass {
 @Anno fun method(@Anno property: Any): Any {
  return (@Anno 1)
 }
}
```
 * 클래스 앞
 * 메서드 앞
 * 매개변수 앞
 * 반환 값 앞 => 소괄호로 감싸야 함
 * 생성자 앞 => constructor 생략 불가. ex) class A @Anno constructor (p: Any){}

#### 애노테이션의 매개변수 및 생성자
```kotlin
annotation class Anno(val a: String)

@Anno("ex") class A {}
```

##### 매개변수 사용가능한 자료형
 * 자바의 기본형과 연동하는 자료형
 * 열거형
 * 문자열
 * 기타 애노테이션
 * 클래스
 * 위 목록의 배열



