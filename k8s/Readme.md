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


3. Next is the creation of [multi-worker deployment](./worker-deployment.yaml). It has no service as it does not need to
   communicate with anyone else.


4. Redis [Deployment](./redis-deployment.yaml) and [Service](./redis-cluster-ip-service.yaml)


5. Postgres [Deployment](./postgres-deployment.yaml) and its [service](./postgres-cluster-ip-service.yaml). We also need
   to add in the environment variables to connect with different services.



Persistent Volume Claim:
    - Right now if postgres fails the whole data is lost. To save that we use volumes to save data to the host machine.
      

PROBLEM:
![postgres1](../images/postgres-deployment-1.png?raw=true "postgres")
     
![postgres2](../images/postgres-persistent-data.png?raw=true "postgres2")

SOLUTION:

![postgres3](../images/postgres-persistent-volume-solution.png?raw=true "postgres3")

Note: Volume in Kubernetes (not docker) are tied to the pods, so they will survive a pod restart however will be
destroyed if pod crashes. We are not using that.

![postgres4](../images/Volume-vs-persistent-volume.png?raw=true "postgres4")

![postgres5](../images/Volume-vs-persistent-volume-2.png?raw=true "postgres5")
