---
{"dg-publish":true,"permalink":"/java/build/java-intelli-j/","dgPassFrontmatter":true,"noteIcon":""}
---


1. 오른쪽 상단에 있는 Gradle 탭을 클릭 (창이 열리지 않으면 View > Tool Windows > Gradle을 선택

2.  Tasks > build를 찾고, 그 아래에 있는 build 작업을 더블 클릭

3. 빌드가 완료되면  build/libs 폴더에 파일이 생성된 걸 볼 수 있음


출처: [https://maketh.tistory.com/228](https://maketh.tistory.com/228) [Manners maketh man:티스토리]


### 2개의 파일이 생성되는 이유
빌드를 하면 실제로 실행할 **jar파일**과 **plain.jar파일** 총 2개가 생성된다.

*plan.jar파일이란?*
	프로젝트와 관련된 (의존하는) 라이브러리를 포함하지 않고 작성한 코드의 클래스파일과 리소스 파일만 포함된 파일이다. 

아래 코드를 `build.gradle` 파일에 추가해주면 빌드시에 *plan.jar* 파일이 생성되지 않게 해준다.
```java
jar{
	enabled = false
}
```





