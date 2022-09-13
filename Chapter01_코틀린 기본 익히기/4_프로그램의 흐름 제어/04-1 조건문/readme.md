# 04-1 조건문

- `if`문과 `if ~ else`문
    - 조건식에는 Boolean 자료형으로 true, false 값만 받을 수 있는 조건식을 구성해야 함
    
    ```kotlin
    if (조건식) { // 조건식이 true인 경우만 수행
    	수행할 문장 
    	...
    }
    ```
    
- 블록의 표현식이 길어질 때
    
    ```kotlin
    fun main(){
    	val a = 12
    	val b = 7
    
    	val max = if(a > b){
    		println("a 선택")
    		a // 마지막 식인 a가 반환되어 max에 할당
    	}else{
    		println("b 선택")
    		b // 마지막 식인 b가 반환되어 max에 할당
    	}
    
    	println(max) // 12
    }
    		
    ```
    
- in 연산자와 범위 연산자로 조건식 간략하게 만들기
    - in 연산자 : `변수 이름 in 시작값..마지막값` , 점 두 개 !!!
    
    ```kotlin
    if (score >= 80 && score <= 89.9)
    
    if (score in 80.0..89.9)
    ```
    
- 인자를 사용하는 `when`문 (switch/case)
    
    ```kotlin
    when(x){
    	1 -> print("x == 1")
    	2 -> print("x == 2")
    	else -> { // 중괄호 가능
    		print("x는 1,2가 아닙니다")
    	}
    }
    
    when(x){
    	0, 1 -> print("x == 0 or x == 1")
    	else -> print("기타")
    }
    ```
    
- when문에 함수의 반환값 사용하기
    
    ```kotlin
    when(x) {
    	parseInt(s) -> print("일치함")
    	else -> print("기타")
    }
    ```
    
    - parseInt( )함수는 인자로 지정된 s를 정수형으로 변환해줌
    - x값과 parseInt(s)값이 일치하면 print(”일치함”)을 수행
- when문에 in 연산자와 범위 지정자 사용하기
    
    ```kotlin
    when (x){
    	in 1..10 -> print("1 <= x <= 10")
    	!in 10..20 -> print(" ! (10 <= x <= 20)")
    	else -> print ("범위에 없습니다.")
    }
    ```
    
    - `! in 10..20` 은 ”10보다 작거나 20보다 크다” 와 같음
- when 과 is 키워드 함께 사용하기
    
    ```kotlin
    val str = "안녕하세요"
    val result = when(str){
    	is String -> "문자열"
    	else -> false
    }
    ```
    
    - 변수 str의 자료형이 String인 경우 “문자열”을 result에 할당함
- 인자가 없는 when문
    
    ```kotlin
    fun main(){
    	var score = readLine()!!.toDouble()
    	var grade : Char = 'F'
    
    	when {
    		score >= 90.0 -> grade = 'A'
    		score in 80.0 .. 89.9 -> grade = 'B'
    		score in 70.0 .. 79.9 -> grade = 'C'
    		else -> grade = 'F'
    	}
    
    	println(grade)
    }
    ```
    
- 다양한 자료형의 인자 받기
    
    ```kotlin
    fun main(){
    	cases("Hello") // String형
    	cases(1) // Int형
    	cases(System.currentTimeMillis()) // 현재 시간을 Long형 값으로 반환
    	cases(MyClass()) // 객체
    }
    
    fun cases(obj : Any){
    	when(obj){
    		1 -> println("Int : $obj")
    		"Hello" -> println("String : $obj")
    		!is String -> println("Not a String")
    		else -> println("Unknown")
    	}
    }
    
    // String : Hello
    // Int : 1
    // Long : 1234
    // Not a String
    ```
