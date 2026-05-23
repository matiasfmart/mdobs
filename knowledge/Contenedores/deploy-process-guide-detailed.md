# Guía Detallada de Procesos de Deploy con Herramientas Específicas

> 📘 **Objetivo**: entender cada flujo de trabajo (manual, semi-automático, full-automático),  
> con detalle de **qué herramientas** se usan en cada tarea: construcción, registro, despliegue, monitoreo, etc.  

---

## 1. Flujo Manual (proyectos personales / aprendizaje)

### Descripción
El desarrollador hace **todo a mano** desde su PC.  
Se usa cuando:
- Se está **aprendiendo Docker**.  
- Se quiere probar un **MVP** sin infra compleja.  
- Solo hay **1 desarrollador y 1 tester**.  

### Herramientas por tarea
- **Construcción de imagen**:  
  - `docker build` desde consola (requiere Docker Desktop o Docker Engine).  
- **Registro de imágenes**:  
  - No se suele usar. O bien: `docker save` → exportar `.tar`.  
- **Ejecución de contenedor**:  
  - `docker run` con parámetros (`-p`, `--env-file`).  
- **Variables de entorno**:  
  - Archivos `.env` en texto plano.  
- **Logs**:  
  - `docker logs -f nombre_contenedor`.  
- **Entrega al tester**:  
  - Exportar imagen (`docker save`) o pasar repo completo + instrucciones.  

### Ejemplo paso a paso
```bash
# 1. Construir imagen
docker build -t miapp:1.0 .

# 2. Crear contenedor
docker run -d --name miapp --env-file .env -p 3000:3000 miapp:1.0

# 3. Ver logs
docker logs -f miapp
```

### Pros/Contras
✅ Máximo control, no dependés de nada externo.  
❌ Muy manual, propenso a errores, nada escalable.  

---

## 2. Flujo Semi-Automático (equipos pequeños / pruebas con testers)

### Descripción
Se busca simplificar la vida al tester:  
- El dev sigue construyendo la imagen.  
- La publica en un **registro de imágenes** (Docker Hub, GHCR).  
- El tester solo hace `docker pull` + `docker run`.  
- Se pueden entregar **scripts de inicio** para automatizar.  

### Herramientas por tarea
- **Construcción de imagen**:  
  - `docker build` local en la PC del dev.  
- **Registro de imágenes**:  
  - **Docker Hub** (más común, gratuito, público/privado).  
  - **GHCR (GitHub Container Registry)** (útil si ya usás GitHub).  
- **Distribución**:  
  - `docker push` (dev publica).  
  - `docker pull` (tester descarga).  
- **Ejecución de contenedor**:  
  - `docker run` manual o mediante **scripts `.bat`/`.ps1`/`bash`**.  
- **Variables de entorno**:  
  - `.env` entregado junto al script.  
- **Logs**:  
  - `docker logs` o `docker compose logs`.  
- **Entrega al tester**:  
  - Script + `.env` + instrucciones mínimas.  

### Ejemplo paso a paso
```bash
# Dev: construir y publicar
docker build -t usuario/miapp:latest .
docker push usuario/miapp:latest

# Tester: descargar y correr
docker pull usuario/miapp:latest
docker run -d --name miapp --env-file .env -p 3000:3000 usuario/miapp:latest
```

### Pros/Contras
✅ Fácil para el tester, distribuye bien las imágenes.  
✅ Más parecido a un flujo real.  
❌ El dev sigue haciendo build/push manualmente.  
❌ Sin control de versiones ni rollback automatizado.  

---

## 3. Flujo Full-Automático (empresas grandes / producción)

### Descripción
El flujo está completamente **automatizado con CI/CD**.  
Cada **push a GitHub** dispara un pipeline que:
1. Valida código (lint, tests, análisis de seguridad).  
2. Construye la imagen Docker.  
3. La publica en un registro.  
4. La despliega automáticamente en un cluster (staging/prod).  
5. Monitorea estado y logs.  

### Herramientas por tarea
- **Control de versiones y disparo de pipelines**:  
  - GitHub → **GitHub Actions**.  
  - GitLab → **GitLab CI/CD**.  
  - Corporaciones → **Jenkins**, **Azure DevOps**, **CircleCI**.  

- **Construcción de imágenes**:  
  - `docker buildx` dentro del pipeline.  
  - Multi-arch build para compatibilidad (x86, ARM).  

- **Registro de imágenes**:  
  - **Docker Hub** (proyectos open-source).  
  - **GHCR** (integración con GitHub repos).  
  - **ECR (AWS Elastic Container Registry)** en AWS.  
  - **ACR (Azure Container Registry)** en Azure.  
  - **Quay** o **Artifactory** en entornos enterprise.  

- **Despliegue**:  
  - Orquestador de contenedores:  
    - **Kubernetes (K8s)** (open-source estándar).  
    - **OpenShift** (versión enterprise con UI).  
  - Infraestructura:  
    - **AWS EKS** (Kubernetes en AWS).  
    - **Azure AKS** (Kubernetes en Azure).  
    - **GCP GKE** (Kubernetes en Google Cloud).  

- **Configuraciones y secretos**:  
  - ConfigMaps y Secrets (Kubernetes).  
  - Vault (Hashicorp) en corporaciones.  

- **Monitoreo y logs**:  
  - `kubectl logs` (básico).  
  - **Prometheus + Grafana** (métricas).  
  - **ELK stack (Elastic, Logstash, Kibana)**.  
  - **Datadog / New Relic** (empresas grandes).  

### Ejemplo pipeline (GitHub Actions)
```yaml
name: CI/CD Pipeline

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: docker/setup-buildx-action@v3
      - run: docker build -t usuario/miapp:${{ github.sha }} .
      - run: docker push usuario/miapp:${{ github.sha }}
      - run: docker push usuario/miapp:latest

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/miapp miapp=usuario/miapp:${{ github.sha }}
```

### Pros/Contras
✅ Cero intervención manual.  
✅ Rollback y versionado fácil.  
✅ Escalable a miles de contenedores.  
❌ Requiere infra compleja + roles DevOps.  
❌ Curva de aprendizaje alta.  

---

## 4. Comparativa de Herramientas

| Etapa               | Manual (Personal)          | Semi-Automático (Equipos pequeños)     | Full-Automático (Empresas)             |
|---------------------|-----------------------------|-----------------------------------------|-----------------------------------------|
| Construcción        | Docker CLI (`docker build`) | Docker CLI + scripts                    | CI/CD (Jenkins, GitHub Actions, GitLab) |
| Registro            | Ninguno / `.tar`           | Docker Hub, GHCR                        | ECR, ACR, GHCR, Quay                    |
| Distribución        | Copiar `.tar` / Git repo   | `docker push` / `docker pull`           | Automático vía pipeline                 |
| Ejecución           | `docker run` manual        | Scripts `.bat`, `.ps1`, `docker compose`| Kubernetes/OpenShift (pods, services)   |
| Configuración/env   | `.env` local               | `.env` + `env_file` en `docker run`     | ConfigMaps, Secrets, Vault              |
| Logs/Monitoreo      | `docker logs`              | `docker logs` + `docker compose logs`   | `kubectl logs`, Prometheus, Grafana     |
| Entorno             | PC personal                | Registro + scripts                      | Nube (AWS EKS, Azure AKS, GCP GKE)      |

---

# ✅ Conclusión

- **Manual** → aprender y probar localmente, usar Docker CLI.  
- **Semi-auto** → testers felices con Docker Hub + scripts simples.  
- **Full-auto** → pipelines (CI/CD), Kubernetes y monitoreo en la nube.  

👉 Para entrevistas **Senior**, esperan que:  
- Domines lo manual (Dockerfile, build, run).  
- Conozcas semi-auto (push/pull, registros, scripts).  
- Entiendas full-auto (CI/CD, Kubernetes, monitoreo, nubes).  

---

## Ver también

- [[CI CD]]
- [[Docker Contenedores y Despliegue]]
- [[Guia doker]]
