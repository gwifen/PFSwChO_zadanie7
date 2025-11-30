# 1. Utworzenie przestrzeni nazw remote
```
kubectl apply -f namespace-remote.yaml
```

# 2. Utworzenie Pod-a remoteweb opartego na obrazie Nginx
```
kubectl apply -f pod-remoteweb.yaml
```
## Sprawdzenie statusu:
```
kubectl get pods -n remote
```

# 3. Udostępnienie Pod-a remoteweb w formie Service typu NodePort
```
kubectl apply -f service-remoteweb.yaml 
```
Tworzy usługę remoteweb-svc, która umożliwia dostęp do strony Nginx przez węzeł Minikube na porcie 31999

## Sprawdzenie usługi:
```
kubectl get svc -n remote
```

# 4. Utworzenie Pod-a testowego testpod na obrazie Busybox
```
kubectl apply -f pod-testpod.yaml
```

## Sprawdzenie statusu:
```
kubectl get pods
```

# 5. Test połączenia z Pod-a testpod
```
kubectl exec -it testpod -- sh
```
```
wget --spider --timeout=1 remoteweb-svc.remote.svc.cluster.local
```

# 6. Weryfikacja dostępności strony Nginx z węzła Minikube
Ponieważ Minikube działa w trybie Docker driver, port NodePort (31999) nie jest wystawiany bezpośrednio na hoście.
```
minikube service remoteweb-svc -n remote --url
```
```
curl <url>
```
