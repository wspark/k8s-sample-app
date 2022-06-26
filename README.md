# k8s 환경에서 Prometheus 기반 모니터링 환경구성


## k8s 구성도




## k8s 시스템 구성

| Component       | 용도              |          버전 |
| :----------     | :---------------: | :-----------: |
| [Kubernetes](https://github.com/wspark/k8s-sample-app/tree/main/k8s)      | 인프라/서비스 배포 |    v1.24      |
| [Grafana](https://github.com/wspark/k8s-sample-app/tree/main/grafana)         |   모니터링 대쉬보드|   V8.5.3      |
| [Prometheus](https://github.com/wspark/k8s-sample-app/tree/main/prometheus)    |   데이터 수집      |    2.x        |
| [Tekton pipeline](https://github.com/wspark/k8s-sample-app/tree/main/tekton) |    CI/CD 구현     |      v0.27.0   |

## 기타시스템 구성

| Component      | IP             | 용도                             |          버전         |
| :------------- | :----------    | :--------------------------------| ---------------------:|
| Bastion        | 10.65.41.80    |  k8s 클러스터 관리/NFS 저장소     |    CentOS 7.9         | 
| nGrinder       | 10.65.41.80    |  성능테스트 Tool                  |   v3.5.5-p1          | 
| MariaDB        | 10.65.40.100   |  RDBMS                           |     10.3             | 


## 어플리케이션 구성

* 샘플 어플리케이션은 https://javatodev.com/spring-boot-mysql 참고하여 작성
* 도서대출 CRUD API로 구성됨.
* 어플리케이션 구성 Component

| Component       | 용도                              |          설명         |
| :-------------  | :--------------------------------| ----------------------:|
| JDK             |  Java 개발 환경                   |   openjdk 1.8          | 
| [SprintBoot](https://github.com/wspark/k8s-sample-app/tree/main/springboot-sample)     |  Java 프레임워크                  |    v2.7.0              | 
| Sprint Data JPA | JPA기반 repository 생성 Library   |    v2.7.0              | 
| LomBok          | 반복적인 코드를 줄이는 Library    |      1.18.24            | 
| Actuator        | 어플리케이션 모니터링을 제공하게 해주는 Library |v2.7.0      | 



## 어플리케이션 성능테스트

### 성능테스트용 tool(ngrinder?)

* 설치
```
java -jar -XX:MaxPermSize=512m ngrinder-controller-3.5.5-p1.war
```
* Admin 접속 : http://10.65.41.80:8080

* Controller 

### 시나리오 작성

### 부하발생






## TO-DO list
* 로깅시스템 구성(EFK)
* Istio 구성
* Argocd 구성


