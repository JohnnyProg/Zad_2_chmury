
# Full-Stack Deployment in Kubernetes

This project demonstrates the deployment of a LAMP stack-like architecture in Kubernetes using Minikube. The stack includes a Java backend and an Apache-based frontend with a static page, along with a MySQL database for persistence. The deployment utilizes Kubernetes manifests to define resources, configurations, and access methods.

## 1. Project Overview

### Stack Type
- **Frontend**: Apache with a static page
- **Backend**: Java service
- **Database**: MySQL

### Assumptions
1. All Docker images are hosted in a private Docker repository.
2. Access to the frontend and backend services is achieved using `curl $(minikube ip)` due to issues with `minikube tunnel`.
3. The backend API is available at `/api/v1`.
4. Kubernetes manifests include configurations for Namespaces, Deployments, Services, ConfigMaps, Secrets, StatefulSets, and Ingress resources.

### Kubernetes Resources
The following Kubernetes manifests are used:
- **Namespace**: `pma_namespace.yaml`
- **ConfigMap**: `pma_env_cm.yaml`
- **Secrets**: `pma_secret_sc.yaml`
- **StatefulSet for MySQL**: `pma_db_ss.yaml`
- **Deployment for Backend**: `pma_backend_dep.yaml`
- **Deployment for Frontend**: `pma_frontend_dep.yaml`
- **Service for Database**: `pma_db_sv.yaml`
- **Service for Backend**: `pma_backend_sv.yaml`
- **Service for Frontend**: `pma_frontend_sv.yaml`
- **Ingress Resource**: `pma_ingress.yaml`

## 2. Deployment

### Instructions
To deploy the stack, use the following steps:

1. **Start Minikube**:
   ```bash
   minikube start
   ```

2. **Apply Kubernetes manifests**:
   Navigate to the directory containing the YAML files and run:
   ```bash
   kubectl apply -f .
   ```

3. **Verify Resources**:
   Confirm that all resources are running:
   ```bash
   kubectl get all -n pma-namespace
   ```

4. **Access Services**:
   - **Frontend**: Access the static page via:
     ```bash
     curl $(minikube ip)
     ```
   - **Backend API**: Access the backend service via:
     ```bash
     curl $(minikube ip)/api/v1
     ```

### Troubleshooting
- If any resource fails to start, check the logs:
  ```bash
  kubectl logs <pod-name> -n pma-namespace
  ```

## 3. Validation

### Confirm Deployment
To verify the stack:
1. Access the frontend:
   ```bash
   curl $(minikube ip)
   ```
   This should return the static content served by Apache.

2. Access the backend API:
   ```bash
   curl $(minikube ip)/api/v1
   ```
   This should return a JSON response from the backend.

3. Confirm Database Connection:
   Inspect the backend logs to verify successful database connections:
   ```bash
   kubectl logs <backend-pod-name> -n pma-namespace
   ```

## 4. YAML File Descriptions

### `pma_namespace.yaml`
Defines a custom namespace (`pma-namespace`) for isolating all stack resources.

### `pma_env_cm.yaml`
Configures environment variables for the stack, including database connection details.

### `pma_secret_sc.yaml`
Stores sensitive data like database credentials securely.

### `pma_db_ss.yaml`
Defines a MySQL StatefulSet for database persistence.

### `pma_backend_dep.yaml`
Deploys the Java backend application as a Deployment resource.

### `pma_frontend_dep.yaml`
Deploys the Apache-based frontend as a Deployment resource.

### `pma_db_sv.yaml`
Creates a ClusterIP Service for the MySQL database.

### `pma_backend_sv.yaml`
Creates a ClusterIP Service for the backend application.

### `pma_frontend_sv.yaml`
Creates a ClusterIP Service for the frontend application.

### `pma_ingress.yaml`
Defines an Ingress resource to route external traffic to the backend and frontend services (currently not utilized due to `minikube tunnel` issues).

## Notes
- Ensure that Minikube is properly configured and running before deploying.
- The project can be extended to use a fully functional ingress setup once `minikube tunnel` issues are resolved.
