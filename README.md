## Git bash : 

#### Création et lancement de la base de données
```SH
./build.sh
./run.sh
```


#### Création du compteur : 
```SH
curl localhost:4040/count/create -X POST
```

Output : 
```SH
{"id":"31783cc3-f5a2-4ab8-9443-127293d2cce1","value":"0","created_at":"2024-05-24T10:28:37.107Z","updated_at":"2024-05-24T10:28:37.107Z"}
```

#### Lancement de count.js :
```SH 
node count.js
```

Output: 
trying to connect to  amqp://localhost:5672
send
send
send
send
send...

## Powershell :

#### Création d'un namespace :
```SH
kubectl create namespace np-kch

kubectl --kubeconfig .\kubeconfig.yml apply -f .\kch-pod.yaml
```

#### Vérification de la présence du pod : 
```SH
kubectl --kubeconfig .\kubeconfig.yml get pods
```

#### Lancement de Rabbit
```SH
docker run -d --name rabbitmq -p 5672:5672 rabbitmq
```

#### On vérifie que rabbit est bien lancé avec la commande 'docker ps', output :

CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS         PORTS                   
                                                 NAMES
befeb66a371b   rabbitmq           "docker-entrypoint.s…"   11 seconds ago   Up 9 seconds   4369/tcp, 5671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:5672->5672/tcp   rabbitmq
64f8531acd1a   counter-database   "docker-entrypoint.s…"   3 hours ago      Up 3 hours     0.0.0.0:5432->5432/tcp                                                   counter-database


#### Lancement 
```SH
npm start
```
```SH 
Output :
> lambda@1.0.0 start
> ts-node src/main

[2024-05-24T12:16:46.869Z](at Object):  {
  user: 'count',
  host: 'localhost',
  database: 'count',
  port: '5432',
  password: 'count'
}
[2024-05-24T12:16:47.044Z](at listenRabbit):  trying to connect to  amqp://localhost:5672
[2024-05-24T12:16:47.048Z](at origin):  undefined
--> Routes:  {
  GET: [ '/', '/routes', '/count', '/count/:id' ],
  POST: [ '/count/:id/add', '/count/:id/reset', '/count/create' ],
  PATCH: [ '/count/delete' ],
  NOT_FOUND: [ '/' ]
}
Postgres: Connection success
[2024-05-24T12:16:47.121Z](at listenRabbit):  connected to  amqp://localhost:5672
[2024-05-24T12:28:31.298Z](at create):  create
[2024-05-24T12:28:31.322Z] ----->  [200] [post#/count/create]
[2024-05-24T12:28:31.323Z] ----->    request.body = null
```

#### Application du fichier déployment :
```SH
kubectl --kubeconfig .\kubeconfig.yml apply -f ./kch-deployment.yaml
```

On vérifie qu'il est bien présent : 
```SH 
kubectl --kubeconfig .\kubeconfig.yml get deployments --namespace=np-kch
```

Output : 
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
kch-database-deployment   0/1     1            0           68s