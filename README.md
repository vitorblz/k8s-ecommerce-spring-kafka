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


## 5- Gerar jar da aplicacao de checkout e buildar imagem docker
```
cd spring-kafka-microservice-checkout;
./gradlew build -x test;
docker build --force-rm -t java-checkout ./spring-kafka-microservice-checkout
minikube -p dev.to image load java-checkout:latest
minikube cache add java-checkout:latest
```

## 5- Gerar jar da aplicacao de payment e buildar imagem docker
```
cd spring-kafka-microservice-payment;
./gradlew build -x test;
docker build --force-rm -t java-payment ./spring-kafka-microservice-payment
minikube cache add java-payment:latest
```

## Comandos uteis

Inicializa o cluster
``` 
minikube -p dev.to start
``` 

Logs das do que esta rodando no cluster 
```
minikube -p dev.to logs
```

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

Acessa maquina do cluster
``` 
minikube -p dev.to ssh
```


Serviços executando
``` 
kubectl get services -n dev-to
```

URL de acesso
```
minikube -p dev.to service -n dev-to db-checkout --url
```

Delete minicube profile
```
minikube -p dev.to delete
```




## Facilitador de criação e configuracao Kafka
Referencia: https://strimzi.io/quickstarts/






Verificar porque não estava funcionando. Faz o servidor java parar:
```
            livenessProbe:
              httpGet:
                path: /app/actuator/health/liveness
                port: 8080
              initialDelaySeconds: 30
            readinessProbe:
              httpGet:
                path: /app/actuator/health/readiness
                port: 8080
              initialDelaySeconds: 30
```