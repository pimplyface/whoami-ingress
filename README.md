# Whoami Ingress Demo

This project deploys the [traefik/whoami](https://hub.docker.com/r/traefik/whoami) application in a Kubernetes cluster, exposed via an Ingress resource. The `whoami` app is a simple web server that returns details about the HTTP request, useful for testing ingress configurations.

## Features
- Kubernetes Deployment with resource limits, health probes, and security context (runs as non-root user).
- Service exposing ports 80 (HTTP) and 443 (HTTPS) with Prometheus annotations.
- Ingress routing traffic to `/foo` and `/bar` paths.
- ServiceMonitor for Prometheus monitoring (requires Prometheus Operator).
- Compatible with both Traefik and NGINX Ingress Controllers.

## Prerequisites
- A Kubernetes cluster (e.g., Minikube, EKS, GKE).
- `kubectl` configured to access the cluster.
- An Ingress Controller installed (e.g., Traefik or NGINX Ingress).
  - For NGINX: Install via Helm: `helm install nginx-ingress ingress-nginx/ingress-nginx`.
  - For Traefik: Ensure it's running in your cluster.

## Deployment
1. Create the namespace:
   ```bash
   kubectl create namespace whoami
   ```

2. Apply the manifests:
   ```bash
   kubectl apply -f whoami.deployment.yml -f whoami.service.yml -f whoami.ingress.yml -f whoami.servicemonitor.yml
   ```

3. Verify the deployment:
   ```bash
   kubectl get pods -n whoami
   kubectl get svc -n whoami
   kubectl get ingress -n whoami
   ```

## Usage
- Access the application via your ingress host (e.g., `http://your-ingress-host/foo` or `http://your-ingress-host/bar`).
- The `whoami` app will display request details, including headers, IP, etc.

## Configuration
- **Ingress Controller**: The ingress is generic and works with both Traefik and NGINX. If using multiple controllers, add `ingressClassName: nginx` (or `traefik`) to the ingress spec.
- **TLS**: To enable HTTPS, add a `tls` section to `whoami.ingress.yml` with a secret containing your certificates.
- **Paths**: Currently routes `/foo` and `/bar` to the service. Modify `whoami.ingress.yml` for custom paths.

## Cleanup
```bash
kubectl delete -f whoami.deployment.yml -f whoami.service.yml -f whoami.ingress.yml
kubectl delete namespace whoami
```

## Notes
- The deployment uses the `latest` tag for the image; pin to a specific version for production.
- Health probes ensure the pod is ready and alive.
- Security context runs the container as a non-root user for better security.
- Monitoring is set up via ServiceMonitor for Prometheus (if using Prometheus Operator). Note: The `whoami` app does not expose a `/metrics` endpoint by default, so metrics scraping may not work without additional configuration.
- For production, consider adding network policies and further hardening.</content>
<parameter name="filePath">/Volumes/M2.SSD/Github/Projects/whoami-ingress/README.md
