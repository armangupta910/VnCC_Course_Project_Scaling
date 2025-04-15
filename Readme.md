<h1>Kubernetes Deployment for Note-Taking App</h1>

<p>This guide explains how to set up and deploy the Note-Taking microservices with Minikube, along with Prometheus monitoring, MySQL database, and KEDA autoscaling.</p>

<hr>

<h2>🔧 Step 1: Install & Start Minikube</h2>

<h3>✅ Install Minikube</h3>
<p>Follow official instructions for your OS here: <a href="https://minikube.sigs.k8s.io/docs/start/">https://minikube.sigs.k8s.io/docs/start/</a></p>

<h3>✅ Start Minikube</h3>
<pre><code>minikube start</code></pre>

<p>(Optional) Enable the metrics server (needed for HPA):</p>
<pre><code>minikube addons enable metrics-server</code></pre>

<hr>

<h2>🚀 Step 2: Deploy Microservices</h2>
<p>Navigate to the <code>deployments/</code> folder and apply the deployment YAMLs:</p>
<pre><code>kubectl apply -f deployments/add-note.yaml
kubectl apply -f deployments/update-note.yaml
kubectl apply -f deployments/delete-note.yaml
kubectl apply -f deployments/get-note.yaml</code></pre>

<hr>

<h2>🌐 Step 3: Deploy Services for Microservices</h2>
<p>Apply the service YAMLs inside the <code>services/</code> folder:</p>
<pre><code>kubectl apply -f services/add-note-service.yaml
kubectl apply -f services/update-note-service.yaml
kubectl apply -f services/delete-note-service.yaml
kubectl apply -f services/get-note-service.yaml</code></pre>

<hr>

<h2>📈 Step 4: Set up Prometheus Monitoring</h2>
<p>Navigate to the <code>prometheus/</code> folder and apply the configurations in this order:</p>
<pre><code>kubectl apply -f prometheus/config-map.yaml
kubectl apply -f prometheus/rbac.yaml
kubectl apply -f prometheus/deployment.yaml
kubectl apply -f prometheus/service.yaml</code></pre>

<p>To access the Prometheus UI in the browser:</p>
<pre><code>minikube service prometheus</code></pre>

<p>This will open Prometheus in your default web browser.</p>

<hr>

<h2>🛢️ Step 5: Deploy MySQL Pod and Service</h2>
<pre><code>kubectl apply -f mysql-exporter.yaml
kubectl apply -f mysql-exporter-service.yaml</code></pre>

<hr>

<h2>⚖️ Step 6: Deploy HPA with KEDA</h2>
<p>Make sure KEDA is installed in your cluster. If not, follow: <a href="https://keda.sh/docs/latest/install/">https://keda.sh/docs/latest/install/</a></p>

<p>Then apply the following KEDA ScaledObjects:</p>
<pre><code>kubectl apply -f hpaAddNote.yaml
kubectl apply -f hpaDeleteNote.yaml
kubectl apply -f hpaUpdateNote.yaml
kubectl apply -f hpaGetNote.yaml</code></pre>

<p>These will autoscale your microservices based on custom metrics or queue length, depending on your configuration.</p>

<hr>

<h2>✅ Summary</h2>
<ul>
  <li>Microservices: <code>add</code>, <code>delete</code>, <code>update</code>, <code>get</code></li>
  <li>Observability: <code>Prometheus</code></li>
  <li>Database: <code>MySQL</code></li>
  <li>Autoscaling: <code>KEDA</code></li>
</ul>

<hr>

<h2>📎 Notes</h2>
<p>All YAML files are organized under respective folders: <code>deployments/</code>, <code>services/</code>, <code>prometheus/</code>, <code>KEDA/</code></p>
<p>You can verify your deployments using:</p>
<pre><code>kubectl get all</code></pre>
