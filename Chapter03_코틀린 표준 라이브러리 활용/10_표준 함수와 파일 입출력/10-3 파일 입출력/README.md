# 10-3 파일 입출력

### 표준 입출력의 기본 개념

- 입력에 실패할 경우 null 가능성이 생기기 때문에 !! 혹은 ?로 NPE를 처리한다.

```kotlin
import java.util.*

fun main() {
    // 기본 코틀린의 readLine(), 기본값은 문자열
    print("Enter: ")
    val input = readLine()!!
		// 입력 직후 정수형으로 바꾸고 싶을 때, readLine()!!.toInt()
    println("You entered: $input")

		// 자바의 Scanner를 통한 입력
    val reader = Scanner(System.`in`)
    var integer:Int = reader.nextInt()
    println("You entered: $integer")
}
```

- nextInt(), nextLong(), nextFloat(), nextDouble(), nextBoolean(), nextLine() 등등…
- Kotlin의 입출력 API
    - [kotlin.io](http://kotlin.io) 패키지는 자바 라이브러리를 확장한 것.
        
        ![Untitled](10-3%20%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8E%E1%85%AE%E1%86%AF%E1%84%85%E1%85%A7%E1%86%A8%20e82ce39b5ac24379978c2c9b75c9d261/Untitled.png)
        
        - 간단한 데이터: readBytes, readLines, readText
        - 대량의 데이터: copyTo, forEachBlock, forEachLine
        - InputStream, Reader, Writer를 쓰고 나서 항상 닫아줘야 한다.
- 자바의 io, nio의 개념
    - 코틀린에서는 자바 라이브러리를 그대로 사용할 수 있으니 알아두자
    - 비동기 관련 루틴은 코루틴에서 지원하고 있다. (11장)
    - io, nio의 기본적인 차이점은 버퍼 사용 여부
        
        ![Untitled](10-3%20%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8E%E1%85%AE%E1%86%AF%E1%84%85%E1%85%A7%E1%86%A8%20e82ce39b5ac24379978c2c9b75c9d261/Untitled%201.png)
        
- 스트림과 채널
    - 입력 스트림과 출력 스트림으로 구분
    - 채널 방식은 양방향으로 입출력이 가능
        
        ![Untitled](10-3%20%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8E%E1%85%AE%E1%86%AF%E1%84%85%E1%85%A7%E1%86%A8%20e82ce39b5ac24379978c2c9b75c9d261/Untitled%202.png)
        
- 넌버퍼와 버퍼 방식
    - 스트림에선 1바이트 씩 읽기 때문에 버퍼를 사용해서 다수의 데이터를 읽는 것보다 느림.
    - io 방식에서는 버퍼와 병합한 BufferedInputStream과 BufferedOutputStream
    - nio에서는 기본적으로 버퍼를 사용하기 때문에 데이터를 일일이 읽는 것보다 더 낫다.
- 블로킹과 넌블로킹
    
    ![Untitled](10-3%20%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8E%E1%85%AE%E1%86%AF%E1%84%85%E1%85%A7%E1%86%A8%20e82ce39b5ac24379978c2c9b75c9d261/Untitled%203.png)
    
    - 쓸 공간이 없을 때, 읽을 내용이 없을 때 대기하고 있는 상대를 블로킹이라고 한다.
    - 메인 코드의 흐름을 방해하지 않도록 입출력 작업 시 스레드나 비동기 루틴에 맡겨
    별개의 흐름으로 작업하는 것을 넌블로킹이라 한다.

### 파일에 쓰기

- Files 클래스와 Paths, StandardOpenOption

```kotlin
import java.io.IOException
import java.nio.file.Files
import java.nio.file.Paths
import java.nio.file.StandardOpenOption

fun main() {

    val path = "D:\\test\\hello.txt"  // 파일을 생성할 경로
    val text = "안녕하세요! Hello World!\n"

    try {
        Files.write(Paths.get(path), text.toByteArray(), StandardOpenOption.CREATE)
    } catch (e: IOException) {
    }
}
```

- StandardOpenOption의 주요 옵션: READ, WRITE, APPEND, CREATE
- 파일 경로는 문자열도 가능하지만 URI 객체도 가능하다.
- File의 PrintWriter 사용하기
    - 콘솔에 출력하듯 바이트 단위로 파일에 쓸 수 있다.
    
    ```kotlin
    import java.io.File
    import java.io.FileWriter
    import java.io.PrintWriter
    
    fun main() {
        val outString: String = "안녕하세요!\tHello\r\nWorld!."  // CR LF
        val path = "D:\\test\\testfile.txt"
    
        val file = File(path)
        val printWriter = PrintWriter(file)
    
        printWriter.println(outString)  // 파일에 출력
        printWriter.close()
    
    		// 람다식으로 줄인 표현
        // File(path).printWriter().use { it.println(outString) }
    }
    ```
    
- File의 BufferedWriter 이용하기
    - PrintWriter와 동일하지만 버퍼를 사용한다는 것이 다르다.
    
    ```kotlin
    File(path).bufferedWriter().use { it.println(outString) }
    ```
    
- File의 writeText() 사용하기 (p490)
    
    ```kotlin
    val file = File(path)
    file.writeText(outString)
    file.appendText("\nDo great work!") // 파일에 문자열 추가
    ```
    
    ![스크린샷 2022-09-23 오후 7.23.45.png](10-3%20%E1%84%91%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%8E%E1%85%AE%E1%86%AF%E1%84%85%E1%85%A7%E1%86%A8%20e82ce39b5ac24379978c2c9b75c9d261/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-09-23_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.23.45.png)
    
    - writeText()의 상위 클래스로 올라가보면 FileOutputStream에서 표준 함수 use()를 이용해 write()가 사용된다.
    - printWrite()는 null을 파일에 쓸 수 있지만, bufferedWriter()는 NPE가 발생할 수 있다.
    - writeText()를 사용하는 경우에는 자료형 불일치 오류가 발생할 수 있다.
- FileWriter 사용하기
    
    ```kotlin
    val writer = FileWriter(path, true)
    try {
        writer.write(outString)
    } catch (e: Exception) {
        // 오류 발생!
    } finally {
        writer.close()
    }
    
    FileWriter(path, true).use { it.write(outString) }
    ```
    

### 파일에서 읽기

- File의 FileReader 사용하기
    
    ```kotlin
    fun main() {
        val path = "D:\\test\\Over the Rainbow.txt"
    
        try {
            val read = FileReader(path)
            println(read.readText())
        } catch (e: Exception) {
            println(e.message)
        }
    }
    ```
    
- 자바의 파일 읽기를 코틀린으로 변경하기
    
    ```kotlin
    fun main() {
        val path = "D:\\test\\Over the Rainbow.txt"
    
    // 단순 변환
    /*
        val file = File(path)
        val inputStream: InputStream = file.inputStream()
        val inputStreamReader = InputStreamReader(inputStream)
        val sb = StringBuilder()
        var line: String?
        val br = BufferedReader(inputStreamReader)
        try {
            line = br.readLine()
            while (line != null) {
                sb.append(line, '\n')
                line = br.readLine()
            }
            println(sb)
        } catch (e:Exception){
        } finally {
            br.close()
        }
    */
    
    // use를 사용한 축소
    val file = File(path)
    val inputStream: InputStream = file.inputStream()
    // 자바의 InputStream에는 bufferedReader()가 없지만 kotlin.io 라이브러리 패키지에서
    // 확장함수 기법으로 InputStream에 추가되었다.    
    val text = inputStream.bufferedReader().use { it.readText() }
    println(text)
    
    // BufferedReader만 사용해서 읽기
    val bufferedReader: BufferedReader = File(path).bufferedReader()
    val inputString = bufferedReader.use { it.readText() }
    println(inputString)
    
    // use 대신 useLines를 이용해서 줄 단위로 처리
    val bufferedReader = File(path).bufferedReader()
    val lineList = mutableListOf<String>()
    bufferedReader.useLines { lines -> lines.forEach { lineList.add(it) } }
    lineList.forEach { println(">  " + it) }
    
    // BufferedReader의 생략, mutableListOf 사용
    val lineList = mutableListOf<String>()
    File(path).useLines { lines -> lines.forEach { lineList.add(it) } }
    lineList.forEach { println(">  " + it) }
    ```
    
- copyTo() 사용하기
    
    ```kotlin
    public fun File.copyTo(target: File, overwrite: Boolean = false,
    bufferSize: Int = DEFAULT_BUFFER_SIZE): File
    
    File(path).copyTo(File("D:\\test\\file2.txt"))
    // path가 없으면 FileNotFoundException
    ```
    
    ```kotlin
    // 파일의 내용 출력하기
    File(path).forEachLine { println(it) }
    
    // 바이트 단위로 읽기 (쓰기는 writeBytes())
    val bytes = File(path).readBytes()
    println(Arrays.toString(bytes))
    
    // 줄 단위로 읽기
    val lines = File(path).readLines()
    lines.forEach { println(it) }
    
    // 텍스트 단위로 읽기 (쓰기는 writeText())
    val text = File(path).readText()
    println(text)
    ```