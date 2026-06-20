# Spark Connect on Kubernetes

This setup deploys:

* Spark Connect server
* Jupyter Notebook
* Spark executors launched dynamically in Kubernetes

## Deploy

Apply all manifests:

```bash
kubectl apply -f namespace.yml -f spark  -f jupyter.yml
```

Verify the pods:

```bash
kubectl get pods -n spark
```

Expected pods:

* `spark-connect`
* `jupyter`

---

## Access Jupyter Notebook

Port-forward the Jupyter service:

```bash
kubectl port-forward -n spark svc/jupyter-svc 8888:8888
```

Open your browser:

```text
http://localhost:8888
```

Token:

```text
letmein
```

---

## Run Spark Jobs

The included notebook (`test.ipynb`) contains example Spark Connect code.

Open the notebook in Jupyter and execute the cells.

The notebook connects to:

```python
sc://spark-connect.spark.svc.cluster.local:15002
```

Spark jobs will be submitted through the Spark Connect server, which will automatically create executor pods inside the `spark` namespace.

---

## Monitor Spark

List Spark driver and executor pods:

```bash
kubectl get pods -n spark
```

Watch pods:

```bash
kubectl get pods -n spark -w
```

Spark Connect logs:

```bash
kubectl logs -n spark deploy/spark-connect -f
```

---

## Cleanup

Remove all resources:

```bash
kubectl delete -f spark  -f jupyter.yml -f namespace.yml

```
