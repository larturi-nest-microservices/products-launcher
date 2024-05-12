# Nest Microservices Orders / Produts with NATS ApiGateway

## Dev

1. Clonar el repositorio
2. Crear un .env basado en el .env.template
3. Ejecutar el comando `git submodule update --init --recursive` para reconstruir los sub-módulos
4. Ejecutar el comando `docker compose up --build`

## Pasos para crear los Git Submodules

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

Si se hace al revés, se perderán las referencias de los sub-módulos en el repositorio principal y tendremos que resolver conflictosl.

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

## Para correr en Prod

1. Clonar el repositorio
2. Crear un .env basado en el .env.template
3. Ejecutar el comando:

```bash
docker compose -f docker-compose.prod.yaml build
```
