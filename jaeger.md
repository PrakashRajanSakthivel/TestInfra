# Kubernetes Observability Stack: Setup & Troubleshooting Summary

## 1. Installation Options and Approaches

- **Istio**
  - Installed using `istioctl install --set profile=demo -y` for a lightweight, test-friendly setup.
- **Jaeger**
  - Attempted Istio’s built-in Jaeger addon (`samples/addons/jaeger.yaml`).
  - Switched to the official Jaeger all-in-one deployment for compatibility and simplicity, using a modern manifest.
- **Prometheus & Grafana**
  - Installed via Istio’s sample addon manifests (`prometheus.yaml`, `grafana.yaml`).
- **Elasticsearch & Kibana**
  - Already installed and tested via port-forwarding.

## 2. Accessing UIs and Services

- **Port-forwarding**
  - Used `kubectl port-forward` to access Jaeger, Prometheus, Grafana, Kibana, and Elasticsearch UIs locally.
- **NodePort**
  - Used NodePort mappings (e.g., for Istio ingress gateway) to access services via `localhost:<NodePort>`.
- **LoadBalancer**
  - Noted that EXTERNAL-IP remains `<pending>` in local clusters, so NodePort or port-forwarding is required.

## 3. Troubleshooting and Fixes

- **DNS/Network Issues**
  - When `curl` failed to download Istio, used manual downloads and package managers as alternatives.
- **Jaeger Not Fully Deployed**
  - Noted missing `jaeger-query` pod/service; re-applied manifests and switched to the all-in-one deployment.
- **Kubernetes API Version Errors**
  - Encountered deprecated `extensions/v1beta1` in old manifests; replaced with updated `apps/v1` manifests.
- **Service Patch Errors**
  - Resolved clusterIP mutation errors by deleting conflicting services and applying clean, modern manifests.
- **Ingress Gateway Access**
  - Addressed `<pending>` EXTERNAL-IP by using NodePort and port-forwarding for local access.

## 4. Verification

- **Checked pod and service status** with `kubectl get pods` and `kubectl get svc` in relevant namespaces.
- **Tested each UI** (Jaeger, Prometheus, Grafana, Kibana, Elasticsearch) via browser after port-forwarding.
- **Validated Istio Ingress** by accessing via NodePort or port-forward.

## 5. Key Learnings and Best Practices

- Always use up-to-date manifests compatible with your Kubernetes version.
- For local clusters, prefer NodePort or port-forwarding over LoadBalancer.
- If a component is missing, check pod/service status and logs, and re-apply manifests as needed.
- For quick tracing setup, Jaeger all-in-one is ideal for dev/test; for production, use the Jaeger Operator.

---

**You now have a robust, observable test environment with Istio service mesh, distributed tracing, metrics, and logging—all running locally!**
