## Docker Desktop
  ```
  https://www.docker.com/products/docker-desktop/
  ```

## Home Brew
  
  ```
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
  ```
  echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/snakai99/.zprofile
  echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/snakai99/.zprofile
  eval "$(/opt/homebrew/bin/brew shellenv)"
  ```
 https://brew.sh/index_ja
 

## kind
### kindのインストール
```
brew install kind
```
### clusterの作成
```
cat <<EOF> cluster.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 30080
    hostPort: 30070
- role: worker
EOF
```
```
kind create cluster --config=cluster.yaml
```

参考
```
kind delete cluster
```

## マニフェスト作成、kubectl実行
```
cat <<EOF> app-nginx.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
  - port: 8080
    targetPort: 80
EOF
```
```
kubectl apply -f app-nginx.yaml
```







## おまけ

https://github.com/snakai99/iac/blob/main/README.md

https://speakerdeck.com/masatoshitada/kubernetes-basics-for-application-developers

```
cat <<EOF> app-nginx.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: web-nginx
  name: web-nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-nginx
  template:
    metadata:
      labels:
        app: web-nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web-nginx
spec:
  selector:
    app: web-nginx
  type: NodePort
  ports:
    - port: 80
      nodePort: 30080
EOF
```
