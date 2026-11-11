# Requisitos

## Funcionales
- Asterisk inicia y acepta registros SIP.
- Al menos dos clientes usan extensiones distintas.
- Se completan llamadas internas y el audio se escucha en ambos sentidos.
- Los logs permiten observar registro y llamadas.

## Técnicos
- Docker Engine y Docker Compose.
- Conectividad IP entre host y clientes.
- SIP UDP/TCP 5060 y RTP UDP 10000–10100 disponibles según la topología.
- Dos softphones y secretos de laboratorio locales excluidos de Git.

## Seguridad
- Usar secretos únicos; no reutilizar credenciales personales.
- No exponer SIP/RTP directamente a Internet.
- Restringir puertos al segmento de laboratorio y ocultar secretos en evidencias.
