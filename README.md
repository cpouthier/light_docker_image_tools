# Building the `blueprint` Docker Image

This guide explains how to build and push the lightweight Docker image [`yourdockerrepository/blueprint`](https://hub.docker.com/r/yourdockerrepository/blueprint), which is designed for use in Kanister blueprints to manipulate Kubernetes resources using `kubectl`, `jq`, and `yq`.

---

## 🧰 What's Included in the Image

- `kubectl` (v1.27.0)
- `jq` (lightweight JSON processor)
- `yq` (YAML processor written in Go)
- `bash` and common Alpine utilities

---

## 🏗️ Dockerfile

Create a file named `Dockerfile` with the following contents:

```dockerfile
FROM alpine:3.19

# Install required tools
RUN apk add --no-cache curl bash jq yq

# Install kubectl
RUN curl -L https://dl.k8s.io/release/v1.27.0/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl && \
    chmod +x /usr/local/bin/kubectl

# Add kubectl to PATH
ENV PATH="/usr/local/bin:$PATH"

# Default command
CMD ["/bin/sh"]
```

---

## 🛠️ Build the Image

Run the following command from the directory containing the `Dockerfile`:

```bash
docker build -t yourdockerrepository/blueprint:latest .
```

---

## 🚀 Push to Docker Hub

First, log in to Docker Hub:

```bash
docker login
```

Then push your image:

```bash
docker push yourdockerrepository/blueprint:latest
```

---

## ✅ Test the Image Locally

You can run and test the image interactively:

```bash
docker run -it --rm yourdockerrepository/blueprint:latest
```

Or run a direct command:

```bash
docker run -it --rm yourdockerrepository/blueprint:latest kubectl version --client
```

---

## 🧪 Usage in Kanister

Use the image in your Kanister blueprint like this:

```yaml
image: yourdockerrepository/blueprint:latest
```

This ensures access to all required tools (`kubectl`, `jq`, `yq`) in your `KubeTask` phases.


