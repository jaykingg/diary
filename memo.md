2022.09.19  
Kotlin Collection 에서 Map과 Filter를 살펴보던도중 의문이 생겼다.  
2개의 연산을 할 경우 순서에 따라 성능이가 발생할까? 그래서 냅다해봄.
```kotlin
val list2 = listOf("apple", "airplane", "before", "circle")

val measuredTime = measureTimedValue {
    list2.filter { it.startsWith("a") }.map { it.uppercase() }
}
measuredTime.also(::println)

val measuredTime2 = measureTimedValue {
    list2.map { it.uppercase() }.filter { it.startsWith("A") }

}
measuredTime2.also(::println)
```
위와 같이 "a"로 시작하는 요소를 대문자로 바꾸는 코드이다.  

성능차이가 발생하는 이유는 생각해보면 간단하다.  
map은 새로운 배열을 반환한다.  
filter는 조건에 부합하는 요소를 모아 배열을 반환한다. 

filter-> map의 경우  
filter (4) + map(2) =  총 6번의 연사늘 거친다.   
map -> filter의 경우  
map(4) + filter(4) = 총 8번의 연산을 거친다.   

따라서 복합적인 연산을 수행할 경우 순서를 바꾸어 성능을 향상 시킬 수 있다.  
단, block에 들어가는 코드자체의 속도에 따라 위의 내용이 상반될 수 있다. 