# 🚀 Tekton CI Setup (Local KIND Cluster)

This guide helps you set up and run a Tekton-based CI pipeline locally using Kubernetes (KIND), with optional GitHub webhook triggering.

---

## 📦 Prerequisites

* Kubernetes cluster (KIND or any local cluster)
* Tekton Pipelines installed
* Tekton Triggers installed
* kubectl configured
* ngrok (for webhook exposure)
* GitHub repository

---

## 🏗️ Namespace Setup

Create namespace (if not already created):

```bash
kubectl create namespace java-pipeline
```

---

## ⚙️ Step 1: Apply Core CI Resources

Apply tasks:

```bash
kubectl apply -f ./tasks -n java-pipeline
```

Apply pipeline:

```bash
kubectl apply -f ./pipeline.yaml -n java-pipeline
```

---

## ▶️ Step 2: Run Pipeline Manually (Optional)

If you are NOT using triggers:

```bash
kubectl apply -f ./pipelinerun.yaml -n java-pipeline
```

Check status:

```bash
kubectl get pipelineruns -n java-pipeline
```

---

## 🔐 Step 3: Setup RBAC for Triggers

Create ServiceAccount:

```bash
kubectl apply -f triggers-sa.yaml -n java-pipeline
```

Create ClusterRoleBinding:

```bash
kubectl apply -f triggers-clusterRoleBinding.yaml
```

---

## 🔁 Step 4: Apply Trigger Resources

```bash
kubectl apply -f triggerbinding.yaml -n java-pipeline
kubectl apply -f triggertemplate.yaml -n java-pipeline
kubectl apply -f eventlistener.yaml -n java-pipeline
```

---

## 🔍 Step 5: Verify EventListener

```bash
kubectl get pods -n java-pipeline
```

Expected:

```
el-java-ci-listener-xxxxx
```

If not running:

```bash
kubectl describe pod <pod-name> -n java-pipeline
```

---

## 🌐 Step 6: Expose EventListener (KIND)

```bash
kubectl port-forward svc/el-java-ci-listener 8080:8080 -n java-pipeline
```

---

## 🌍 Step 7: Expose via ngrok

```bash
ngrok http 8080
```

Example output:

```
https://abc123.ngrok.io
```

---

## 🔗 Step 8: Configure GitHub Webhook

Go to your GitHub repository:

**Settings → Webhooks → Add webhook**

### Configure:

* **Payload URL:**

  ```
  https://abc123.ngrok.io
  ```

* **Content type:**

  ```
  application/json
  ```

* **Events:**

  ```
  Just the push event
  ```

---

## 🧪 Step 9: Test the Trigger

Push code to `main` branch:

```bash
git add .
git commit -m "test trigger"
git push origin main
```

---

## ✅ Step 10: Verify Pipeline Execution

```bash
kubectl get pipelineruns -n java-pipeline
```

Expected:

```
java-ci-run-xxxxx
```

---

## 🐞 Debugging

### Check EventListener logs

```bash
kubectl logs -l eventlistener=java-ci-listener -n java-pipeline
```

---

### Watch PipelineRuns live

```bash
kubectl get pipelineruns -n java-pipeline -w
```

---

### Check Task logs

```bash
kubectl logs <pod-name> -n java-pipeline
```

---

## ⚠️ Common Issues

### ❌ Pipeline not triggering

* ngrok not running
* wrong webhook URL

### ❌ Wrong branch issue

Handled inside task using branch normalization

### ❌ Permission issues

* Ensure ClusterRoleBinding is applied correctly

---

Happy Building! 🔥
