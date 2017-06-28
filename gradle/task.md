# Task
## 의존관계
Tasks 간에는 의존관계가 존재할 수 있다. 의존관계가 있을 경우 선행 taks가 완료된 후 task가 실행된다. 

**_선행 tasks는 알아서 실행된다._**

### Task graph
gradle은 실행하는 task를 분석 해 Directed Acyclic Graph를 구성한다. 이 그래프를 가지고 제일 앞의 task부터 실행한다.
### 제약
#### Task는 1회만 실행
## Listing tasks
`build.gradle`파일이 있는 디렉토리에서
```
$ gradle tasks
```
## 플러그인
플러그인을 추가하면 태스크가 추가 된다.
