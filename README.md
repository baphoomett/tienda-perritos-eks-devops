# Tienda Perritos

Resumen
-------
Aplicación simple compuesta por tres servicios: `backend`, `frontend` y `db` (MySQL). Contiene Dockerfiles por servicio y manifiestos de Kubernetes en la carpeta `k8s/` para despliegue en un clúster.

Estructura del proyecto
-----------------------
- `backend/` - Código del servidor Node.js (Dockerfile, `server.js`, `package.json`).
- `frontend/` - Aplicación estática / Node (Dockerfile, `index.html`, `app.js`).
- `db/` - Imagen de MySQL inicial con `init.sql`.
- `k8s/` - Manifiestos Kubernetes (Deployment, Service, HPA, Secret, Namespace).
- `.github/workflows/` - Pipelines CI/CD para frontend y backend.

Requisitos previos
------------------
- Docker (Engine)
- kubectl (si vas a desplegar en Kubernetes)
- Acceso a un clúster Kubernetes (minikube, kind, EKS, etc.)
- Git

Construir y ejecutar localmente con Docker
-----------------------------------------
Construir imágenes (desde la raíz del repo):

```bash
docker build -t tienda-backend:local ./backend
docker build -t tienda-frontend:local ./frontend
docker build -t tienda-mysql:local ./db
```

Ejecutar contenedores individualmente (ejemplo backend):

```bash
docker run --rm -p 3000:3000 --name tienda-backend tienda-backend:local
```

Despliegue en Kubernetes
------------------------
1. Crear el namespace y secretos:

```bash
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/mysql-secret.yaml
```
2. Desplegar MySQL, backend y frontend (aplica según orden recomendado):

```bash
kubectl apply -f k8s/mysql-deployment.yaml
kubectl apply -f k8s/mysql-service.yaml
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/backend-service.yaml
kubectl apply -f k8s/frontend-deployment.yaml
kubectl apply -f k8s/frontend-service.yaml
```

3. (Opcional) HPA:

```bash
kubectl apply -f k8s/backend-hpa.yaml
kubectl apply -f k8s/frontend-hpa.yaml
```

CI/CD
-----
Este repositorio incluye flujos de trabajo en `.github/workflows/` para construir y publicar (o ejecutar tests) del frontend y backend. Revisa los ficheros:

- `.github/workflows/cicd-tienda-backend.yml`
- `.github/workflows/cicd-tienda-frontend.yml`


Buenas prácticas de commits
--------------------------
- Usa mensajes descriptivos y en tiempo imperativo (ej: "Añadir endpoint /pets para listar mascotas").
- Sigue la convención de tipo: `feat:`, `fix:`, `chore:`, `docs:`, `test:`.
- Mantén la línea de asunto corta (<= 72 caracteres) y si hace falta añade cuerpo con más contexto.


