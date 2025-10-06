<<<<<<< HEAD
# nextjs-devops-assessment
=======
# Next.js DevOps Assessment - Repository Skeleton

This repository contains a minimal Next.js application and all supporting configuration for the assessment:
containerization (Docker), CI/CD (GitHub Actions -> GHCR), and Kubernetes manifests for Minikube deployment.

## What is included
- Minimal Next.js app (pages/index.js, pages/api/health.js)
- Dockerfile (multi-stage, optimized)
- .dockerignore
- GitHub Actions workflow `.github/workflows/ci.yml` (build + push to GHCR)
- Kubernetes manifests in `k8s/` (deployment + service)
- This README with step-by-step instructions

## Prerequisites
- Node.js >= 18
- npm
- Docker
- kubectl
- Minikube (for local Kubernetes)
- Git and a GitHub account (to push the public repo)
- (Optional) A GitHub Personal Access Token (if you want to manually push to GHCR)

## Local development
```bash
# install deps
npm install

# run development server
npm run dev
# open http://localhost:3000
```

## Run in Docker locally
```bash
docker build -t nextjs-devops-assessment:local .
docker run -p 3000:3000 nextjs-devops-assessment:local
# open http://localhost:3000
```

## Publish image to GitHub Container Registry (GHCR)
1. Build and tag (replace <USERNAME>):
   ```bash
   docker build -t ghcr.io/<USERNAME>/nextjs-devops-assessment:latest .
   ```
2. Login to GHCR
   ```bash
   echo $GHCR_PAT | docker login ghcr.io -u <USERNAME> --password-stdin
   # GHCR_PAT requires `write:packages` scope
   ```
3. Push
   ```bash
   docker push ghcr.io/<USERNAME>/nextjs-devops-assessment:latest
   ```
4. Alternatively, rely on GitHub Actions (workflow included) which pushes the image automatically on `main` branch commits.

## Deploy to Minikube (two options)

### Option A — Deploy using GHCR image (recommended)
1. Make sure the image is public (or create imagePullSecret if private).
2. Update `k8s/deployment.yaml` image field with your GHCR path.
3. Apply manifests:
   ```bash
   kubectl apply -f k8s/deployment.yaml
   kubectl apply -f k8s/service.yaml
   ```
4. Get URL:
   ```bash
   minikube service nextjs-service --url
   ```

### Option B — Build image inside Minikube's Docker daemon (no GHCR required)
```bash
eval $(minikube docker-env)
docker build -t nextjs-devops-assessment:local .
# Ensure k8s/deployment.yaml uses image: nextjs-devops-assessment:local
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
minikube service nextjs-service --url
```

## Health checks
- Liveness: `GET /api/health`
- Readiness: `GET /`

## GitHub Actions notes
- The workflow `ci.yml` runs on pushes to `main` and uses the repository's `GITHUB_TOKEN` to authenticate to GHCR.
- Ensure repository permissions allow `packages: write` for the workflow (the workflow sets this permission).

## Submission checklist
- [ ] Public GitHub repository created
- [ ] All code, Dockerfile, workflows, Kubernetes manifests pushed
- [ ] GHCR image pushed (or confirm Actions has pushed it)
- [ ] README updated with final repository/image URLs

## Helpful commands summary
```bash
# build locally
docker build -t nextjs-devops-assessment:local .

# run locally
docker run -p 3000:3000 nextjs-devops-assessment:local

# apply k8s manifests
kubectl apply -f k8s/

# get service url (minikube)
minikube service nextjs-service --url
```
>>>>>>> 1c61274 (Initial commit)
