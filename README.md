# k8s-ecommerce-spring-kafka
Projeto realiza o deploy de um aplicativo com arquitetura de microserviços no kubernetes


## 1- Install Minikube
```
#!/usr/bin/env bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## 2- Install kubectl
```
#!/usr/bin/env bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

## 3- Criar cluster
```
minikube -p dev.to start --cpus 2 --memory=4096; \
minikube -p dev.to addons enable ingress; \
minikube -p dev.to addons enable metrics-server; \
kubectl create namespace dev-to
```

## 4- Clonar repositorios
```
git clone https://github.com/vitorblz/spring-kafka-microservice-payment;
git clone https://github.com/vitorblz/spring-kafka-microservice-checkout.git
```




## Comandos uteis

Ip do cluster: 
``` 
minikube -p dev.to ip
```

Ver Dashboard de um profile: 
``` 
minikube -p dev.to dashboard
```

Deploy no namespace dev.to 
``` 
kubectl apply -f k8s/postgres-checkout/
```