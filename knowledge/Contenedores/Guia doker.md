# Guía rápida de Docker — *Grace Hub*

> **Objetivo:** tener a mano los comandos esenciales de Docker, más los **comandos exactos** para tu proyecto **Grace Hub** (Next.js + Mongo Atlas).  
> Requisitos: **Docker Desktop** instalado y corriendo (Windows/Mac) o Docker Engine (Linux), y un archivo `.env` válido al lado de donde ejecutes los comandos.

---

## 0) Terminología mínima
- **Imagen**: “paquete” listo para correr (incluye Node, dependencias, build).  
- **Contenedor**: instancia **en ejecución** de una imagen.  
- **Tag**: etiqueta/versionado de imagen (`:latest`, `:1.2`, `:tester`).  
- **Registro**: dónde se publican imágenes (Docker Hub, GHCR, ECR, etc.).

---

## 1) Comandos esenciales

### Ver estado/versión
```bash
docker --version
docker info
```

### Listar
```bash
docker images           # imágenes locales
docker ps               # contenedores corriendo
docker ps -a            # contenedores (incluye parados)
```

### Build (crear imagen desde Dockerfile en carpeta actual)
```bash
docker build -t NOMBRE:TAG .
# Ejemplo genérico
docker build -t miapp:1.0 .
```

### Tag (renombrar/etiquetar una imagen ya construida)
```bash
docker tag NOMBRE:TAG_ORIG destino/imagen:TAG_NUEVO
# Ejemplo
docker tag miapp:1.0 usuario/miapp:1.0
```

### Run (crear y arrancar un contenedor)
```bash
docker run [opciones] IMAGEN[:TAG]

# Opciones útiles
# --name            nombre del contenedor
# -d                “detached” (en background)
# --env-file .env   cargar variables de entorno desde .env
# -e CLAVE=VALOR    pasar variable puntual
# -p HOST:CONT      mapear puertos (HOST→CONTENEDOR)
# -v host:cont      montar volumen/carpeta
```

### Logs / shell dentro del contenedor
```bash
docker logs -f NOMBRE_CONTENEDOR   # seguir logs
docker exec -it NOMBRE_CONTENEDOR sh   # entrar con shell (Linux/Alpine)
```

### Stop/Start/Remove
```bash
docker stop NOMBRE_CONTENEDOR
docker start NOMBRE_CONTENEDOR
docker rm NOMBRE_CONTENEDOR
docker rmi NOMBRE_IMAGEN:TAG
```

### Limpieza
```bash
docker container prune -f   # borra contenedores parados
docker image prune -f       # borra imágenes “dangling” no usadas
docker system prune -af     # ⚠️ borra TODO lo no usado (cuidadito)
```

---

## 2) Publicar/Descargar del registro

### Docker Hub — login, push y pull
```bash
docker login                             # autenticación
docker push usuario/imagen:TAG           # publicar
docker pull usuario/imagen:TAG           # descargar
```

### Multi-arquitectura (M1 arm64 + Windows/PC amd64)
```bash
# crear/usar builder multi-arch (una vez)
docker buildx create --name multi --use 2>/dev/null || docker buildx use multi

# build + push en ambas arquitecturas al mismo tag
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t usuario/imagen:latest \
  -t usuario/imagen:1.2.0 \
  --push .

# inspeccionar manifest
docker buildx imagetools inspect usuario/imagen:latest
```

---

## 3) Docker Compose (opcional)

**Archivo `docker-compose.yml` (ejemplo mínimo usando imagen ya publicada):**
```yaml
services:
  app:
    image: usuario/imagen:latest
    env_file: .env
    ports:
      - "3000:3000"
    # pull_policy: always   # en Compose v2.21+ para forzar pull cada up
```

**Comandos:**
```bash
docker compose up -d         # levantar
docker compose up -d --build # reconstruye si hay build context
docker compose logs -f       # ver logs
docker compose down          # apagar (y liberar red)
docker compose down --rmi local --volumes  # ⚠️ más agresivo
```

---

## 4) **Grace Hub** — comandos exactos de tu proyecto

### A) Usando **imagen publicada en Docker Hub** (tester)
> Imagen: `matiasfmartz/grace-hub:latest` — Puerto: **3000** — .env en la carpeta actual

**Primera vez (si la imagen no está local):**
```bash
docker pull matiasfmartz/grace-hub:latest
```

**Arrancar (crea si no existe / inicia si está detenido):**
```bash
# crear + correr
docker run -d --name grace-hub-container --env-file .env -p 3000:3000 matiasfmartz/grace-hub:latest

# ver logs
docker logs -f grace-hub-container
```

**Parar y borrar (si necesitás limpiar):**
```bash
docker stop grace-hub-container
docker rm grace-hub-container
```

**Actualizar a una nueva imagen (cuando vos publicás un fix):**
```bash
docker stop grace-hub-container
docker rm grace-hub-container
docker pull matiasfmartz/grace-hub:latest
docker run -d --name grace-hub-container --env-file .env -p 3000:3000 matiasfmartz/grace-hub:latest
```

### B) Build **local** desde tu `Dockerfile` (si querés probar sin subir a Hub)
> Usa Node `24.6.0-alpine` y `output: 'standalone'` en `next.config.ts`

**Construir:**
```bash
docker build -t grace-hub:local .
```

**Correr:**
```bash
docker run -d --name grace-hub-local --env-file .env -p 3000:3000 grace-hub:local
docker logs -f grace-hub-local
```

**Publicar esa build local a tu Docker Hub (opcional):**
```bash
docker tag grace-hub:local matiasfmartz/grace-hub:latest
docker push matiasfmartz/grace-hub:latest
```

### C) Multi-arch de Grace Hub (para M1 + Windows x86 con un mismo tag)
```bash
docker buildx create --name multi --use 2>/dev/null || docker buildx use multi

docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t matiasfmartz/grace-hub:latest \
  -t matiasfmartz/grace-hub:1.2 \
  --push .
```

---

## 5) `.env` — ubicación y uso
- Poné el archivo `.env` en la **misma carpeta** donde corrés los comandos.  
- En `docker run`, usás `--env-file .env`.  
- **No subas** `.env` al repo. Compartí **`.env.example`**.

Ejemplo (`.env.example`):
```env
MONGODB_USER=
MONGODB_PASSWORD=
MONGODB_CLUSTER_URL=cluster0.xxxxx.mongodb.net
MONGODB_DB_NAME=grace-hub
NODE_ENV=production
PORT=3000
```

---

## 6) Troubleshooting rápido
- **“port is already allocated”**: el 3000 ya está ocupado.
  ```bash
  docker ps  # buscá qué contenedor usa 3000 y paralo
  docker stop grace-hub-container
  ```
  o corré en otro puerto `-p 3001:3000`.

- **“Cannot connect to the Docker daemon”**: Docker Desktop no está corriendo.
  - Abrí Docker Desktop y esperá a que esté “Running”.
  - En Windows podés iniciar `C:\Program Files\Docker\Docker\Docker Desktop.exe`.

- **Variables `.env` no aplican**: verificá ruta y nombre.
  - `--env-file .env` debe apuntar al archivo correcto (misma carpeta).
  - Chequeá que los valores no tengan comillas sobrantes o caracteres sin escapar.

- **Conexión a Mongo Atlas**: usá una URI válida (sin comillas) y con password URL-encoded si tiene símbolos.
  - Formato: `mongodb+srv://USER:PASS@CLUSTER_URL/DBNAME?retryWrites=true&w=majority`.

---

## 7) Scripts de inicio para usuarios Windows (resumen)
- `start-gracehub.bat` / `start-gracehub.ps1`:  
  - Verifican si Docker está corriendo; si no, **inician Docker Desktop** y esperan.  
  - Si el contenedor existe, **lo arrancan**; si no existe, **lo crean**.  
  - **Abren el navegador** en `http://localhost:3000`.

> Podés entregar el script + `.env` en la misma carpeta para doble-click sin terminal.

---

## 8) Mini checklist de ciclo de prueba manual
1. **Code fix** → commit → (opcional) build local.  
2. **Publicar** a Docker Hub (si querés que el tester lo baje):
   ```bash
   docker build -t matiasfmartz/grace-hub:latest .
   docker push  matiasfmartz/grace-hub:latest
   ```
3. **Tester** actualiza y corre:
   ```bash
   docker stop grace-hub-container && docker rm grace-hub-container
   docker pull matiasfmartz/grace-hub:latest
   docker run -d --name grace-hub-container --env-file .env -p 3000:3000 matiasfmartz/grace-hub:latest
   ```

---

## Ver también

- [[Docker Contenedores y Despliegue]]
- [[CI CD]]
- [[deploy-process-guide-detailed]]
