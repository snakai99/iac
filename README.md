- Home Brew

  https://brew.sh/index_ja
  
  ```
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
  ```
  echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/snakai99/.zprofile
  echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/snakai99/.zprofile
  eval "$(/opt/homebrew/bin/brew shellenv)"
  ```
- Docker Desktop
```
https://www.docker.com/products/docker-desktop/
```

-- kind
```
cat <<EOF> c.yaml
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
cat <<EOF> d.yaml
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
