# MySQL Operator Minikube install

## kubectl을 사용한 설치 방법

**1. CRD(Custom Resource Definition) 설치**
```bash
kubectl apply -f https://raw.githubusercontent.com/mysql/mysql-operator/9.2.0-2.2.3/deploy/deploy-crds.yaml
```

**2. MySQL Operator 설치**
```bash
kubectl apply -f https://raw.githubusercontent.com/mysql/mysql-operator/9.2.0-2.2.3/deploy/deploy-operator.yaml
```

**3. Operator 설치 확인**
```bash
kubectl get deployment -n mysql-operator mysql-operator
```

## MySQL InnoDB Cluster 설치

**1. MySQL root 사용자 시크릿 생성**
```bash
kubectl create secret generic mypwds \
  --from-literal=rootUser=root \
  --from-literal=rootHost=% \
  --from-literal=rootPassword="password"
```

**2. InnoDB Cluster 설정 파일 생성 (mycluster.yaml)**
```yaml
apiVersion: mysql.oracle.com/v2
kind: InnoDBCluster
metadata:
  name: mycluster
spec:
  secretName: mypwds
  tlsUseSelfSigned: true
  instances: 3
  router:
    instances: 1
```

**3. InnoDB Cluster 배포**
```bash
kubectl apply -f mycluster.yaml
```

**4. 클러스터 상태 확인**
```bash
kubectl get innodbcluster --watch
```
