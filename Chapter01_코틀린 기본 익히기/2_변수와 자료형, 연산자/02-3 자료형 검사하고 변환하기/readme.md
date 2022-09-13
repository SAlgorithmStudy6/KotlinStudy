# 02-3 자료형 검사하고 변환하기

- 코틀린은 변수를 사용할 때 반드시 값이 할당 되어있어야함
- 값이 할당 되지 않은 변수를 사용하면 오류 발생
- null 상태인 변수를 허용하려면 물음표(?) 기호를 사용해 선언해야한다.
- 변수에 null 할당하기
    
    ```kotlin
    var str1 : String ? = "Hello Kotiln"
    str1 = null
    println("str1 : $str1")
    
    // 오류! ? 없으면 null 선언 불가
    var str1 : String = "Hello Kotiln" 
    str1 = null
    println("str1 : $str1")
    
    // 오류! 값 할당 바로 해줘야함
    var str1:String?
    
    // 정상 작동
    var str1:String? = null 
    ```
    

- null 할당 시 length 실행 불가 → 세이프콜이나 non-null 단정기호만 허용함
    
    ```kotlin
    // 오류! null 할당시 length 실행 불가
    var str1:String? = "Hello Kotlin"
    str1 = null
    println("str1 : $str1 length : ${str1.length}")
    
    // 오류! ?만 써도 length 실행 불가
    var str1:String? = "Hello Kotlin"
    println("str1 : $str1 length : ${str1.length}")
    
    // 세이프콜 (변수 뒤에 ?.)
    println("str1 : $str1 length : ${str1?.length}") // str1 : null length : null
    
    // non-null (변수 뒤에 !!.)
    println("str1 : $str1 length : ${str1!!.length}") // str1 : null length : null
    // null일 경우 실행할때는 에러가 나지 않지만 프로그램 실행시 에러 발생
    ```
    

- 💡세이프 콜(`?.`)
    - 해당 변수를 검사 후 null이 발견되면 코드에 접근하지 않고 그대로 null을 내보냄
    
- 💡non-null 단정 기호 (`!!.`)
    - 해당 변수의 값이 null이 아니라고 단정 짓고, 프로그램 실행
    - 프로그램 실행 시 변수의 값이 null이면 NFE(NullPointerException) 발생
    
- 조건문을 활용해 null 허용한 변수 검사
    
    ```kotlin
    var str1 : String ? = "Hello Kotlin"
    str1 = null
    
    // 조건식을 통해 null 상태 검사
    val len = if(str1 != null) str1.length else -1
    println("str1 : $str1, length : ${len}") // str1 : null, length : -1
    ```
    

- 세이프 콜과 엘비스 연산을 활용해 null을 허용한 변수 더 안전하게 사용하기
    
    ```kotlin
    var str1 : String? = "Hello Kotlin"
    str = null
    println("str1 : $str1, length :${str1?.length ?: -1}") // 세이프 콜과 엘비스 연산자 활용
    // null이면 -1
    
    var str1 : String? = "Hello Kotlin"
    str = "hi"
    println("str1 : $str1, length :${str1?.length ?: -1}") // 세이프 콜과 엘비스 연산자 활용
    // 2
    
    ```
    
    - `str1?. length ?: -1` 의 의미는 `if (str1 != null) str1.length else -1` 과 같다

- 💡 엘비스 연산, 널 복합 연산자 (`?:`)
    - null 값을 허용하지 않는 변수에 null 값이 들어 갔을 때 **null을 대신할 디폴트 값을 지정**할 수 있음
    
    ```kotlin
    fun main(){
    	var yts : String? = null
    
    	val name : String = yts ?: "VTS"
    	// yts라는 변수는 현재 null이기때문에 name에 "VTS"값이 들어감
    	val name2 : String = yts ?: return 
    	// 함수 자체를 return 시키도록 만들 수도 있음
    	val name3 : String = yts ?: throw NullPointerException() 
    	// yts라는 변수는 null이기때문에 NullPointerException() 예외 발생
    ```
    

- 자료형 변환 : 코틀린에서는 직접 해야 함
    
    ```kotlin
    val a : Int = 1 
    val b : Double = a // 오류! 자료형 불일치
    val c : Int = 1.1 // 오류! 자료형 불일치
    
    val a : Int = 1
    val b : Double = a.toDouble() // 변환 메서드 사용
    
    ```
    

- 자료형이 서로 다른 값을 연산하면, 범위가 큰 자료형으로 자동 형 변환하여 연산
    
    ```kotlin
    val result = 1L + 3 // Long형 + Int형 -> result는 Long 형
    ```
    

- 형 변환 메서드의 종류
    - toByte
    - toFloat
    - toLong
    - toShort
    - toDouble
    - toChar
    - toInt
- 기본형과 참조형 자료형의 비교 원리
    - 값만 비교하는 방법 ==
        - 값이 동일하면 true, 다르면 false
    - 참조 주소까지 비교하는 방법 ===
        - 값과 상관없이 참조가 동일하면 true
    
    ```kotlin
    val a : Int = 128
    val b : Int = 128
    println(a == b) // true
    println(a === b) // true
    
    val a : Int = 128
    val b : Int? = 128
    println(a == b) // true
    println(a === b) // false
    ```
    
    - Int 형으로 선언된 a는 기본형으로 변환되어 스택에 128이라는 값 자체를 저장
    - 하지만 Int?형으로 선언된 b는 참조형으로 저장되므로 b에는 128이 저장된 힙의 참조주소가 들어있음
    - 그래서 삼중 등호로 비교하면 false가 나옴
- 스마트 캐스트
    - 어떤 값이 정수일 수도 있고, 실수 일 수도 있는 경우
    - 컴파일러가 자동으로 형 변환을 하는 스마트 캐스트
    - 스마트 캐스트가 적용되는 자료형 : Number형 .. 등
    
    ```kotlin
    var test : Number = 12.2 // Float형으로 스마트 캐스트
    
    test = 12 // Int형으로 스마트 캐스트
    
    test = 120L // Long형으로 스마트 캐스트
    
    test = 12.0f // Float 형으로 스마트 캐스트
    ```
    

- 자료형 검사하기
    - `is` 키워드
    
    ```kotlin
    val num = 256
    
    if (num is Int){
    	print(num)
    }else if(num !is Int){
    	print("Not a Int")
    }
    ```
    
    - 코틀린 최상위 기본 클래스 : `Any`형, 어떤 자료형도 될 수 있음
    
    ```kotlin
    val x : Any
    x = "Hello"
    if( x is String){
    	print(x.length) // x는 자동적으로 String으로 스마트 캐스트
    }
    ```
    
    - Any형은 코틀린의 최상위 기본 클래스로 어떤 자료형이라도 될 수 있는 특수한 자료형
    - 이때 `is`를 사용하여 자료형을 검사하면 검사한 자료형으로 스마트 캐스트 됨

- `as`에 의한 스마트 캐스트
    - `as`는 형 변환이 가능하지 않으면 예외를 발생시킴
    
    ```kotlin
    val x : String = y as String
    ```
    
    - 위의 경우 y가 null이 아니면 String으로 형 변환되어 x에 할당됨
    - y가 null이면 형 변환을 할 수 없으므로 예외 발생
    
    ```kotlin
    val y : String = "Hello"
    val x : String? = y as? String // 이렇게하면 null 예외 발생해도 괜찮
    println("x = $x"); // x = Hello
    
    var y : String? = null
    val x : String? = y as? String
    println("x = $x") // x = null
    ```
    

- 묵시적 변환
    - Any 형은 자료형이 정해지지 않은 경우 사용
    - 코틀린 Any 형은 모든 클래스의 뿌리
    - Any형 변수의 변환
        
        ```kotlin
        var a : Any = 1 // Any형 a는 1로 초기화 될 때 Int형이 됨
        a = 20L // Int형이었던 a는 변경된 값 20L에 의해 Long형이 됨
        println("a = $a, type : {$a.javaClass}") // a의 자바 기본형을 출력하면 long이 나옴
        ```
        
    - Any형으로 인자를 받는 함수 만들기 (다음 장에서 자세히 배움)
        
        ```kotlin
        fun main(){
        	checkArg("Hello") // 인자를 문자열로
        	checkArg(5) // 인자를 숫자로
        }
        
        fun checkArg(x : Any){
        	if (x is String){
        		println("x is String: $x")
        	}
        	if (x is Int){
        		println("x is Int: $x")
        	}
        }
        
        // 실행결과
        // x is String : Hello
        // x is Int : 5
        ```
        
        - checkArg()함수의 인자 x가 Any형으로 선언되었음
        - 함수는 x에 들어오는 인자의 자료형에 따라 문자열, 정수형 등으로 받아 처리 가능
        - if문에서 `is`연산자를 이용해 인자로 전달 받은 값을 검사하며 자료형을 `Any`에서 검사한 자료형(String, Int)로 변환하고 있음! (⭐실무에서 자주 사용되는 방법⭐)
