# L5 - 2019

## Scope
1. Kubernetes
2. K3s
3. Helm
4. Docker
5. Application deployment

## Tasks
1. **Infrastructure setup**

    - using your AWS account create at least 3 EC2 machines with Ubuntu 18.04 (or any other OS you are familiar with)
    - one machine will act as the Kubernetes master, the other ones will be worker nodes
    - choose appropriate machine flavours (at least 1 CPU and 512 MB RAM)

2. **K3s cluster installation**

    - K3s is a lightweight version of Kubernetes, which is also easier to setup
    - read K3s' [documentation](https://rancher.com/docs/k3s/latest/en/quick-start/)
    - install all required software on the machines (including containerd and Docker)
    - following the guide, provision a K3s cluster with one master node and 2 worker nodes
    - remember to set appropriate firewall rules!
    - while installing the K3s agents make sure those will use Docker (see documentation)
    - obtain `kubeconfig` file and save it to your local machine (also install `kubectl`)
    - instead of manually installing K3s you can also use [k3sup](https://github.com/alexellis/k3sup)

3. **Kubernetes objects & manifests**

    - [documentation](https://kubernetes.io/docs/concepts/)
    - what is the difference between a Docker container and a Kubernetes Pod?
    - what are the following Kubernetes objects and what are they used for?
        - deployment
        - service
        - daemonset
        - statefulset
        - configmap
        - secret
        - persistentvolume(claim)
    - how to deploy these objects to your Kubernetes cluster?
    - deploy the provided sample manifests (`examples/plain_manifests`),
      check if they were correctly deployed (`kubectl get`),
      explain what these manifests do,
      eventually, delete them from your cluster

4. **Helm**

    - [documentation](https://helm.sh/docs/)
    - in practice, you don't write manifests with hardcoded values and don't deploy 
      them manually one after the other
    - Helm is a package manager for Kubernetes application deployments
    - it provides a template engine for Kubernetes manifests and manages deployments (rolling updates, rollbacks etc.)
    - install the Helm CLI
    - deploy the provided sample Helm chart (`examples/helm`),
      check if application and all its components were deployed correctly,
      eventually, remove the application from your cluster
    - explain how to use values defined in `values.yaml` inside your manifests
    - how to use 3rd party Helm charts in your chart?
    - you can find many existing charts in [Helm chart repository](https://github.com/helm/charts/tree/master/stable)

5. **Application chart**

    - prepare Docker images for our Reddit Scraper application - for all custom images (celery workers, scheduler etc.):
        - option A: push them as public images on your Docker Hub
        - option B: clone the repo to the K3s worker nodes and build the Dockerfiles there
    - create a Helm chart for our application
    - decide which components ("services" in Docker-Compose) are already present as Helm charts and include them in our chart
    - for other components write appropriate Helm templates (deployments, services, etc.) 
    - make sure to handle volumes! (for simplicity you can use hostPaths, but in practice, you should use a Volume Provider, like EBS, Cinder, NFS etc.)
    - add appropriate liveness and readiness probes

6. **Application deployment**
    - deploy the prepared application chart to your Kubernetes cluster
    - check if everything works fine
    - make some change in the `values.yaml` file (e.g. increase the replicas of the text embedding worker) and perform a rolling update
    - rollback the change

7. **Application check**
    - make sure that everything is working correctly
        - data is collected
        - you can see changes on the Grafana dashboard
        - you can see changes on the Redash dashboard

