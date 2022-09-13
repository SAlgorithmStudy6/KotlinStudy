# 04-2 반복문

- for문
    - 선언 방법
        
        ```kotlin
        for (x in 1..5){
        	println(x)
        }
        ```
        
    - 5,4,3,2,1로 출력하려면 (위에서 아래로) `downTo`
        
        ```kotlin
        for (i in 5 downTo 1) print(i)
        
        for (i in 5..1) print(i) // 아무것도 출력되지 않음
        ```
        
    - 5,3,1을 출력하려면 (건너뛰면서) `step`
        
        ```kotlin
        for (i in 5 downTo 1 step 2) print(i)
        ```
        
    - for문을 이용해 삼각형 출력하기
        
        ```kotlin
        fun main(){
        		print("Enter the lines: ");
        		val n = readLine()!!.toInt() // 콘솔로부터 입력받음
        		
        		for (line in 1..n){
        				for(space in 1..(n-line)) print (" ") // 공백 출력
        				for(star in 1..(2*line -1)) print("*") // 별표 출력
        				println()
        		}
        }
        
        // 실행 결과
        // Enter the lines: 4
        //    *
        //   ***
        //  *****
        // *******
        ```
        
- while문
    
    ```kotlin
    var i = 1
    while (i <= 5){
    	println("$i")
    	++i
    }
    ```
    
- do ~ while문
    
    ```kotlin
    fun main( ) {
        do {
            print("Enter an integer: ")
            val input = readLine()!!.toInt()
    
            for (i in 0..(input - 1)) {
                for (j in 0..(input - 1)) print((i + j) % input + 1)
                println()
            }
        }while (input != 0)
    }
    
    // Enter an integer: 3
    // 123
    // 231
    // 312
    ```
