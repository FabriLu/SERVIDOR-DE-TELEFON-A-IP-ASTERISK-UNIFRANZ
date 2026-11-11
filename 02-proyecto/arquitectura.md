# Arquitectura

| Componente | Descripción / dato del equipo |
|---|---|
| Host Docker | Completar sistema operativo, versión y recursos. |
| Contenedor | Asterisk desde Alpine Linux; servicio Compose `asterisk`. |
| Red | Red local; completar segmento, gateway e IP del host. |
| Clientes | Dos o más softphones en equipos o celulares. |
| SIP | UDP/TCP 5060. |
| RTP | UDP 10000–10100; ajustar firewall si cambia el rango. |

Los softphones se registran en la IP del host por SIP 5060. Asterisk enruta llamadas entre 1001 y 1002; el audio usa RTP. Docker Desktop, NAT o firewalls pueden requerir ajustes para clientes en redes distintas.

```text
Softphone 1001 ── SIP 5060 ──┐
                             ├── Host Docker ── Asterisk
Softphone 1002 ── SIP 5060 ──┘        └── RTP UDP 10000–10100
```

Completar con las direcciones y el diagrama reales del laboratorio.
