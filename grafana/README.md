
# grafana 구성

prometheus 에서 수집되는 데이터를 grafana에서 시각화하여 보여주는 도구이며 알림설정을 통해 여러 채널에 알람을 보낼 수 있다.
본 구성에서는 K8s 클러스터/Springboot 어플리케이션을 모니터링/알림 기능 구현을 보여준다.

## grafana 설치

설치는 Helm 을 통해 진행함.

### grafana helm repo 추가
```text
# helm 으로 repo 추가 및 설치
$ helm repo add grafana https://grafana.github.io/helm-charts
$ helm repo list 
NAME                	URL                                               
grafana             	https://grafana.github.io/helm-charts
$ helm install grafana grafana/grafana -n monitoring
```
### grafana data 저장용 PVC 생성
```text
$ kubectl create -f grafana-pv.yaml
$ kubectl create -f grafana-pvc.yaml
$ kubectl get pvc -n monitoring
NAME                      STATUS   VOLUME                       CAPACITY   ACCESS MODES   STORAGECLASS   AGE
grafana                   Bound    grafana-pv                   10Gi       RWX                           8h
```

### grafana deployment 수정(PVC mapping)
```text
$ kubectl edit deployment grafana -n monitoring

124       - name: storage
125         persistentVolumeClaim:
126           claimName: grafana

```
## grafana 대쉬보드 구성

### K8s 클러스터 전체 모니터링 구성

### SpringBoot 샘플어플리케이션 구성

## grafana 알림설정 설정

### 알림채널 등록

### 알림확인

