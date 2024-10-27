---
{"dg-publish":true,"permalink":"/cs/java/error-exception/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-28T04:17:43.701+09:00","updated":"2024-10-28T04:30:43.908+09:00"}
---

## Error
프로그램 수준에서 ==복구하기 어려운 심각한 문제==로, JVM 오류가 많음


## Exception
프로그램에서 예상 가능한 문제로, ==예외 처리를 통해 복구하거나 회피할 수 있는 오류==

- Checked Exception (체크드 예외)
    
    - ==예상 가능한 위험을 코드에서 처리하게 하려는 것==으로 예외가 발생할 가능성이 있는 코드는 `try-catch`로 처리하거나 `throws` 키워드로 선언해야함
    - 파일 읽기/쓰기, 데이터베이스 연결, 네트워크 통신등과 같이 <u>외부 환경에 의존하는 작업</u>에서 발생할 가능성이 높은 예외들이 해당됨
    - 예시: `IOException`, `SQLException` 등.
    
- Unchecked Exception (언체크드 예외)
	- ==잘못된 코드에서 발생하는 오류를 잡기 위해 사용==
    - 컴파일러가 예외 처리를 강제하지 않으며, 런타임에 발생하는 예외
    - 개발자의 부주의나 로직 오류로 발생하는 경우가 많음
    - 예시: `NullPointerException`, `ArrayIndexOutOfBoundsException` 등.


## Error와 Exception의 차이 요약

| 구분       | Error                                    | Exception                              |
| -------- | ---------------------------------------- | -------------------------------------- |
| 정의       | JVM에서 발생하는 치명적인 문제                       | 예측 가능한 문제로, 복구 가능성이 있는 예외              |
| 복구 가능 여부 | 복구 불가능                                   | 복구 가능                                  |
| 예시       | `OutOfMemoryError`, `StackOverflowError` | `IOException`, `NullPointerException`  |
| 처리 방식    | 일반적으로 처리하지 않음                            | `try-catch`로 예외 처리 가능                  |
| 유형       | 없음                                       | Checked Exception, Unchecked Exception |
