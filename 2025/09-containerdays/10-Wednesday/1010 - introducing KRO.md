# Introducing Kubernetes Resource Orchestrator (Abdel Sghiouar)

<https://github.com/kro-run/kro>

> KRO is the solution to the terraform problem :)

writing/maintaining CRDs and operators can be challenging

`ResourceGraphDefinition` is a kro crds which you use to specify a new data model / CRD

```yaml
apiVersion: kro.run/v1alpha1
kind: ResourceGraphDefinition
metadata:
  name: my-application
spec:
  # kro uses this simple schema to create your CRD schema and apply it
  # The schema defines what users can provide when they instantiate the RGD (create an instance).
  schema:
    apiVersion: v1alpha1
    kind: Application
    spec:
      # Spec fields that users can provide.
      name: string
      image: string | default="nginx"
      ingress:
        enabled: boolean | default=false
    status:
      # Fields the controller will inject into instances status.
      deploymentConditions: ${deployment.status.conditions}
      availableReplicas: ${deployment.status.availableReplicas}

  # Define the resources this API will manage.
  resources:
    - id: deployment
      template:
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: ${schema.spec.name} # Use the name provided by user
        spec:
          replicas: 3
          selector:
            matchLabels:
              app: ${schema.spec.name}
          template:
            metadata:
              labels:
                app: ${schema.spec.name}
            spec:
              containers:
                - name: ${schema.spec.name}
                  image: ${schema.spec.image} # Use the image provided by user
                  ports:
                    - containerPort: 80

    - id: service
      template:
        apiVersion: v1
        kind: Service
        metadata:
          name: ${schema.spec.name}-service
        spec:
          selector: ${deployment.spec.selector.matchLabels} # Use the deployment selector
          ports:
            - protocol: TCP
              port: 80
              targetPort: 80
```

then you can create a new instance of the KRO type with:

```yaml
apiVersion: kro.run/v1alpha1
kind: Application
metadata:
  name: my-application-instance
spec:
  name: my-awesome-app
  ingress:
    enabled: true
```

typically, `kro` takes care of providing your custom CRD. in some cases it
could also create some simple k8s native manifests.

more examples: <https://kro.run/examples/basic/optionals>
