# Nest Microservices Orders / Produts with NATS ApiGateway

## Dev

1. Clonar el repositorio
2. Crear un .env basado en el .env.template
3. Ejecutar el comando `git submodule update --init --recursive` para reconstruir los sub-módulos
4. Ejecutar el comando `docker compose up --build`

## Pasos para crear nuevos Git Submodules

1. Crear un nuevo repositorio en GitHub
2. Clonar el repositorio en la máquina local
3. Añadir el submodule, donde `repository_url` es la url del repositorio y `directory_name` es el nombre de la carpeta donde quieres que se guarde el sub-módulo (no debe de existir en el proyecto)

```bash
git submodule add <repository_url> <directory_name>
```

1. Añadir los cambios al repositorio (git add, git commit, git push) Ej:

```bash
git add .
git commit -m "Add submodule"
git push
```

5. Inicializar y actualizar Sub-módulos, cuando alguien clona el repositorio por primera vez, debe de ejecutar el siguiente comando para inicializar y actualizar los sub-módulos

```bash
git submodule update --init --recursive
```

6. Para actualizar las referencias de los sub-módulos

```bash
git submodule update --remote
```

## Importante

Si se trabaja en el repositorio que tiene los sub-módulos, primero actualizar y hacer push en el sub-módulo y después en el repositorio principal.

Si se hace al revés, se perderán las referencias de los sub-módulos en el repositorio principal y tendremos que resolver conflictos.

## Diagrama Arquitectura

<https://excalidraw.com/#room=95cf8ca0048fc68731fc,ECONS8YjJHtbNBHdiBG1FA>

## Integración con Stripe para los pagos (Payment MS)

Para pruebas del endpoint `/payments/webhook` desde local usamos Hookdeck.
  
To receive events on your local server, install the Hookdeck CLI

- <https://dashboard.hookdeck.com/connections>

`hookdeck login`

`hookdeck listen 3003 stripe-to-localhost`

Edit the webhook endpoint URL in Stripe / Developers / Webhooks

- <https://dashboard.stripe.com/test/webhooks>

- Credit Card Test: 4242 4242 4242 4242

## En caso de problemas con los submodulos de git

```bash
# Agregar uno por uno los submodulos

git submodule add https://github.com/larturi-nest-microservices/products-microservice.git
git submodule add https://github.com/larturi-nest-microservices/orders-microservice.git
git submodule add https://github.com/larturi-nest-microservices/payments-microservice.git
git submodule add https://github.com/larturi-nest-microservices/auth-microservice.git
git submodule add https://github.com/larturi-nest-microservices/client-gateway.git
```

## Para correr en Prod con Kubernetes y subir a GCP

1. Clonar el repositorio products-launcher
2. Crear un .env basado en el .env.template
3. Ejecutar el comando:

```bash
docker compose -f docker-compose.prod.yaml build
```

Para subir imagenes al registro de Google Cloud. Ejemplo con orders-microservice:

```bash
cd orders-microservice

docker build -f dockerfile.prod -t southamerica-west1-docker.pkg.dev/cogent-calling-423723-h9/image-registry/orders-ms .
```

Mas info en `K8s.README.md`

## Paso a paso para actualizar un servicio y llevarlo a PROD en GCP

1. Una vez realizados los cambios en el repo del ms, hacer el push a la rama cloud-build se dispara el proceso de CI/CD siguiendo el script del archivo cloudbuild.yml de cada servicio.
2. Una vez finalizado, se genera la nueva imagen de Docker y se sube en Artifact Registry de GCP.
3. Con la imagen de Docker hosteada, desde local, parado en la carpeta `k8s/tienda-products-ms` actualizar el cluster con helm:

```bash
helm upgrade tienda-products-ms .
```

4. Para ver el estado de los pods:

```bash
kubectl get pods
```

5. Para ver los logs de un pod:

```bash
kubectl logs payments-ms-6fbc5d5bd6-6tcvf
```

## Servicios de Google Cloud

- Artifact Registry
- Cloud Build
- Secret Manager
  