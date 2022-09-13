# 04-3 흐름의 중단과 반환

- 흐름 제어문
    - return : 함수에서 결과값을 반환하거나 지정된 라벨로 이동
    - break : for문이나 while문의 조건식에 상관없이 반복문을 끝낸다.
    - continue : for문이나 while문의 본문을 모두 수행하지 않고 다시 조건식으로 넘어간다.
- 예외 처리문
    - try {…} catch {…} : try 블록의 본문을 수행하는 도중 예외가 발생하면 catch 블록의 본문을 실행한다.
    - try {…} catch {…} finally {…} : 예외가 발생해도 finally 블록 본문은 항상 실행한다.
- return문
    
    ```kotlin
    fun add(a : Int, b : Int) : Int {
    		return a + b
    }
    ```
    
    - return으로 Unit 반환하기
        - 코틀린에서 Unit는 void와 비슷하다고 했지만
        - Unit이란 반환하는 값이 아예 없는 뜻이 아니라 Unit 자료형 자체를 반환한다는 말
        
        ```kotlin
        // 1. Unit을 명시적으로 반환
        fun hello(name : String) : Unit{
        		println(name)
        		return Unit
        }
        
        // 2. Unit 이름을 생략한 반환
        fun hello(name : String) : Unit {
        		println(name)
        		return
        }
        
        // 3. return문 자체를 생략
        fun hello(name : String) {
        		println(name)
        }
        
        // Unit 생략 가능
        fun hello(name : String) {
            println(name)
        }
        ```
        
    
    - 람다식에서 return 사용하기
        - 인라인으로 선언되지 않은 람다식에서는 return을 그냥 사용할 수 없음
        - return@label과 같이 라벨 표기와 함께 사용해야 함
        - 라벨이란 코드에서 특정한 위치를 임의로 표시한 것으로 @기호와 이름을 붙여 사용
        - 인라인으로 선언된 함수에서 람다식을 매개변수로 사용하면 람다식에서 return을 사용할 수 있다.
        
        ```kotlin
        fun main(){
        		retFunc( )
        }
        
        inline fun inlineLambda(a : Int, b : Int, out : (Int, Int) -> Uint){
        		out (a, b)
        }
        
        fun retFunc( ){
        		println("start of retFunc")
        		inlineLambda(13, 3) { a, b ->
        				val result = a + b
        				if (result > 10) return // 10보다 크면 이 함수를 빠져나감
        				println("result : $result")
        		}
        		println("end of retFunc")
        }
        
        // 실행 결과 
        // start of retFunc
        ```
        
    
    - 람다식에서 라벨과 함께 return 사용하기
        
        ```kotlin
        람다식 함수 이름 라벨 이름@{
        	...
        return@라벨 이름
        }
        ```
        
        ```kotlin
        fun main() {
        	retFunc()
        }
        
        fun inlineLambda(a : Int, b : Int, out : (Int, Int) -> Unit){  // inline을 제거
        	out(a , b)
        }
        
        fun retFunc() {
        	println("start of retFunc")
        	inlineLambda(13,3) lit@{ a,b -> // 1. 람다식 블록의 시작부분에 라벨을 지정
        		val result = a + b
        		if (result > 10) return@lit // 2. 라벨을 사용한 블록의 끝부분으로 반환
        		println("result : $result")
        	} // 3. 이 부분으로 빠져나감
        	println("end of retFunc") // 4. 이 부분이 실행
        }
        
        // 실행 결과
        // start of retFunc
        // end of retFunc
        ```
        
        - inlineLambda( )함수는 코드 앞에 inline을 삭제 했으므로 인라인 함수가 아님
        - inline을 지우면 return에 오류가 표시됨
        - 1처럼 람다식 블록 앞에 라벨(lit@)을 정의한다.
        - 2처럼 return에 @로 시작하는 라벨(@lit)을 붙여줌
        - 라벨 이름은 내맘대로!
        
    - 암묵적 라벨
        - 람다식 표현식 블록에 직접 라벨을 쓰는게 아니라 람다식 명칭을 그대로 라벨처럼 사용하는 것
        
        ```kotlin
        fun retFunc( ){
        		println("start of retFunc")
        		inlineLambda(13, 3) { a, b -> 
        				val result = a + b
        				if (result > 10) return@inlineLambda
        				println("result : $result")
        		}
        		println("end of retFunc")
        }
        ```
        
    
    - 익명 함수를 사용한 반환
        - 람다식 대신 익명 함수를 넣을 수도 있음
        - 라벨을 사용하지 않고도 가까운 익명 함수 자체가 반환되어 동일한 결과
        - 보통의 경우 람다식을 사용하고, return과 같이 명시적으로 반환해야 할 것이 여러개라면 익명함수를 쓰는 것이 좋음
        
        ```kotlin
        fun retFunc( ){
        		println("start of retFunc")
        		inlineLambda(13, 3) { a, b , fun (a, b) {
        				val result = a + b
        				if (result > 10) return@inlineLambda
        				println("result : $result")
        		}) // inlineLambda()의 끝부분
        		println("end of retFunc")
        }
        ```
        
        ```kotlin
        // 람다식 방법, 익명함수 방법 비교
        // 람다식 방법
        val getMessage = lambda@ { num  : Int ->
        		if (num !in 1..100){
        				return@lambda "Error" // 라벨을 통한 반환
        		}
        		"Suceess" // 마지막 식이 반환
        }
        
        // 익명 함수 방법
        val getMessage = fun(num : Int) : String {
        		if (num !in 1..100){
        				return "Error"
        		}
        		return "Success"
        }
        ```
        
    
    - 람다식과 익명함수를 함수에 할당할 때 주의할 점
        
        ```kotlin
        fun greet( ) = { println("Hello") }
        ```
        
        - greet( ) 함수를 실행하면 아무것도 나오지 않음
        - 할당 연산자에 의해 greet ( ) 함수에 { println(”Hello”)}가 할당 된 것 뿐이니까
        - greet( ) ( ) 해야 함수가 실행됨
        
        ```kotlin
        fun greet( ) = fun ( ) {pritln("Hello")}
        ```
        
        - 함수가 할당됨을 명시적으로 표현하려면 익명함수를 써서 선언하는 게 읽기는 더 좋음
- break와 continue문
    - break와 continue에 라벨 함께 사용하기
    
    ```kotlin
    fun labelBreak( ){
    		println("labelBreak")
    		for(i in 1..5){
    				second@ for (j in 1..5) {
    						if (j == 3) break
    						println("i : $i, j : $j")
    				}
    				println("after for j")
    		}
    		println("after for i")
    }
    
    /* 실행 결과
    labelBreak
    i : 1, j : 1
    i : 1, j : 2
    after for j
    i : 2, j : 1
    i : 2, j : 2
    after for j
    i : 3, j : 1
    i : 3, j : 2
    after for j
    i : 4, j : 1
    i : 4, j : 2
    after for j
    i : 5, j : 1
    i : 5, j : 2
    after for j
    after for i
    */
    
    // 첫번째 for문 바로 탈출하기
    fun labelBreak( ){
        println("labelBreak")
        first@ for(i in 1..5){
            second@ for(j in 1..5){
                if (j == 3) break@first
                println("i : $i , j : $j")
            }
            println("after for j")
        }
        println("after for i")
    }
    
    /* 실행 결과
    labelBrak
    i : 1 , j : 1
    i : 1 , j : 2
    after for i
    */
    
    // break@first를 continue@first로 바꾸면?
    // 라벨이 지정된 위치에서 첫번째 라벨의 for문이 실행됨
    fun labelBreak( ){
        println("labelContinue")
        first@ for(i in 1..5){
            second@ for(j in 1..5){
                if (j == 3) continue@first
                println("i : $i , j : $j")
            }
            println("after for j")
        }
        println("after for i")
    }
    
    /* 실행 결과
    labelBrak
    i : 1 , j : 1
    i : 1 , j : 2
    i : 2 , j : 1
    i : 2 , j : 2
    i : 3 , j : 1
    i : 3 , j : 2
    i : 4 , j : 1
    i : 4 , j : 2
    i : 5 , j : 1
    i : 5 , j : 2
    after for i
    */
    ```
    
- 예외 처리
    - 예외를 발생 시키는 상황
        1. 운영체제의 문제( 잘못된 시스템 호출의 문제)
        2. 입력 값의 문제( 존재하지 않는 파일 또는 숫자 입력란에 문자 입력 등)
        3. 받아들일 수 없는 연산 ( 0으로 나누기)
        4. 메모리의 할당 실패 및 부족
        5. 컴퓨터 기계 자체의 문제( 전원 문제, 망가진 기억장치 등)
        
    - 특정 예외 처리
        - 산술 연산에 대한 예외를 따로 특정해서 잡으려면 ArithmeticException 사용 가능
        
        ```kotlin
        fun main( ){
        	val a = 6
        	val b = 0
        	val c : Int
        
        	try {
        			c = a/b // 0으로 나눔
        	} catch ( e : ArithmeticException) {
        			println("Exception is handeled. ${e.message}")
        	} finally {
        			println("finally 블록은 반드시 항상 실행됨")
        	}
        }
        
        // 실행 결과
        // Exception is handeled. / by zero
        // finally 블록은 항상 실행됨
        ```
        
    - 스택의 추적
        
        ```kotlin
        fun main( ){
        	val a = 6
        	val b = 0
        	val c : Int
        
        	try {
        			c = a/b // 0으로 나눔
        	} catch ( e : Exception) { // ArithmeticException -> Exception
        			e.printStackTrace()
        	} finally {
        			println("finally 블록은 반드시 항상 실행됨")
        	}
        }
        
        // 실행 결과
        // java.lang.ArithmeticException: / by zero
        //	at TestKt.main(test.kt:8)
        //	at TestKt.main(test.kt)
        // finally 블록은 반드시 항상 실행됨
        ```
        
    - 예외 발생 시키기
        
        ```kotlin
        fun main( ){
        		var amount = 600
        
        		try {
        				amount -= 100
        				checkAmount(amount)
        		} catch ( e : Exception){
        				println(e.message)
        		}
        		println("amount : $amount")
        }
        
        fun checkAmount(amount : Int){
        		if(amount < 1000)
        				throw Exception("잔고가 $amount 으로 1000원 이하입니다.")
        }
        
        /* 실행결과
        잔고가 500 으로 1000원 이하입니다.
        amount : 500
        */
        ```
        
    - 사용자 정의 예외
        
        ```kotlin
        class InvalidNameException(message : String) : Exception(message) // 사용자 예외 클래스
        
        fun main( ){
        		var name = "Kildong123" // 숫자가 포함된 이름
        
        		try {
        				validateName(name) 
        		} catch ( e : InvalidNameException){ // 숫자가 포함된 예외 처리
        				println(e.message)
        		} catch ( e : Exception){ // 기타 예외 처리
        				println(e.message)
        		}
        }
        
        fun validateName(name : String){
        		if(name.matches(Regex(".*\\d+.*"))) // 숫자가 포함되어있으면 예외 발생
        				throw InvalidNameException("your name : $name contains numerals.")
        }
        
        // 실행 결과
        // your name : Kildong123 contains numerals.
        ```
