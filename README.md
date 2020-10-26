# Elasticsearch Kubernetes

This formula will help you to deploy and test a Kubernetes-ready Elasticsearch cluster. With this setup you will get:

- Elasticsearch 7.9.3
  - 2 master nodes
  - 1 data node
  - 1 client (coordination-only) node
- Customizable Elasticsearch configuration with predefined values

## Prerequisites

You need to have a running Kubernetes cluster on your system (version 1.17 or newer).
If you want to run it locally right on your PC or laptop, you can use [minikube](https://github.com/kubernetes/minikube), [kind](https://kind.sigs.k8s.io/), or [docker-desktop](https://www.docker.com/products/docker-desktop). Personally, I use `docker-desktop` since I'm using Windows 10, it integrates seamlessly with the running Ubuntu instance on WSL2.

You can check the version using `kubectl version` command.

```bash
kubectl version

Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.0", GitCommit:"70132b0f130acc0bed193d9ba59dd186f0e634cf", GitTreeState:"clean", BuildDate:"2019-12-07T21:20:10Z", GoVersion:"go1.13.4", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.8", GitCommit:"9f2892aab98fe339f3bd70e3c470144299398ace", GitTreeState:"clean", BuildDate:"2020-08-13T16:04:18Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
```

Make sure your kubectl has a `kustomize` command.

```bash
kubectl kustomize --help

# make sure running this command doesn't produce any errors.
```

## Deploying

You should fulfill the prerequisites above before proceeding.

```bash
# we're going to use a namespace called `monitoring`
# so first create it if you don't have one
kubectl create namespace monitoring

# clone this repo
git clone https://github.com/husniadil/elasticsearch-kubernetes

# go to elasticsearch-kubernetes directory
cd elasticsearch-kubernetes

# apply kubernetes resource config
# note: dot after -k indicates current directory
kubectl apply -k .

# check status until you see that all pods have been run
# keep checking
kubectl get all -n monitoring

# once all pods are ready, you can access it via load-balancer host
curl http://elasticsearch.monitoring.svc.cluster.local
# or load-balancer IP
curl http://10.103.9.167
```

[![Deployment](https://user-images.githubusercontent.com/10581130/97151810-9bbe1a80-17a2-11eb-95d4-9bb0c209cea8.gif)](https://asciinema.org/a/nv6eyG8PEpOU1XpljK5UZ1mOR)

## Destroying

Destroying your cluster is simple as well. Make sure that you know what you are doing.

```bash
# note: dot after -k indicates current directory
kubectl delete -k .
```

[![Deployment](https://user-images.githubusercontent.com/10581130/97151797-9791fd00-17a2-11eb-84f3-c2308afb3a50.gif)](https://asciinema.org/a/TdbnSraK3oooVkzHOeeh59vvG)

## Disclaimer

This script is provided as is and it's intended for educational purpose. For production-ready deployment, you have to know Elasticsearch best practices, you may find some useful references below.

## For reading

- https://github.com/fabric8io/elasticsearch-cloud-kubernetes
- https://medium.com/@manis.eren/elasticsearch-on-kubernetes-master-connection-problem-because-of-jvm-dns-caching-27db53dea1e4
- https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes
- https://medium.com/faun/https-medium-com-thakur-vaibhav23-ha-es-k8s-7e655c1b7b61
- https://www.elastic.co/guide/en/elasticsearch/reference/7.9/modules-cluster.html#cluster-shard-allocation-settings
- https://www.elastic.co/blog/how-many-shards-should-i-have-in-my-elasticsearch-cluster
- https://www.elastic.co/guide/en/elasticsearch/reference/current/size-your-shards.html
- https://qbox.io/blog/optimizing-elasticsearch-how-many-shards-per-index
- https://blog.mapillary.com/tech/2017/01/12/scaling-down-an-elasticsearch-cluster.html
