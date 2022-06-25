# k8s 환경에서 SprintBoot 어플리케이션 배포하기

* 시스템 구성 Component

| Component       | 용도              |          버전 |
| :----------     | :---------------: | :-----------: |
| Kubernetes      | 인프라/서비스 배포 |    git status |
| Grafana         |   모니터링 대쉬보드|   V8.5.3      |
| Prometheus      |   데이터 수집      |      git diff |
| Tekton pipeline |    CI/CD 구현     |      v0.27.0   |
| Tekton dashboard|  tekton 대쉬보드  |       v0.31.4  |


## k8s 구성도

아래와 같이 구성됨



## 어플리케이션 구성

* 샘플 어플리케이션은 https://javatodev.com/spring-boot-mysql 참고하여 작성됨
* 기본적인 도서대출 CRUD API로 구성됨.

* 어플리케이션 구성 Component

| Component       | 용도                              |          설명         |
| :-------------  | :--------------------------------| ----------------------:|
| JDK             |  Java 개발 환경                   |   openjdk 1.8          | 
| SprintBoot      |  Java 프레임워크                  |    v2.7.0              | 
| Sprint Data JPA | JPA기반 repository 생성 Library   |    v2.7.0              | 
| LomBok          | 반복적인 코드를 줄이는 Library    |      1.18.24            | 
| Actuator        | 어플리케이션 모니터링을 제공하게 해주는 Library |v2.7.0      | 
| MariaDB         |  RDBMS                           |    10.3                | 


* 도서대출 API 목록

| URI                        | HTTP 메서드 |          설명           |
| :----------               | :----------:| :----------------------:|
|/api/library/book           |   GET       |  도서 전체 조회          |
|/api/library/book?isbn=1919 |   GET       |  도서 ISBN으로 도서 조회 |
|/api/library/boot/:id       |   GET       |  도서ID로 조회           |
|/api/library/book           |   POST      |  도서 등록               |
|/api/library/boot/:id       |   DELETE    |  도서 삭제               |
|/api/library/book/lend      |   POST      |  도서 대출               |
|/api/library/member         |   GET       |  회원 전체 조회          |
|/api/library/member/:id     |   GET       |  회원ID로 조회           |



## API 확인

* Author 추가
<img src="images/spring-api-post.jpg" align="center" />

* Author 리스트 확인
<img src="images/spring-api-get.jpg" align="center" />

## Custom API Endpoint 추가

### Actuator 설정
* application.properties에 endpoint 지정하면 /actuator/prometheus로 Metric을 노출한다.
```text
spring.datasource.url=jdbc:mysql://10.65.40.100:3306/wspark
spring.datasource.username=root
spring.datasource.password=paasword
spring.jpa.hibernate.ddl-auto=update
management.endpoints.web.exposure.include=health,info,prometheus //추가
```

* Timed 어노테이션을 Controller 단에 추가하여 API 메서드의 수행시간을 알 수 있음

```text

import io.micrometer.core.annotation.Timed;

    @Timed(value = "get-book")    //추가부분으로 get-book으로 prometheus가 수집
    @GetMapping("/book")
    public ResponseEntity readBooks(@RequestParam(required = false) String isbn) {
        if (isbn == null) {
            return ResponseEntity.ok(libraryService.readBooks());
        }
        return ResponseEntity.ok(libraryService.readBook(isbn));
    }
```


### Prometheus 설정

* prometheus가 어플리케이션의 metric을 수집할 어플리케이션 target 정보를 입력함.
* helm 으로 설치시 chart의 value.yaml에 추가하며 기본 설치시 prometheus.yaml 에 추가한다.
```text
    scrape_configs:
      - job_name: prometheus
        static_configs:
          - targets:
            - localhost:9090
        //추가부분
      - job_name: k8s-sample-app
        metrics_path: /actuator/prometheus
        static_configs:
          - targets:
            -  10.65.41.81:31236
```
