# substreams-charts

Helm repository for substreams charts.

## For `minikube` testing

### Prerequisites

Make sure you have all these applications available.

- `minikube`
- `helm`
- `kubectl`
- `docker`
- [Optional] `substreams` CLI

### Start `minikube`

Make sure `docker` is running.

```sh
minikube start
```

### Deploy `producer` chart.

```sh
helm install producer ./charts/producer
```

Make sure to wait until the `producer` pod is ready.

```sh
kubectl get pod
```
```sh
NAME         READY   STATUS    RESTARTS   AGE
producer-0   1/1     Running   0          57s
```

### Deploy `firehose-antelope` chart.

```sh
helm install firehose-antelope ./charts/firehose-antelope -f ./local-test-values.yaml
```
If the deployment has no error, you should see this

```sh
kubectl get pod
```
```sh
NAME                       READY   STATUS    RESTARTS   AGE
firehose-0                 0/1     Running   0          2m27s
merger-0                   1/1     Running   0          2m27s
producer-0                 1/1     Running   0          3m19s
reader-node-0              1/1     Running   0          2m27s
relayer-7fc86d45cc-dz8vz   1/1     Running   0          2m27s
substreams-tier1-0         1/1     Running   0          2m27s
substreams-tier2-0         1/1     Running   0          2m27s
```

### Test with `substreams` CLI

Make sure to have [Substreams Modules Repo](https://github.com/ultraio/substreams-modules) built.

Port forwarding

```sh
kubectl port-forward service/substreams-tier1 9000:9000
```

Run `substreams` CLI

```sh
substreams run -e localhost:9000 ULTRA_PATH/substreams-modules/eosio.token/substreams.yaml map_transfers --start-block 2 --stop-block 100 --plaintext
```

### To stop `minikube`

```sh
minikube stop
```

### [Recommended] To delete persistent data before starting a new test

```sh
minikube delete
```
