# 02-2 변수와 자료형

- 변수는 val, var 키워드를 이용해 선언할 수 있음
- val : 값 변경 불가
- var : 값 변경 가능
- `val username: String = “jiwon”`
    - val로 선언했기 때문에 이제 값 변경 불가
- 코틀린은 자료형을 지정하지 않고 변수를 선언하면 알아서 자료형 지정 가능
    - `val username = “jiwon”`도 가능
- 하지만 값을 주지 않고 자료형 없이 변수 선언 불가
- 값을 할당해주지 않으면 변수의 자료형을 추론 할 수 없기 때문에
    
    `val username` : 불가
    
- 변수 이름 규칙
    1. 숫자로 시작하면 안됨
    2. while, if와 같은 코틀린에서 사용되는 키워드는 사용 불가
    3. 카멜 표기법을 쓰는 게 좋다

- 자료형
    - 코틀린은 참조형 자료형을 사용한다.
        - 기본형 자료형 : 가공되지 않은 순수한 자료형 (자바에서 int, long, float, double 등)
        - 참조형 자료형 : 객체를 생성하고 동적 메모리 영역에 데이터를 둔 다음 참조하는 자료형(자바에서 String, Date)
    - 💡기본형과 참조형의 동작 원리
        
        ```kotlin
        int a = 77;
        Person person = new Person();
        ```
        
        - 기본형으로 선언한 변수 a는 스택에 저장되며, 크기도 고정되어있음.
        - 참조형 변수는 스택에 값이 아닌 참조 주소가 있음, 실제 객체는 힙에 저장되어 있음.
        - 참조형으로 선언한 변수는 성능 최적화를 위해 코틀린 컴파일러에서 다시 기본형으로 대체되므로 최적화하려고 하지 않아도 됨
        
    
    - 정수 자료형
        
        
        | Long | 8바이트 | -2^63 ~ 2^63 -1 |
        | --- | --- | --- |
        | Int | 4바이트 | -2^31 ~ 2^31 -1 |
        | Short | 2바이트 | -2^15 ~ 2^15 -1 |
        | Byte | 1바이트 | -2^7 ~ 2^7 -1 |
        
        ```kotlin
        // 값 할당 방법
        val num05 = 127 // Int형으로 추론
        val num06 = -32768 // Int형으로 추론
        val num07 = 2147483647 // Int형으로 추론
        val num08 = 9223372036854775807 //Long형으로 추론
        
        // 2진수나 16진수 표현
        val exp01 = 123 // Int형으로 추론
        val exp02 = 123L // 접미사 L을 사용해 Long형으로 추론
        val exp03 = 0x0F // 접두사 0x를 사용해 16진 표기가 사용된 Int형으로 추론
        val exp04 = 0b00001011 // 접두사 0b를 사용해 2진 표기가 사용된 Int형으로 추론
        
        // 숫자는 보통 Int로 추론되기때문에 Short나 Byte를 쓰려면
        val exp08 : Byte = 127 // 명시적으로 자료형을 지정(Byte형)
        val exp09 = 32767 // 명시적으로 자료형을 지정하지 않으면 Short형 범위도 Int형으로 추론
        val exp10 : Short = 32767 // 명시적으로 자료형을 지정(Short형)
        ```
        
    
    - 부호 없는 정수 자료형
        
        
        | ULong | 8바이트 | 0 ~  2^64 -1 |
        | --- | --- | --- |
        | UInt | 4바이트 | -0 ~ 2^32 -1 |
        | UShort | 2바이트 | 0 ~  2^16 -1 |
        | UByte | 1바이트 | 0 ~  2^8 -1 |
        
        ```kotlin
        // 값 할당하는 방법
        val uint : UInt = 153u
        val ushort :UShort = 65535u
        val ulong : ULong = 46322342uL
        val ubyte : UByte = 255u
        
        // 언더스코어로 자릿값 구분 가능, 언더스코어 아무데나 넣을 수 있음
        val number = 1_000_000
        ```
        
    
    - 실수 자료형
        
        
        | Double | 8바이트 |  약 4.9E - 324 ~ 1.7E + 308 |
        | --- | --- | --- |
        | Float | 4바이트 | 약 1.4E - 45 ~ 3.4E + 38 |
        
        ```kotlin
        // 값 할당 방법
        val exp01 = 3.14 // 기본으로 Double형으로 추론
        val exp02 = 3.14F // 식별자 F에 의해 Float형으로 추론, f도 가능
        ```
        
        - 무한한 실수의 개수를 메모리 공간에 모두 저장할 수 없기 때문에 부동 소수점 방식을 사용해 실수를 표기한다.
        - 코틀린에서 소수점의 이동은 오른쪽에 e나 E와 함께 밑수인 10을 제외하고 지수만 적는다.
        
        ```kotlin
        val exp03 = 3.14E-2 // 왼쪽으로 소수점 2칸 이동, 0.0314
        val exp04 = 3.14e2 // 오른쪽으로 소수점 2칸 이동, 314
        ```
        
    
    - 정수자료형과 실수 자료형의 최솟값과 최댓값 알아보기
        
        
        |  | Min | Max |
        | --- | --- | --- |
        | Byte | -128 | 127 |
        | Short | -32768 | 32767 |
        | Int | -2147483648 | -2147483647 |
        | Long | 9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
        | Float | 1.4E-45 | 3.4028235E38 |
        | Double | 4.9E-324 | 1.797631348623157E308 |
    
    - 논리 자료형
        
        
        | Boolean | 1비트 | true, false |
        | --- | --- | --- |
        
        ```kotlin
        val isOpen = true // isOpen은 Boolean형으로 추론
        val isUpload : Boolean // 변수만 선언한 경우 자료형 선언 필수
        ```
        
    
    - 문자 자료형
        
        
        | Char | 2바이트 | 0 ~ 2^15 -1 |
        | --- | --- | --- |
        
        ```kotlin
        val ch = 'c' // ch는 Char로 추론
        val ch2 : Char // 변수만 선언한 경우 자료형 선언 필수
        
        // 코틀린에서는 숫자를 사용해 문자를 "선언"하는 것은 금지
        val chNum : Char = 65 // 에러 발생
        
        // 사용할때는 숫자를 더하는 방식으로 다른 문자 표현 가능
        val ch = 'A'
        println(ch + 1) // B
        
        // 정수 자료형으로 문자 자료형을 선언하려면? 65 바로 선언하고 싶으면?
        val code : Int = 65
        val chFromCode : Char = code.toChar()
        println(chFromCode) // A
        
        // 문자 자료형에는 1개의 문자만 저장 가능
        // 여러 문자가 포함된 문자열을 쓰려면 문자열 자료형 사용
        ```
        
    
    - 문자열 자료형
        - 문자열 자료형은 기본형에 속하지 않는 배열 형태로 되어있는 특수한 자료형
        - 문자열 선언 및 할당
            
            ```kotlin
            var str1 : String = "Hello"
            var str2 = "World"
            var str3 = "Hello"
            
            println("str1 === str2 : ${str1 === str2}") // str1 === str2 : false
            println("str1 === str3 : ${str1 === str3}") // str1 === str3 : true
            ```
            
        - “Hello”를 스택에 2번 저장하는 것보다 이미 저장된 값을 활용하는 것이 효율적이므로 힙 영역의 String Pool이라는 공간에 문자열인 “Hello”를 저장해두고 이 값을 str1, str3이 참조하도록 만듬.
        - 문자열 자료형은 String Pool을 이용해 필요한 경우 메모리 공간을 재활용함
        - 표현식과 $ 기호 사용하여 문자열 출력하기 ($ 사용법)
            - 문자열 안에 변수 a 값 사용하기
            
            ```kotlin
            var a = 1
            var s1 = "a is $a" // s1 = "a is 1"
            ```
            
            - 문자열에 표현식 사용하기
            
            ```kotlin
            var a = 1
            var str1 = "a = $a"
            var str2 = "a = ${a + 2}" // 문자열에 표현식 사용
            
            println("str1 : \"$str1\", str2 : \"$str2\"") 
            // str1 : "a = 1", str2 : "a = 3"
            ```
            
            - “””  : 눈에 보이지 않는 줄바꿈, 탭, 공백을 문자열에 포함시켜 출력하기
            
            ```kotlin
            val num = 10
            var fomattedString = """
            			var a = 5
            			var b = "Kotlin"
            			println(a + num)
            			"""
            println(fomattedString)
            
            /*
            
            			var a = 5
            			var b = "Kotlin"
            			println(a + num)
            
            */
            ```
            
        - 자료형에 별명 붙이기 : typealias
            
            ```kotlin
            typealias Username = String
            val user : Username = "Kildong"
            ```
