## Create manifest files for out application
Project one is about creating kubernetes manifest files of our application (pods, deployments and services..)
Then expose our application externally using ingress

- We are going to use Nginx ingress controller to expose out application externally
- Using AWS loadbalancer controller for loadbalancing services






## Install NGINX Controller  (in case you do not have )
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx                        
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.service.type=LoadBalancer \
  --set controller.service.annotations."service\.beta\.kubernetes\.io/aws-load-balancer-type"="nlb"
```

## Deploy your application
```bash
kubectl apply -f *
```

## Write an ingress resources for your application

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-app-ingress
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: *
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-service
            port:
              number: 80
```




