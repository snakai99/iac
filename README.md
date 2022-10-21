## 動画
https://event.cloudnativedays.jp/cndt2020/talks/47

## Docker Desktop

  https://www.docker.com/products/docker-desktop/


## Home Brew
  
  ```
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
  
  参考
  ```
  echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/xxxxx/.zprofile
  echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/xxxxx/.zprofile
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
kind delete cluster
kind create cluster --config=cluster.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

cat << EOF >dashboard-adminuser.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
kubectl apply -f dashboard-adminuser.yaml 
kubectl -n kubernetes-dashboard create token admin-user
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
kubectl apply -f app-nginx.yaml
```

動作確認
```
kubectl run temp --image=curlimages/curl --rm --restart=Never -it -- curl http://nginx-service:8080
```
```
kubectl proxy &
open http://127.0.0.1:8001/api/v1/namespaces/default/services/nginx-service:8080/proxy/
```

## おまけ

https://github.com/snakai99/iac/blob/main/README.md

https://speakerdeck.com/masatoshitada/kubernetes-basics-for-application-developers

```
cat <<EOF> app-httpd.yaml
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

.zshrc
```
autoload -Uz compinit
compinit

source <(kubectl completion zsh)

alias k=kubectl
```

```
cat <<EOF> app-httpd.yaml
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
        image: httpd
        ports:
        - containerPort: 80
EOF
```
```
kubectl apply -f app-httpd.yaml
```
```
cat <<EOF> start
open https://github.com/snakai99/iac/blob/main/README.md
EOF
chmod +x start
```
