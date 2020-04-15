References:
* Use `envFrom` to turn key value pairs in a configmap directly as environment variables: https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#configure-all-key-value-pairs-in-a-configmap-as-container-environment-variables
* Use configMapGenerator's behavior:merge to merge different params.env files.

kubectl kustomize base:
```
apiVersion: v1
data:
  A: "1"
  B: "2"
kind: ConfigMap
metadata:
  name: example-configmap-f68btk86bd
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx-deployment
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
      - envFrom:
        - configMapRef:
            name: example-configmap-f68btk86bd
        image: nginx:1.14.2
        name: nginx
        ports:
        - containerPort: 80
```

kubectl kustomize overlays/patched:
```
apiVersion: v1
data:
  A: "1"
  B: "1"
  C: "3"
kind: ConfigMap
metadata:
  annotations: {}
  labels: {}
  name: example-configmap-269kt2fkkt
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx-deployment
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
      - envFrom:
        - configMapRef:
            name: example-configmap-269kt2fkkt
        image: nginx:1.14.2
        name: nginx
        ports:
        - containerPort: 80
```
