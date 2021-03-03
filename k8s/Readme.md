This folder houses all the configuration files need by kubernetes.
--
![K8s design](../images/kubernetes-design.png?raw=true "K8s design")

One separate config file for each type (total 11)
- Ingress service
- ClusterIpServices
- Deployments
- Postgres PVC

Run:
--
From the root directory type this to deploy all configs:

``kubectl apply -f k8s``

Creation:
--

Refresher for Object Types:
![object types](../images/k8s-object-types.png?raw=true "object types")

1. We first created the multi-client deployment called [client-deployment](./client-deployment.yaml). which has a 
  template for client pods. Next we create a service that exposes the client pods called 
[client-cluster-ip-service.yaml](./client-cluster-ip-service.yaml). This follows this nomenclature:
  ![ports](../images/ports-explanation.png?raw=true "ports")

2. Create a [multi server deployment](./server-deployment.yaml) and a [server service](./server-cluster-ip-service.yaml)
similarly.
![server](../images/server-deployment.png?raw=true "server")
   
3. Next is the creation of [multi-worker deployment](./worker-deployment.yaml) and its 
   [service]()