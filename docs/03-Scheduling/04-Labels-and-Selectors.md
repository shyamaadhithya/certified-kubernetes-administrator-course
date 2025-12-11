# Labels and Selectors

_Take me to [Video Tutorial](https://kodekloud.com/topic/labels-and-selectors/)_
  
In this section, we will take a look at **`Labels and Selectors`**

### What are Labels?
`Labels` are **key-value** pairs attached to Kubernetes objects (`Pods`, `Deployments`, `Services`, `Nodes`, etc.).

They are used to identify, group, and select objects.

Example of labels:
```yaml linenums="1"
metadata:
  labels:
    app: web
    tier: frontend
```

  ![labels-ckc](../../images/labels-ckc.PNG)

Labels do NOTHING by themselves. They simply **tag** an object with information.

### What is a Selector?

A selector finds objects that match certain labels.

  ![sl](../../images/sl.PNG)

It is used by:

- ReplicaSet
- Deployment
- Service
- HPA
- NetworkPolicy
- Jobs
- StatefulSets

Selectors connect workloads to Pods.  

Example: Deployment using `labels` and `selector`
```yaml title="deployment.yaml" linenums="1"
spec:
  selector:
    matchLabels:
      app: web                 # selector
  template:
    metadata:
      labels:
        app: web               # labels on Pod
```

**Relationship:**

- Deployment `selector` → find Pods with **app=web**
- Deployment `template` `labels` → create Pods with **app=web**

If `labels` do not match the `selector` → Deployment will NOT manage those Pods.

### How are labels and selectors are used in kubernetes?
- We have created different types of objects in kubernetes such as **`PODs`**, **`ReplicaSets`**, **`Deployments`** etc.
  
  ![ls](../../images/ls.PNG)
  
How do you specify labels?
   ```yaml linenums="1" title="pod-definition.yaml"
    apiVersion: v1
    kind: Pod
    metadata:
     name: simple-webapp
     labels:
       app: App1
       function: Front-end
    spec:
     containers:
     - name: simple-webapp
       image: simple-webapp
       ports:
       - containerPort: 8080
   ```
 ![lpod](../../images/lpod.PNG)
 
Once the pod is created, to select the pod with labels run the below command
```bash
$ kubectl get pods --selector app=App1
```

Kubernetes uses labels to connect different objects together
   ```yaml title="replicaset.yaml" linenums="1"
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: simple-webapp
      labels:
        app: App1
        function: Front-end
    spec:
     replicas: 3
     selector:
       matchLabels:
        app: App1
     template:
       metadata:
         labels:
           app: App1
           function: Front-end
       spec:
         containers:
         - name: simple-webapp
           image: simple-webapp   
   ```

  ![lrs](../../images/lrs.PNG)

For services
 
      ```yaml title="services.yaml" linenums="1"
      apiVersion: v1
      kind: Service
      metadata:
       name: my-service
      spec:
       selector:
         app: App1
       ports:
       - protocol: TCP
         port: 80
         targetPort: 9376 
       ```
       
  ![lrs1](../../images/lrs1.PNG)
  
## Annotations
- While labels and selectors are used to group objects, annotations are used to record other details for informative purpose.
    ```yaml title="annotation.yaml" linenums="1"
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: simple-webapp
      labels:
        app: App1
        function: Front-end
      annotations:
         buildversion: 1.34
    spec:
     replicas: 3
     selector:
       matchLabels:
        app: App1
    template:
      metadata:
        labels:
          app: App1
          function: Front-end
      spec:
        containers:
        - name: simple-webapp
          image: simple-webapp   
    ```
  ![annotations](../../images/annotations.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
