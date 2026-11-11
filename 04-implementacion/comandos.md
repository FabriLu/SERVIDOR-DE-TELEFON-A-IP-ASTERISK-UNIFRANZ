# Comandos

Ejecutar desde `04-implementacion/docker/`.

| Comando | Uso |
|---|---|
| `docker compose config` | Validar configuración Compose. |
| `docker compose up --build -d` | Construir e iniciar. |
| `docker compose ps` | Ver estado. |
| `docker compose logs -f asterisk` | Seguir logs. |
| `docker compose exec asterisk asterisk -rx "pjsip show endpoints"` | Consultar endpoints. |
| `docker compose exec asterisk asterisk -rx "pjsip show contacts"` | Consultar registros. |
| `docker compose exec asterisk asterisk -rvvv` | Abrir consola Asterisk. |
| `docker compose down` | Detener y retirar contenedor. |

Registrar otros comandos, fecha y resultado.
