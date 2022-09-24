# 10-1 코틀린 표준 함수

### 람다식과 고차 함수 복습하기

- 람다식
    
    ```kotlin
    val 변수이름: 자료형선언 = { 매개변수[,...] -> 람다식 본문 }
    val sum: (Int, Int) -> Int = { x,y -> x + y }
    val mul = { x: Int, y, Int -> x * y }
    ```
    
    - 매개변수가 1개인 경우, 매개변수를 생략하고 it으로 표기
        
        ```kotlin
        val add: (Int) -> Int = {it + 1}
        ```
        
    - 추론된 반환 자료형이 Unit이 아닌 경우에는 본문의 마지막 표현식이 반환값
        
        ```kotlin
        val isPositive: (Int) -> Boolean = {
            val isPositive = it > 0
            isPositive  // 마지막 표현식이 반환됨
        }
        
        val isPositiveLabel: (Int) -> Boolean = number@ {
            val isPositive = it > 0
            return@number isPositive  // 라벨을 사용해 반환됨
        }
        ```
        
- 고차 함수
    - 고차 함수는 함수의 매개변수로 함수를 받거나 함수 자체를 반환할 수 있는 함수
    
    ```kotlin
    fun inc(x: Int): Int {
        return x + 1
    }
    
    fun high(name: String, body: (Int) -> Int): Int {
        println("name: $name")
        val x = 0
        return body(x)
    }
    // body는 람다식 함수로 받을 수 있다
    val result1 = high("Sean", { x -> inc(x + 3) })  // 함수를 이용한 람다식
    val result2 = high("Sean") { inc(it + 3) }  // 소괄호 바깥으로 빼내고 생략
    val result3 = high("Kim", ::inc)    // 매개변수 없이 함수의 이름만 사용할 때
    val result4 = high("Sean") { x -> x + 3 }   // 람다식 자체를 넘겨준 형태
    val result5 = high("Sean") { it + 3 }       // 매개변수가 1개인 경우 생략
    ```
    

### 클로저

- 람다식으로 표현된 내부 함수에서 외부 범위에 선언된 변수에 접근할 수 있는 개념
- 람다식 안에 있는 외부 변수는 값을 유지하기 위해 람다식이 포획한 변수라 부른다.
- 기본적으로 함수 안에 정의된 변수는 지역 변수로 스택에 저장되어 있다.
- 하지만 클로저 개념에서 포획한 변수는 참조가 유지되어 함수가 종료되어도 사라지지 않고
함수의 변수에 접근하거나 수정할 수 있게 한다.
- 클로저의 조건
    - final 변수를 포획한 경우 변수 값을 람다식과 함께 저장한다.
    - final이 아닌 변수를 포획한 경우 변수를 특정 wrapper로 감싸서 나중에 변경하거나 읽을 수 있게 한다.이때 래퍼에 대한 참조를 람다식과 함께 저장한다.
- 클로저 테스트하기
    - 코틀린에서 final이 아닌 변수를 사용하면 내부적으로 변환된 자바 코드에서
    final로 지정해 사용된다.
    
    ```kotlin
    fun main() {
        val calc = Calc()
        var result = 0 // 외부의 변수
        calc.addNum(2, 3) { x, y -> result = x + y }
        println(result)
    }
    
    class Calc {
        fun addNum(a: Int, b: Int, add: (Int, Int) -> Unit) {
            add(a,b)    // 람다식 add에는 변환값이 없다
        }
    }
    ```
    
    - var로 선언된 result는 addNum()이 호출되면 자신의 유효 범위를 벗어나 삭제되어야 하지만
    클로저의 개념에 의해 독립된 복사본을 가진다.
    - 람다식에서 변환 값은 Unit으로 선언되어 반환 값이 없지만, 포획된 변수 result에 값을 저장할 수 있다.
    
    ![스크린샷 2022-09-18 오후 4.08.04.png](10-1%20%E1%84%8F%E1%85%A9%E1%84%90%E1%85%B3%E1%86%AF%E1%84%85%E1%85%B5%E1%86%AB%20%E1%84%91%E1%85%AD%E1%84%8C%E1%85%AE%E1%86%AB%20%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE%204ef49da9a81e447f8cc0e6ae4559cc17/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-18_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.08.04.png)
    
    - length는 람다식 바깥의 변수로 인자로 입력받은 길이에 일치하는 요소 목록을 반환해
    filterResult에 저장하고 출력한다.
- 클로저를 사용하면 내부의 람다식에서 외부 함수의 변수에 접근해 처리할 수 있어 효율성이 높다.
- 완잔히 다른 함수에서 변수에 접근하는 것을 제한할 수 있다.
- 코틀린의 표준 라이브러리는 이러한 개념이 사용되어 설계되었다.

### 코틀린의 표준 라이브러리

![Untitled](10-1%20%E1%84%8F%E1%85%A9%E1%84%90%E1%85%B3%E1%86%AF%E1%84%85%E1%85%B5%E1%86%AB%20%E1%84%91%E1%85%AD%E1%84%8C%E1%85%AE%E1%86%AB%20%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE%204ef49da9a81e447f8cc0e6ae4559cc17/Untitled.png)

### let() 함수 활용하기

- let() 함수는 함수를 호출하는 객체 T를 이어지는 block의 인자로 넘기고 block의 결괏값 R을 반환한다.
    
    ```kotlin
    public inline fun <T, R> T.let(block: (T) -> R: R {...return block(this)}
    ```
    
    ```kotlin
    fun main() {
        val score: Int? = 32
    //    var score = null
    
        // 일반적인 null 검사
        fun checkScore() {
            if (score != null) {
                println("Score: $score")
            }
        }
    
        // let 함수를 사용해 null 검사를 제거
        fun checkScoreLet() {
            score?.let { println("Score: $it") }  // 1번
            val str = score.let { it.toString() } // 2번
            println(str)
        }
    
        checkScore()
        checkScoreLet()
    }
    ```
    
    1번은 세이프 콜을 사용했으므로 score = null 이면 람다식은 실행되지 않는다.
    
    2번은 it을 문자열로 변환한 후 반환된 값을 str에 할당했으므로 null이 출력된다. 
    
- 커스텀 뷰에서 let() 함수 활용하기
    
    ```kotlin
    // 1.
    val padding = TypedValue.applyDimension(
        TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics).toInt()
    
    setPadding(padding, 0, padding, 0)
    
    // 2. val padding을 없애서 효율적으로
    TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics)
            .toInt().let { padding -> setPadding(padding, 0, padding, 0) }
    
    // 3. 인자가 하나 밖에 없으므로 padding을 it으로
    TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics)
            .toInt().let { setPadding(it, 0, it, 0) }
    ```
    
- null 가능성 있는 객체에서 let() 함수 활용하기
    
    ```kotlin
    var obj: String?
    if (null != obj) {
        Toast.makeText(applicationContext, obj, Toast.LENGTH_LONG).show()
    }
    
    obj?.let {
        Toast.makeText(applicationContext, it, Toast.LENGTH_LONG).show()
    }
    ```
    
    ```kotlin
    val firstName: String?
    var lastName: String
    
    // if를 사용
    if (null != firstName) {
        print("$firstName $lastName")
    } else {
        print("$lastName")
    }
    
    // let을 사용
    firstName?.let { print("$it $lastName") } ?: print("$lastName")
    ```
    
- 메서드 체이닝을 사용할 때 let() 함수 활용하기
    - 메서드 체이닝이란 여러 메서드 혹은 함수를 연속적으로 호출하는 기법
    
    ```kotlin
    var a = 1
    var b = 2
    
    a = a.let { it + 2 }.let {
        val i = it + b
        i  // 마지막 식 반환
    }
    println(a) // 5
    ```
    

### also() 함수 활용하기

- also() 함수는 함수를 호출하는 객체 T를 이어지는 block에 전달하고 객체 T 자체를 반환한다.
    
    ```kotlin
    public inline fun <T, R> T.let(block: (T) -> R: R = block(this)
    public inline fun <T> T.also(block: (T) -> Unit): T { block(this); return this }
    ```
    
    ```kotlin
    var m = 1
    m = m.also { it + 3 }
    println(m)  // 원본 값 1
    ```
    
- let() 함수와 also() 함수 비교
    
    ```kotlin
    fun main() {
        data class Person(var name: String, var skills: String)
        var person = Person("Kildong", "Kotlin")
        val a = person.let {  // 1번
            it.skills = "Android"
            "success"
        }
        println(person)
        println("a: $a")    // String
        val b = person.also {  // 2번
            it.skills = "Java"
            "success"
        }
        println(person)
        println("b: $b")    // Person의 객체 b
    }
    /*
    출력
    Person(name=Kildong, skills=Android)
    a: success
    Person(name=Kildong, skills=Java)
    b: Person(name=Kildong, skills=Java)
    */
    ```
    
    1. let() 함수와 also() 함수를 비교해 보면 let() 함수는 person 객체에서 skills를 변경하고
    마지막 표현식인 “success”를 반환해 a에 할당한다.
    2. 반면 also()는 람다식이 본문을 처리하지만 마지막 표현식이 b에 할당되는 것이 아닌 person 객체
    자신에 할당된다. 따라서 b는 Person의 객체 person을 반환하고 새로운 객체 b가 할당된다.
- 특정 단위의 동작 분리
    
    ```kotlin
    // 기존의 디렉터리 생성 함수
    fun makeDir(path: String): File {
        val result = File(path)
        result.mkdirs()
        return result
    }
    
    // let()과 alse()를 통해 개선된 함수
    fun makeDir(path: String) = path.let{ File(it) }.also { it.mkdirs() }
    ```
    

### apply() 함수 활용하기

- apply() 함수는 also() 함수와 마찬가지로 호출하는 객체 T를 이어지는 block으로 전달하고
객체 자체인 this를 반환한다.
    
    ```kotlin
    public inline fun <T, R> T.let(block: (T) -> R): R = block(this)
    public inline fun <T> T.also(block: (T) -> Unit): T {block(this); return this}
    public inline fun <T> T.apply(block: T.() -> Unit): T { block(); return this }
    ```
    
    - apply() 함수는 특정 객체를 생성하며 함께 호출해야되는 초기화 코드가 있을 때 사용할 수 있다.
    - apply() 함수와 also() 함수의 다른 점은 T.()와 같은 표현에서 람다식이 확장함수로 처리된다.
    
    ```kotlin
    fun main() {
        data class Person(var name: String, var skills: String)
        var person = Person("Kildong", "Kotlin")
            person.apply { this.skills = "Swift" }  // 1.this는 person 객체
        println(person)
    
        val returnObj = person.apply {  // 2번
            name = "Sean"   // this는 생략할 수 있음
            skills = "Java" // this 없이 객체의 멤버에 여러 번 접근
        }
        println(person)
        println(returnObj)
    }
    /*
    출력
    Person(name=Kildong, skills=Swift)
    Person(name=Sean, skills=Java)
    Person(name=Sean, skills=Java)
    */
    ```
    
    1. apply()는 확장 함수로서 person을 this로 받아오는데 클로저를 사용하는 방식과 같다.
        - 객체의 프로퍼티를 변경하면 원본 객체에 반영되고 this로 반환된다.
    2. this로 반환된 객체를 returnObj에 할당
    3. also() 함수는 it을 사용해 멤버에 접근한다.
- 레이아웃을 초기화할 때 apply() 함수 활용하기
    
    ```kotlin
    // 기존의 코드
    val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT)
    param.gravity = Gravity.CENTER_HORIZONTAL
    param.weight = 1f
    param.topMargin = 100
    param.bottomMargin = 100
    
    val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT).apply {
        gravity = Gravity.CENTER_HORIZONTAL
        weight = 1f
        topMargin = 100
        bottomMargin = 100  // param. 없이도 값을 지정할 수 있다.
    }
    ```
    
- 디렉터리를 생성할 때 apply() 함수 활용하기
    
    ```kotlin
    // 기존의 디렉터리 생성 함수
    fun makeDir(path: String): File {
        val result = File(path)
        result.mkdirs()
        return result
    }
    
    File(path).apply { mkdirs() }
    ```
    

### run() 함수 활용하기

- run() 함수는 인자가 없는 익명 함수처럼 동작하는 형태와 객체에서 호출하는 형태 
두 가지로 사용할 수 있다.
    
    ```kotlin
    public inline fun <R> run(block: () -> R): R = return block()
    public inline fun <T, R> T.run(block: T.() -> R): R = return block()
    ```
    
    - block이 독립적으로 사용된다. 이어지는 block 내에서 처리할 작업을 넣어 줄 수 있다.
    - 일반 함수와 마찬가지로 값을 반환하지 않거나 특정 값을 반환할 수도 있다.
    
    ```kotlin
    var skills = "Kotlin"
    println(skills)
    
    val a = 10
    skills = run {
    	val level = "Kotlin Level:" + a
    	level // 마지막 표현식이 반환됨
    }
    println(skills)  // Kotlin Level:10
    ```
    
    ```kotlin
    fun main() {
        data class Person(var name: String, var skills: String)
        var person = Person("Kildong", "Kotlin")
        val returnObj = person.apply {
            name = "Sean"
            skills = "Java"
            "success"
        }
        val returnObj2 = person.run {
            this.name = "Dooly"
            this.skills = "C#"
            "success"   // 없으면 Unit이 반환됨
        }
        println(person)
        println("returnObj2: $returnObj2")
    }
    /*
    출력:
    Person(name=Dooly, skills=C#)
    returnObj2: success
    */
    ```
    

### with() 함수 활용하기

- with() 함수는 인자로 받는 객체를 이어지는 block의 receiver로 전달하며 결괏값을 반환한다.
run() 과 비슷하지만 receiver의 유무가 다르다.
    
    ```kotlin
    public inline fun <T, R> with(receiver: T, block: T.() -> R): R = receiver.block()
    ```
    
    - with() 함수는 매개변수가 2개이므로 with() {…} 와 같은 형태로 넣어준다.
    - 세이프 콜을 지원하지 않으므로 let과 같이 쓰기도 한다.
    
    ```kotlin
    fun main() {
        data class User(val name: String, var skills: String, var email: String? = null)
        val user = User("Kildong", "default")
    
        val result = with (user) {
            skills = "Kotlin"
            email = "kildong@example.com"
            // "Success"
        }
        println(user)
        println("result: $result")
    }
    /*
    출력:
    User(name=Kildong, skills=Kotlin, email=kildong@example.com)
    result: kotlin.Unit
    */
    ```
    

### use() 함수 활용하기

- use() 함수를 사용하면 객체를 사용한 후 close() 함수를 자동적으로 호출해 닫을 수 있다.
- T는 Closeable? 이므로 block은 닫힐 수 있는 객체를 지정해야한다.

```kotlin
public inline fun <T: Closealbe?, R> T.use(block: (T) -> R): R
```

```kotlin
fun main() {
    // output.txt에 hello 출력하고 use()로 파일을 닫는다.
    PrintWriter(FileOutputStream("d:\\test\\output.txt")).use {
        it.println("hello")
    }
		// contents.txt를 열고 내용을 읽어온다.
    val file = File("d:\\test\\contents.txt")
    file.bufferedReader().use {
        println(it.readText())
    }
}
```

### 기타 함수의 활용

- takeIf() 함수와 takeUnless() 함수의 활용
    - takeIf() 함수는 람다식이 true면 결과를 반환하고, takeUnless() 함수는 람다식이 false면 결과를 반환한다.
        
        ```kotlin
        public inline fun <T> T.takeIf(predicate: (T) -> Boolean): T?
        	= if (predicate(this)) this else null
        ```
        
        ```kotlin
        // 기존 코드
        if (someObject != null && someObject.status) {
        	doThis()
        }
        // 개선한 코드
        if (someObject?.status == true) {
        	doThis()
        }
        // takeIf()
        someObject?.takeIf { it.status }?.apply { doThis() }
        ```
        
        ```kotlin
        val input = "Kotlin"
        val keyword = "in"
        
        // 입력 문자열에 키워드가 있으면 인덱스를 반환하는 함수
        input.indexOf(keyword).takeIf { it >= 0 } ?: error("keyword not found")
        
        input.indexOf(keyword).takeUnless { it < 0 } ?: error("keyword not found")
        
        ```
        
- 시간의 측정
    
    ```kotlin
    val executionTime = measureTimeMillis {
    	// some codes....
    }
    println("Execution Time = $executionTime ms")
    ```
    
- 난수 생성하기
    
    ```kotlin
    val number = Random.nextInt(21)
    // 0~21 사이의 난수 제공
    ```