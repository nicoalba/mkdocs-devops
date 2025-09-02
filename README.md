# MkDocs sample docsite with DevOps (Docker/K8s)

My sample MkDocs site containerized with Docker and served via K8s.

This project demonstrates how to build, containerize, and deploy a static documentation site using [MkDocs](https://www.mkdocs.org/) with the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme. It includes Docker and Kubernetes configurations for development and deployment.

---

## Tech stack

- MkDocs  
- Material for MkDocs  
- Docker  
- Kubernetes (via Docker Desktop integration)  

---

## Features

- Static documentation site powered by Markdown  
- Dockerized/containerized for isolated and reproducible builds  
- Kubernetes deployment via manifest (`mkdocs-deployment.yaml`)  
- (Optional) LoadBalancer service configuration (`service.yaml`) 

---

## Project structure

```
mkdocs-devops/
├── docs/                    # Markdown content
├── mkdocs.yml               # MkDocs config
├── Dockerfile               # Docker build setup
├── mkdocs-deployment.yaml   # Kubernetes deployment
├── service.yaml             # (Optional) Kubernetes service
└── README.md
```

---

## Run with Docker

```bash
docker build -t mkdocs-devops .
docker run -p 8000:8000 mkdocs-devops
```

Go to [http://localhost:8000](http://localhost:8000).

---

## ☸Run with Kubernetes

### First-time deployment

1. Push your Docker image to Docker Hub:

    ```bash
    docker tag mkdocs-devops yourdockerhubusername/mkdocs-devops
    docker push yourdockerhubusername/mkdocs-devops
    ```

2. Update the image field in the `mkdocs-deployment.yaml` to use the image from Docker Hub.

3. After updating the `mkdocs-deployment.yaml`, apply the changes:

    ```bash
    kubectl apply -f mkdocs-deployment.yaml
    ```

4. Access the site.
    
    If using port-forwarding:

    ```bash
    kubectl port-forward deployment/mkdocs-deployment 8000:8000
    ```

    This will forward port 8000 on your local machine to port 8000 in your container, making the site accessible locally.

    Then open your web browser and go to [http://localhost:8000](http://localhost:8000).
---

### Stop/resume steps

| Situation                          | Command                                                   | When to use                                                                                                    |
| ---------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| First-time deploy                  | `kubectl apply -f mkdocs-deployment.yaml`                 | When you're initially creating the deployment.                                                                 |
| Stopping the deployment gracefully | `kubectl scale deployment mkdocs-deployment --replicas=0` | To stop the app without deleting the deployment.                                                               |
| Restarting after scaling to zero   | `kubectl scale deployment mkdocs-deployment --replicas=1` | When you've previously applied the YAML and just want to start it again. Port forwarding has to be done again. |

## Notes

- The `service.yaml` file is only required if you're exposing the deployment via LoadBalancer or NodePort.
- This repo is a dev-focused demo; production use should involve ingress and TLS.

