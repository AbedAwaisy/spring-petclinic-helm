
# 🐾 Spring Petclinic on Kubernetes with PostgreSQL (Helm-based)

This project demonstrates how to deploy the Spring Petclinic application on a local Kubernetes cluster using **Minikube**, with a **PostgreSQL database**, fully managed via a **Helm chart**.

---

## 🧱 Architecture Overview

- **Spring Boot App** deployed as a Kubernetes `Deployment`
- **PostgreSQL DB** deployed as a `StatefulSet`
- Configuration via **Kubernetes Secret** using the [Service Binding Specification](https://servicebinding.io/)
- Services exposed via:
  - **NodePort** (port `31111`)
  - **Ingress** at `http://petclinic.local`

---

## 🚀 Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Helm 3](https://helm.sh/docs/intro/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- Optional: [nginx Ingress addon enabled](https://minikube.sigs.k8s.io/docs/handbook/addons/ingress/)

---

## 🛠️ Setup Instructions

### 1. Clone this repository
```bash
git clone https://github.com/AbedAwaisy/spring-petclinic-helm.git
cd spring-petclinic-helm
```

### 2. Start Minikube
```bash
minikube start
```

### 3. Enable Ingress
```bash
minikube addons enable ingress
```

### 4. Create the namespace
```bash
kubectl create namespace petclinic
```

### 5. Install the Helm chart
```bash
helm install petclinic ./petclinic-chart -n petclinic
```

---

## 🌐 Accessing the Application

### Option A: Via NodePort
Get Minikube's IP:
```bash
minikube ip
```

Then open in your browser:
```
http://<minikube-ip>:31111
```

---

### Option B: Via Ingress (Recommended)

1. Add the following to your `/etc/hosts`:
   ```bash
   <minikube-ip> petclinic.local
   ```

   Example:
   ```
   192.168.49.2 petclinic.local
   ```

2. Start the tunnel (if needed):
   ```bash
   minikube tunnel
   ```

3. Open in your browser:
   ```
   http://petclinic.local
   ```

---

## 🐘 Verifying Database Integration

1. Add an owner using the Petclinic UI.
2. Connect to the DB pod:
   ```bash
   kubectl exec -it demo-db-0 -n petclinic -- bash
   psql -U user -d petclinic
   ```

3. Run:
   ```sql
   SELECT * FROM owners;
   ```

You should see the data you added in the app.

---

## 📦 Helm Chart Structure

```
petclinic-chart/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── db-secret.yaml
    ├── db-service.yaml
    ├── db-statefulset.yaml
    └── petclinic-deployment.yaml
```

---

## 📋 Configuration

All key settings are in `values.yaml`:
```yaml
app:
  name: petclinic
  image: dsyer/petclinic
  nodePort: 31111

db:
  name: demo-db
  image: postgres:17
  user: user
  password: pass
  database: petclinic
  port: 5432

ingress:
  enabled: true
  host: petclinic.local
```

---

## ✅ What This Project Demonstrates

- Helm-based deployment
- Stateful app with persistent database
- Secure config via Secrets
- Spring Boot + PostgreSQL integration
- Ingress-based domain routing
- Production-ready Kubernetes practices

---

## 🧹 Cleanup

```bash
helm uninstall petclinic -n petclinic
kubectl delete namespace petclinic
```

---

## 🧑‍💻 Author

Abed Awaisy  
DevOps Engineer  
[GitHub](https://github.com/AbedAwaisy) • [LinkedIn](https://www.linkedin.com/in/abedawaisy)
