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
cat <<EOF> c.xml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
EOF
```
