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

6. [Persistent Volume claim](db-per-volume-claim.yaml)

7. Initialized the env variables in [server](./server-deployment.yaml) and [worker](./worker-deployment.yaml) pods.

8. Next moving on to creating secrets using kubectl.

   ``kubectl create secret generic pgpassword --from-literal PGPASSWORD=12345asdf``

   other types: ``generic``, ``docker-registry``, ``tls``

   can view the secret using:
   ``kubectl get secrets``
9. Next we create the [Ingress](./ingress-service.yaml) controller and not the Load balancer for routing traffic. Load balancers are 
   legacy/deprecated and are not recommended for use anymore.

Persistent Volume Claim
--

Right now if postgres fails the whole data is lost. To save that we use volumes to save data to the host machine.

PROBLEM:

![postgres1](../images/postgres-deployment-1.png?raw=true "postgres")

![postgres2](../images/postgres-persistent-data.png?raw=true "postgres2")

SOLUTION:

![postgres3](../images/postgres-persistent-volume-solution.png?raw=true "postgres3")

Note: Volume in Kubernetes (not docker) are tied to the pods, so they will survive a pod restart however will be
destroyed if pod crashes. We are not using that.

![postgres4](../images/Volume-vs-persistent-volume.png?raw=true "postgres4")

![postgres5](../images/Volume-vs-persistent-volume-2.png?raw=true "postgres5")

Ingress Controller
--

NOTE: Ingress controllers need to be configured differently based on the platform, so the config will change for local
or Google kubernetes cluster. 

![ingress1](../images/community-Ingress.png?raw=true "ingress1")

link to git: https://github.com/kubernetes/ingress-nginx
   - This is open source and works really well. Lot of backing.


How ingress controller routes traffic?
![ingress2](../images/how-ingress-works.png?raw=true "ingress2")

The ingress controller and the thing that accepts and routes traffic is exactly the same.

```
Just in case you wanted to understand ingress-nginx a bit better,
check out this article by Hongli Lai - 
https://www.joyfulbikeshedding.com/blog/2018-03-26-studying-the-kubernetes-ingress-system.html.  
Hongli is an absolute genius, he co-created Phusion Passenger, an extremely popular webserver that 
integrates with Nginx.
```

We can Verify the service was enabled by running the following:

``kubectl get pods -n ingress-nginx``

It should show something similar:

``ingress-nginx-controller-5cc4589cc8-x7pg2   1/1     Running     0          66s``