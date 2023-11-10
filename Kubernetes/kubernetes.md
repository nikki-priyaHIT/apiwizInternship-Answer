# 1. What are different types of services?

In Kubernetes, services are used to expose applications running in the cluster to external clients or to enable communication between different parts of the application. There are several types of services in Kubernetes:

1. **ClusterIP:**
   - **Description:** Exposes the service on a cluster-internal IP address. The service is accessible within the cluster.
   - **Use Case:** Used for communication between different parts of the application running within the cluster.

2. **NodePort:**
   - **Description:** Exposes the service on each Node's IP at a static port (NodePort). The service is accessible on each Node's IP and port combination.
   - **Use Case:** Useful for exposing a service externally for development purposes or when LoadBalancer is not available.

3. **LoadBalancer:**
   - **Description:** Exposes the service externally using a cloud provider's load balancer. The external IP is dynamically assigned.
   - **Use Case:** Ideal for exposing a service to the public internet with automatic load balancing.

4. **ExternalName:**
   - **Description:** Maps the service to the contents of the `externalName` field (e.g., `example.com`), effectively allowing the service to act as a CNAME record.
   - **Use Case:** Used when you want to direct traffic from within the cluster to a service outside the cluster.

5. **Headless Service:**
   - **Description:** Acts as a service without a cluster-internal IP, and the corresponding DNS record resolves to the set of IPs of the pods.
   - **Use Case:** Useful for stateful applications where each Pod has a unique identity and needs to be directly accessible.

6. **Ingress:**
   - **Description:** Manages external access to services within a cluster, typically HTTP. It provides HTTP and HTTPS routing to services based on rules defined by the user.
   - **Use Case:** Used to expose services externally and manage routing based on URLs.

These service types provide flexibility in exposing and accessing applications within a Kubernetes cluster, depending on the specific requirements of the application architecture and deployment environment.


# 2. What is a pod? 

In Kubernetes, a **Pod** is the smallest and simplest unit in the Kubernetes object model. It is the basic building block for running and deploying containers. A Pod represents a single instance of a running process in a cluster and can encapsulate one or more containers.

Key characteristics of a Pod:

1. **Co-located Containers:**
   - A Pod can contain one or more containers that are tightly coupled and share the same network namespace, storage, and IPC (Inter-Process Communication) namespace.
   - Containers within a Pod can communicate with each other using localhost.

2. **Shared Resources:**
   - Containers in a Pod share certain resources, such as storage volumes, that are defined at the Pod level.
   - They can also share environment variables and communicate through inter-process communication.

3. **Atomic Unit of Deployment:**
   - A Pod is the smallest deployable unit in Kubernetes. It represents a single instance of a running process.

4. **Pod Identity:**
   - Each Pod has a unique IP address within the cluster and a unique hostname.
   - Containers within a Pod share the same network namespace, so they can communicate using `localhost`.

5. **Single-Container and Multi-Container Pods:**
   - A Pod can contain a single container, and this is a common pattern.
   - It can also contain multiple containers that need to work together and share resources.

Example of a simple Pod definition:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: myimage:latest
```

This YAML manifest defines a Pod named "mypod" with a single container named "mycontainer" running the specified Docker image. This is a basic example, and more complex configurations with multiple containers and shared resources can be defined based on the application's requirements.

Pods are usually created and managed by higher-level abstractions like Deployments or StatefulSets. They provide an abstraction layer that simplifies the deployment and scaling of applications in a Kubernetes cluster.


# 3. Create a pod with the above created custom image when a pod dies k8s should automatically restart.

Certainly! To create a Pod with automatic restarts upon failure, you can use a Kubernetes resource called a **Deployment**. Deployments are a higher-level abstraction that manages the creation, scaling, and updating of Pods.

Here's an example Deployment YAML file that creates a Pod with automatic restarts:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: your-custom-image:latest
```

In this example:

- `replicas: 1` specifies that you want one replica (Pod) of your application.
- `selector` is used to match Pods managed by this Deployment based on their labels.
- `template` defines the specification of the Pod.

With this Deployment, Kubernetes will automatically restart the Pod if it fails or is terminated. To apply this configuration, save it to a file (e.g., `deployment.yaml`) and use the `kubectl apply` command:

```bash
kubectl apply -f deployment.yaml
```

Replace `your-custom-image:latest` with the actual name and version of your custom image.

To monitor the status and logs of your Pod, you can use the following commands:

```bash
# Check the status of the deployment
kubectl get deployment my-deployment

# View the logs of the Pod
kubectl logs -f <pod-name>
```

This setup ensures that if the Pod dies, the Deployment controller will automatically create a new one to maintain the desired number of replicas.


# 4. How to access the custom application with a specific port?

In Kubernetes, accessing a custom application with a specific port typically involves exposing the application using a Kubernetes Service. Here's a basic guide on how you can achieve this:

1. **Create a Deployment:**
   First, you need to create a Deployment to manage the Pods running your custom application. This is similar to the example I provided earlier for creating a Pod.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-deployment
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: my-app
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
         - name: my-container
           image: your-custom-image:latest
           ports:
           - containerPort: 8080  # Replace with the port your application is running on
   ```

   Apply the Deployment:

   ```bash
   kubectl apply -f deployment.yaml
   ```

2. **Create a Service:**
   Next, you need to create a Service to expose your application within the cluster.

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     selector:
       app: my-app
     ports:
     - protocol: TCP
       port: 80  # Port on which the service will be accessed
       targetPort: 8080  # Port on which the Pods are running
   ```

   Apply the Service:

   ```bash
   kubectl apply -f service.yaml
   ```

   In this example, the Service is configured to expose your application on port 80, and it routes traffic to the Pods on port 8080.

3. **Access the Application:**
   Once the Service is created, you can access your application using the ClusterIP of the Service.

   ```bash
   kubectl get services
   ```

   Look for the `my-service` entry, and note the `CLUSTER-IP`. You can now access your application using this IP and the specified port (e.g., `http://<CLUSTER-IP>:80`).

   If you want to access the application from outside the cluster, you might want to use a different Service type, such as `NodePort` or `LoadBalancer`. Keep in mind that these options depend on your Kubernetes environment, and `NodePort` is more suitable for development purposes.

Adjust the port numbers and configurations based on your application's needs.
