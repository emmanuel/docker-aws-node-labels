A small Alpine based container that fetched AWS metadata for the instance it executes on and applies it as node labels in Kubernetes.  Ideally used by dropping into the `/etc/kubernetes/manifets` directory a pod spec like:

```
apiVersion: v1
kind: Pod
metadata:
  name: aws-node-labels
spec:
  hostNework: true
  restartPolicy: OnFailure
  containers:
    - name: apply-labels
      image: elevy/aws-node-labels:latest
      securityContext:
        privileged: true
      env:
        - name: CONTROLLER_ENDPOINT
          value: "${CONTROLLER_ENDPOINT}"
      volumeMounts:
        - mountPath: /etc/kubernetes/ssl
          name: kubernetes-ssl
          readOnly: true
  volumes:
    - name: kubernetes-ssl
      hostPath:
        path: /etc/kubernetes/ssl
```

Where `${CONTROLLER_ENDPOINT}` would be replaced by your Kubernetes API server endpoint