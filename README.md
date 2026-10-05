# College Library Book Catalogue

A static college library catalogue for seat number **2329239**. The site is
published to GitHub Pages and can also be run locally with Docker or deployed
to Minikube.

## Live site

[Open the catalogue](https://4neha789.github.io/devops-c-2329239/)

## Catalogue contents

The HTML page in [`site/index.html`](site/index.html) lists:

- The Pragmatic Programmer
- Clean Code
- Introduction to Algorithms
- Designing Data-Intensive Applications

## GitHub Pages deployment

The workflow in
[`.github/workflows/pages.yml`](.github/workflows/pages.yml) publishes the
`site/` directory to GitHub Pages on each push to `main`. In the repository's
**Settings → Pages**, select **GitHub Actions** as the build and deployment
source.

## Run locally with Docker

From the repository root, build the nginx image and start the container:

```powershell
docker build -t devops-c-2329239 .
docker run --name devops-c-2329239-local -d -p 8082:80 devops-c-2329239
```

Open [http://localhost:8082/](http://localhost:8082/). To stop the container:

```powershell
docker stop devops-c-2329239-local
```

## Deploy to Minikube

With Docker Desktop and Minikube running, execute these commands from the
repository root. Building the image directly in Minikube makes it available to
the cluster without pushing it to a remote registry.

```powershell
minikube image build --tag devops-c-2329239:latest .
kubectl apply -f k8s\catalogue.yaml
kubectl rollout status deployment/devops-c-2329239
kubectl get pods,service -l app=devops-c-2329239
```

Forward the service to port `8081` on your computer:

```powershell
kubectl port-forward service/devops-c-2329239 8081:80
```

Keep that command running and open
[http://localhost:8081/](http://localhost:8081/). Press **Ctrl+C** in the
terminal to stop port forwarding. The deployment and service remain in the
cluster; remove them with:

```powershell
kubectl delete -f k8s\catalogue.yaml
```
