# Guía Completa: Docker, Kubernetes, Jenkins y CI/CD

---

## 1. Contenedor vs Imagen

### Imagen Docker
- Es un **paquete inmutable** que contiene:
  - Un mini sistema operativo base (Linux slim, Alpine, etc.).
  - Dependencias.
  - Tu código copiado.
  - Configuración de arranque (`CMD`).
- Se crea con un **Dockerfile** + comando `docker build`.
- Es como la **clase en POO** o la **receta congelada**.

### Contenedor Docker
- Es una **instancia en ejecución de una imagen**.
- Se crea con `docker run` o con `docker compose up`.
- Es como el **objeto en POO** o la **porción de torta servida**.

### VM vs Contenedor
- **VM** = computadora completa emulada (tiene SO propio, pesado).
- **Contenedor** = comparte el kernel del host, es liviano, arranca en segundos.

---

## 2. Dockerfile

- Nombre estándar: `Dockerfile` (sin extensión).
- Puede llamarse distinto (ej. `Dockerfile.dev`, `DockerMati`), pero tenés que indicarlo con `-f`:
  ```bash
  docker build -t miapp:1.0 -f DockerMati .
  ```
- El **Dockerfile no contiene tu código**, solo instrucciones.
- Tu código se **copia** dentro de la imagen con instrucciones (`COPY . .`).
- Cada instrucción (`FROM`, `RUN`, `COPY`) crea una **capa** en la imagen.

### Ejemplo básico
```dockerfile
FROM node:20
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
CMD ["npm", "start"]
```

---

## 3. Docker build vs run

- `docker build` → crea la **imagen** a partir del `Dockerfile`.
- `docker run` → crea y arranca un **contenedor** a partir de esa imagen.
- `docker images` → lista las imágenes en tu máquina.
- `docker ps` → lista los contenedores en ejecución.

---

## 4. docker-compose.yml

- Es un **archivo YAML declarativo** para levantar múltiples contenedores de una.
- Nombre estándar: `docker-compose.yml` (puede llamarse distinto, pero hay que pasarlo con `-f`).
- Lo ejecutás con:
  ```bash
  docker compose up -d
  ```
  → crea y arranca todos los servicios definidos.
- Equivale a guardar varios `docker run` en un solo archivo.

### Ejemplo
```yaml
services:
  mongo:
    image: mongo:7.0
    ports:
      - "27017:27017"
    volumes:
      - ./data/mongo:/data/db

  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - MONGODB_URI=mongodb://mongo:27017
```

---

## 5. Volúmenes, Puertos y Networking

### Volúmenes
- **Volumen** = carpeta de tu PC montada en el contenedor → persiste datos.
```yaml
volumes:
  - ./data/mongo:/data/db
```

### Puertos
- **Puertos** = puente entre el puerto interno del contenedor y tu host.
```yaml
ports:
  - "3000:3000"
```

### Networking interno en Compose
- Los contenedores se hablan por **nombre de servicio** (`mongo`, `app`).
- `localhost` dentro de un contenedor apunta solo a él mismo, no al host.

---

## 6. Dónde se guardan las imágenes

- Docker las guarda en su **almacenamiento interno** (ej: `/var/lib/docker` en Linux).
- No las ves como archivos normales.
- Para verlas: `docker images`.
- Para exportar como archivo:
  ```bash
  docker save -o miapp.tar miapp:1.0
  ```
- Para importarlas en otra máquina:
  ```bash
  docker load -i miapp.tar
  ```

---

## 7. Registros Docker

- Como GitHub para código, pero para imágenes.
- Ahí se guardan las imágenes que luego Kubernetes/OpenShift baja para correr.

### Ejemplos de registros
- **Docker Hub** (hub.docker.com) → el más popular, gratis para imágenes públicas, estándar en la industria.
- **GitHub Container Registry (GHCR)** (ghcr.io) → integrado en GitHub, útil si ya trabajás con GitHub Actions y repos privados.
- **Otros**: GitLab Container Registry, AWS ECR, Azure ACR, Google GAR, Red Hat Quay, OpenShift Internal Registry.

### Diferencias
- **Docker Hub**: universal, ideal para proyectos open source o compartir fácil.
- **GHCR**: ideal si tu repo ya vive en GitHub, buena integración con Actions y permisos.

---

## 8. Kubernetes

- No compite con Docker → lo complementa.
- Es un **orquestador de contenedores** a gran escala.

### Maneja:
- **Pods**: unidad mínima (generalmente 1 contenedor).
- **Deployments**: definen cuántos pods querés, cómo se actualizan.
- **Services**: exponen pods y hacen balanceo de carga.
- **ConfigMaps/Secrets**: configs y credenciales.
- **Liveness/Readiness probes**: health checks.

### Funcionalidades clave
- Escala, balancea, recupera fallos, hace rolling updates.
- OpenShift = Kubernetes + interfaz gráfica + extras empresariales.

---

## 9. Jenkins

- Es una **aplicación servidor** (con UI web).
- Se usa para **automatizar pipelines** de CI/CD.
- Un **pipeline** = serie de pasos: compilar, testear, analizar, generar imagen Docker, subir a registro, desplegar en Kubernetes/OpenShift.

### Dos formas de configurarlo
1. Por UI (jobs freestyle).
2. Como código en un **`Jenkinsfile`** (Groovy).

### Ejemplo de pipeline en Jenkinsfile
```groovy
pipeline {
  agent any
  stages {
    stage('Build') { steps { sh 'npm install && npm run build' } }
    stage('Docker Build') { steps { sh 'docker build -t miapp:1.0 .' } }
    stage('Push Image') { steps { sh 'docker push registry/miapp:1.0' } }
    stage('Deploy') { steps { sh 'kubectl apply -f k8s/deployment.yml' } }
  }
}
```

---

## 10. CI/CD

- **CI (Continuous Integration):** integrar cambios continuamente → build + tests + análisis automático en cada commit.
- **CD (Continuous Delivery/Deployment):** entregar/desplegar automáticamente si CI pasó.
- Herramientas: Jenkins, GitHub Actions, GitLab CI, CircleCI.
- En Vercel → CI/CD ya viene integrado: cuando hacés push a GitHub, hace build y deploy automático.

---

## 11. CDN y Serverless

### CDN (Content Delivery Network)
- Red global de servidores que replican archivos estáticos (imágenes, CSS, JS).
- Cuando un usuario pide un archivo, el CDN lo entrega desde el nodo más cercano.
- Mejora la velocidad y reduce latencia.

### Archivos estáticos
- Cosas que no cambian por usuario (logo, bundle JS, CSS).

### Serverless
- Funciones en la nube que solo se ejecutan cuando alguien las llama.
- No hay un servidor “siempre prendido”.
- Ejemplo: API Routes en Next.js en Vercel.

---

## 12. ¿Por qué un dev debería saber Kubernetes/Jenkins?

- En teoría: DevOps se encarga de eso.
- En la práctica: muchas empresas esperan que un **senior** pueda:
  - Desplegar su microservicio en Kubernetes.
  - Leer un YAML de Deployment/Service.
  - Ver logs de pods (`kubectl logs`).
  - Hacer debug básico en un cluster.
- Jenkins, Kubernetes, CI/CD → son parte de la visión **end-to-end**: un dev senior no solo programa, también entiende cómo y dónde corre su código.

---

## 13. Imágenes comunes en Docker Hub

En Docker Hub podés encontrar imágenes oficiales de muchas tecnologías clave:

- **Mongo** → base de datos NoSQL orientada a documentos JSON.
- **Node** → runtime de JavaScript (para backends y build de frontends como Next.js).
- **Nginx** → servidor web / proxy reverso / balanceador de carga.
- **Postgres** → base de datos relacional SQL.
- **Redis** → base de datos en memoria, usada como cache, cola de mensajes y almacenamiento rápido.

Ejemplo de uso rápido:
```bash
docker run -p 27017:27017 mongo:7
docker run -p 5432:5432 -e POSTGRES_PASSWORD=1234 postgres
docker run -p 6379:6379 redis
docker run -p 8080:80 nginx
docker run -it node:20 node -v
```

---

## 14. Flujo completo para deployar una aplicación con Docker

### Paso 1: Generar la imagen
- En tu PC de desarrollo, con el `Dockerfile`:
  ```bash
  docker build -t miapp:1.0 .
  ```
- Esto crea una **imagen local** llamada `miapp:1.0`.

---

### Paso 2: Opciones para mover la imagen a otra PC

#### Opción A: Exportar como archivo `.tar`
1. En tu PC:
   ```bash
   docker save -o miapp.tar miapp:1.0
   ```
   → Genera un archivo `miapp.tar`.
2. Lo copiás (USB, email, drive).
3. En la otra PC:
   ```bash
   docker load -i miapp.tar
   docker run -p 3000:3000 miapp:1.0
   ```

#### Opción B: Subirla a un registro (Docker Hub, GHCR, etc.)
1. Taggear la imagen:
   ```bash
   docker tag miapp:1.0 usuario/miapp:1.0
   ```
2. Subirla:
   ```bash
   docker push usuario/miapp:1.0
   ```
3. En otra PC, solo necesitás:
   ```bash
   docker pull usuario/miapp:1.0
   docker run -p 3000:3000 usuario/miapp:1.0
   ```

#### Opción C: Construir de nuevo en otra PC
1. En la otra PC, bajás el código (`git clone` o zip).  
2. Ejecutás:
   ```bash
   docker build -t miapp:1.0 .
   docker run -p 3000:3000 miapp:1.0
   ```
👉 No necesitás instalar Node ni dependencias en esa PC, solo Docker.

---

### Paso 3: ¿Qué necesita una PC “vacía”?
- Solo **Docker Desktop** (Windows/Mac) o **Docker Engine** (Linux).  
- Nada más: Node, Mongo, etc. van **dentro de los contenedores**.  
- Opcional: `docker-compose` si tenés varios servicios (ej. app + Mongo).

---

### Paso 4: Opciones de despliegue

- **Con `docker run`**: levantar un contenedor puntual.  
- **Con `docker-compose.yml`**: levantar múltiples servicios conectados (app + db).  
- **Con registro (Docker Hub / GHCR)**: distribuir imágenes fácilmente entre equipos.  
- **En un cluster (Kubernetes/OpenShift)**: usado en producción, escala y balancea.

---

### Ejemplo práctico con docker-compose

Archivo `docker-compose.yml`:
```yaml
services:
  mongo:
    image: mongo:7
    ports: ["27017:27017"]

  app:
    image: usuario/miapp:1.0
    ports: ["3000:3000"]
    environment:
      - MONGODB_URI=mongodb://mongo:27017
```

Comando:
```bash
docker compose up -d
```
→ Levanta **tu app** + **Mongo** en otra PC con solo tener Docker.

---

# ✅ Resumen final corto

1. **Dockerfile → build → Imagen → run → Contenedor.**
2. **docker-compose.yml** simplifica levantar múltiples contenedores.
3. **Volúmenes y puertos** hacen que los contenedores guarden datos y se comuniquen.
4. **Imágenes se guardan en registros** (Docker Hub, GHCR, OpenShift Registry).
5. **Kubernetes** orquesta contenedores en clusters (Pods, Deployments, Services).
6. **OpenShift** = Kubernetes con interfaz enterprise.
7. **Jenkins** = servidor para pipelines CI/CD → automatiza build, tests, imágenes y despliegues.
8. **CI/CD** = filosofía de integrar y desplegar automáticamente.
9. **CDN** sirve estáticos desde nodos cercanos; **Serverless** ejecuta funciones bajo demanda.
10. A un **dev senior** se le pide saber esto porque da visión completa: código → contenedor → cluster → producción.
11. **Docker Hub** es el registro más común; **GHCR** se usa mucho si ya trabajás en GitHub.
12. Imágenes oficiales útiles: **Mongo, Node, Nginx, Postgres, Redis**.
13. Para mover una app entre PCs: exportar `.tar`, subir a un registro o reconstruir en la otra máquina.
---

## Ver también

- [[Guia doker]]
- [[CI CD]]
- [[deploy-process-guide-detailed]]