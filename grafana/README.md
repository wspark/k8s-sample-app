
### grafana 설치

#### grafana helm repo 추가
```text
# helm 으로 repo 추가 및 설치
$ helm repo add grafana https://grafana.github.io/helm-charts
$ helm repo list 
NAME                	URL                                               
grafana             	https://grafana.github.io/helm-charts
$ helm install grafana grafana/grafana -n monitoring
```
#### grafana data 저장용 PVC 생성
```text
$ kubectl create -f grafana-pv.yaml
$ kubectl create -f grafana-pvc.yaml
$ kubectl get pvc -n monitoring
NAME                      STATUS   VOLUME                       CAPACITY   ACCESS MODES   STORAGECLASS   AGE
grafana                   Bound    grafana-pv                   10Gi       RWX                           8h
```

#### grafana deployment 수정(PVC mapping)
```text
$ kubectl edit deployment grafana -n monitoring

124       - name: storage
125         persistentVolumeClaim:
126           claimName: grafana

```
### grafana dashboard


### grafana alert설정

