## 🧩 API Deployment

The `api` service is a core component of this project. It's defined by two Kubernetes manifests:

### Deployment: `api`

- **Container Image**: `three-tier-api`
- **Port**: `5001`
- **Env Variables**:
  - `MYSQL_HOST`: mysql
  - `MYSQL_USER`: appuser
  - `MYSQL_PASSWORD`: app_password
  - `MYSQL_DB`: quotesdb

Apply it using:

```bash
kubectl apply -f path/to/api-deployment.yaml

## 🌐 **Frontend Deployment**

The `frontend` component serves the web UI of the application.

### Deployment: `frontend`

- **Container Image**: `three-tier-frontend`
- **Port**: `5002`
- **Depends on**: `api` service (`http://api:5001/api/quotes`)

Environment variables:
- `API_URL=http://api:5001/api/quotes`

Apply using:

```bash
kubectl apply -f path/to/frontend-deployment.yaml

## 🛢️** Database (MySQL)**

This app uses MySQL as a backend database, deployed via a Kubernetes Deployment and PVC.

### Deployment: `mysql`

- **Image**: `mysql/mysql-server:5.7`
- **Port**: 3306
- **Persistent Volume**: 1Gi (bound to `/var/lib/mysql`)

Environment variables:
- `MYSQL_ROOT_PASSWORD`: root
- `MYSQL_DATABASE`: quotesdb

### PersistentVolumeClaim

Reserves storage for the MySQL pod:
```yaml
storage: 1Gi
accessModes: ReadWriteOnce


## 🌐 Accessing the Frontend

The frontend is exposed via a `LoadBalancer` service:

```bash
kubectl apply -f frontend-service.yaml
kubectl get svc frontend


