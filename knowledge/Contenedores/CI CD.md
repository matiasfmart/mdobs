# Guía de Procesos de Deploy con Docker y CI/CD

> 📘 **Objetivo**: Entender paso a paso cómo funciona el despliegue de aplicaciones, desde lo **manual** hasta lo **full automático**, con explicaciones detalladas, buenas prácticas y diferencias entre proyectos personales y grandes corporaciones.  

---

## 1. Proceso Manual (básico / proyectos personales)

### Descripción
El desarrollador controla **cada paso a mano**: construir la imagen, correr el contenedor y compartirlo con el usuario final.  
Es la forma ideal para **aprender** cómo funciona Docker y CI/CD.

### Pasos
1. **Escribir Dockerfile** en el repositorio del proyecto.  
   - Contiene las instrucciones para empaquetar la app.
2. **Construir imagen localmente**:  
   ```bash
   docker build -t miapp:1.0 .
   ```
3. **Ejecutar contenedor** con `.env`:  
   ```bash
   docker run -d --name miapp-container --env-file .env -p 3000:3000 miapp:1.0
   ```
4. **Probar en localhost** (`http://localhost:3000`).
5. (Opcional) **Exportar imagen** a archivo `.tar` para compartir:  
   ```bash
   docker save -o miapp.tar miapp:1.0
   ```
   y luego importarla en otra PC:
   ```bash
   docker load -i miapp.tar
   ```

### Uso típico
- Proyectos personales.
- Pocas personas involucradas.
- Ideal para aprendizaje o MVPs.

### Pros
- Control absoluto.
- Sin necesidad de servicios externos.

### Contras
- Mucho trabajo manual.
- Propenso a errores humanos.
- Escalabilidad limitada.

---

## 2. Proceso Semi-Automático (intermedio)

### Descripción
Se combinan pasos manuales con algo de automatización:  
- Se suben las imágenes a un **registro (Docker Hub, GHCR, etc.)**.  
- Los testers/usuarios solo tienen que hacer `docker pull` + `docker run`.  
- Opcionalmente se usan scripts (`.bat`, `.ps1`, `bash`) para simplificar.

### Pasos
1. **Construir y etiquetar imagen**:
   ```bash
   docker build -t usuario/miapp:1.2 .
   docker tag usuario/miapp:1.2 usuario/miapp:latest
   ```
2. **Publicar imagen en registro**:
   ```bash
   docker push usuario/miapp:1.2
   docker push usuario/miapp:latest
   ```
3. **Usuario final** solo hace:
   ```bash
   docker pull usuario/miapp:latest
   docker run -d --name miapp --env-file .env -p 3000:3000 usuario/miapp:latest
   ```
4. (Opcional) Entregar un **script de inicio** que:  
   - Verifique si Docker está corriendo.  
   - Arranque el contenedor si está detenido o lo cree si no existe.  
   - Abra el navegador automáticamente.

### Uso típico
- Proyectos en prueba con testers.  
- Equipos pequeños (2–5 personas).  
- Startups en fase temprana.

### Pros
- Mucho menos trabajo manual para el tester.  
- Se asegura que todos usan la misma imagen.  
- Más parecido al flujo real de producción.

### Contras
- El dev sigue teniendo que **construir + pushear** manualmente.  
- Sin control de versiones ni rollback automático.  

---

## 3. Proceso Full Automático (profesional / empresas grandes)

### Descripción
Todo el flujo se gestiona con **CI/CD pipelines** (Jenkins, GitHub Actions, GitLab CI, Azure DevOps, etc.).  
Cada vez que hay un **push a GitHub**:
1. Se ejecutan tests automáticos.  
2. Se construye la imagen Docker.  
3. Se publica en un registro (Docker Hub, GHCR, ECR, etc.).  
4. Se despliega automáticamente en un **entorno de pruebas** (staging).  
5. Si pasa QA, se promueve a **producción**.

### Pasos
1. **Configurar pipeline CI/CD** con un archivo declarativo:
   - Ejemplo GitHub Actions (`.github/workflows/deploy.yml`):
     ```yaml
     name: Build and Deploy
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
     ```
2. **CI** valida: build + tests + análisis de seguridad.  
3. **CD** publica en el entorno correcto (Dev → QA → Prod).  
4. Orquestadores como **Kubernetes/OpenShift** levantan pods automáticamente desde la nueva imagen.  
5. Se monitorean logs y métricas.

### Uso típico
- Grandes corporaciones.  
- Startups en crecimiento con múltiples devs y testers.  
- Sistemas críticos (bancos, e-commerce, salud).

### Pros
- Cero intervención manual en despliegues.  
- Control de versiones y rollback fácil.  
- Escalabilidad y confiabilidad.  
- QA/seguridad integrados en el pipeline.

### Contras
- Complejidad alta.  
- Necesita conocimientos de CI/CD + DevOps.  
- Requiere infraestructura (nube, clusters, pipelines).

---

## 4. Buenas prácticas generales

### Para proyectos personales
- Usar **tags fijos** (`latest`, `tester`) para no acumular imágenes.  
- Reutilizar scripts de inicio (`.bat`, `.ps1`).  
- Mantener `.env.example` en el repo (sin credenciales reales).  
- Documentar los comandos básicos en un `README.md`.

### Para empresas
- Separar **branches** (`dev`, `staging`, `main`).  
- Usar **versionado semántico** (`1.2.3`).  
- Mantener un **registro privado** de imágenes.  
- Automatizar pipelines (CI/CD).  
- Integrar **tests + seguridad** antes del deploy.  
- Orquestar con Kubernetes/OpenShift para alta disponibilidad.  

---

## 5. Comparativa Rápida

| Aspecto              | Manual (Personal)            | Semi-Automático (Pequeños equipos) | Full Automático (Empresas) |
|-----------------------|------------------------------|------------------------------------|----------------------------|
| Construcción          | Manual (dev)                 | Manual + push a registro           | Automática en pipeline     |
| Distribución          | Export `.tar` o repo Git     | Registro público/privado           | Registro + pipeline        |
| Ejecución             | `docker run` manual          | Script `.bat` / `.ps1` / `compose` | Orquestador (K8s/OpenShift)|
| Actualización         | Rebuild manual               | Rebuild + push                     | Automática en cada commit  |
| Escalabilidad         | Baja                         | Media                              | Alta                       |
| Ideal para            | Aprender, MVP                | Testing colaborativo               | Producción crítica         |

---

# ✅ Conclusión

- **Aprendé primero el flujo manual** → te da claridad de cada comando.  
- **Usá el semi-automático** para testers y proyectos propios con amigos.  
- **Estudiá el full automático (CI/CD + K8s)** para entrevistas y seniority.  

En una entrevista de **Senior**, esperan que **entiendas los tres**:  
- Manual (demuestra que sabés cómo funciona por dentro).  
- Semi-auto (cómo simplificar la vida al tester).  
- Full-auto (visión completa de DevOps en empresa).  

---

## Ver también

- [[Docker Contenedores y Despliegue]]
- [[deploy-process-guide-detailed]]
- [[Guia doker]]
