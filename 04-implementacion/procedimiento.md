# Procedimiento

## Preparación
1. Instalar y verificar Docker Engine y Compose.
2. Confirmar IP del host y conectividad de los clientes.
3. Copiar `.env.example` como `.env` en `docker/`; definir secretos locales y no versionar `.env`.
4. Reemplazar los secretos de ejemplo en `docker/config/pjsip.conf` con los mismos valores.

## Despliegue
Desde `04-implementacion/docker/`:
```sh
docker compose config
docker compose up --build -d
docker compose ps
docker compose logs -f asterisk
```

## Registro y llamada
1. Configurar los softphones con IP del host, extensión, secreto local, UDP y puerto 5060.
2. Confirmar contactos registrados en Asterisk.
3. Llamar de 1001 a 1002 y en sentido inverso; verificar audio bidireccional.
4. Guardar evidencias sin secretos.

Detener con `docker compose down`. Confirmar que la imagen construida contiene los módulos PJSIP requeridos.
