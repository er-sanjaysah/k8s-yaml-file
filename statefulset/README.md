kubectl apply -f mongo-service.yaml
kubectl apply -f mongo-statefulset.yaml

5. Initialize the Replica Set

kubectl exec -it mongo-0 -- mongosh

Run inside Mongo shell:
>>>>>>>>>>>>>>>>>>>>>>>>>
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongo-0.mongo.mongo.svc.cluster.local:27017" },
    { _id: 1, host: "mongo-1.mongo.mongo.svc.cluster.local:27017" },
    { _id: 2, host: "mongo-2.mongo.mongo.svc.cluster.local:27017" }
  ]
})

>>>>>>>>>>>>>>>>>>>>>>>>

rs.status()


6. Access the cluster

kubectl run --rm -it mongo-client --image=mongo:6.0 -- bash

mongosh "mongodb://mongo-0.mongo:27017,mongo-1.mongo:27017,mongo-2.mongo:27017/?replicaSet=rs0"

>>>>>>>>>>>>>>>>>>>>>>
From outside the cluster if you want to access mongdb then you have to do port-forward

You’d need to:

Port-forward:

kubectl port-forward svc/mongo 27017:27017 -n mongo


Then connect from your laptop:

mongosh "mongodb://localhost:27017"

>>>>>>>>>>>>>>>>>>>>>>>>>>>
