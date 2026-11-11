# Servidor de telefonía IP con Asterisk — UNIFRANZ

Proyecto académico para desplegar Asterisk en Alpine Linux con Docker, registrar al menos dos clientes SIP y probar llamadas internas.

## Integrantes
- Elia Solis
- Juan Manuel Chacon
- Gerardo Gutierrez
- Fabrizio Magne Luizaga

## Estructura
`01-equipo/` integrantes y roles · `02-proyecto/` objetivo y arquitectura · `03-investigacion/` requisitos y fuentes · `04-implementacion/` despliegue y configuración · `05-pruebas/` plan, resultados y clientes · `06-bitacora/` avances · `07-prompt-espejo/` uso de IA · `08-activitywatch/` periodo · `09-conclusiones/` conclusiones.

## Inicio rápido
1. Copia `04-implementacion/docker/.env.example` a `.env` en esa carpeta y define secretos locales.
2. Desde `04-implementacion/docker/`, ejecuta `docker compose up --build -d`.
3. Registra las extensiones 1001 y 1002 en dos clientes SIP de la misma red.
4. Sigue `04-implementacion/procedimiento.md` y `05-pruebas/plan-de-pruebas.md`.

Base didáctica para red local. Completa y verifica credenciales, direccionamiento, firewall y módulos de Asterisk en el entorno del equipo. No expongas SIP/RTP directamente a Internet. Las pruebas y bitácoras deben reflejar resultados reales; no se incluyen capturas ni resultados simulados.
