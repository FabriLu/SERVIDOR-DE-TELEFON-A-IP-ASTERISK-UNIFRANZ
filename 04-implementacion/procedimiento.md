# 03 - Procedimientos: Servidor de telefonía IP con Asterisk (Alpine Linux + Docker)

> **Cómo usar este documento:** los campos marcados con **[COMPLETAR]** deben llenarse con lo que realmente ocurrió. Si un paso no se ejecutó o dio un resultado distinto al esperado, se anota tal cual en la bitácora (`05-bitacora.md`). Las capturas se guardan en `evidencias/capturas/`.

## Resumen del entorno

| Elemento | Valor |
|---|---|
| Máquina | VM en servidor Proxmox |
| Sistema operativo | Alpine Linux **[versión: COMPLETAR]** |
| Contenedores | Docker + Docker Compose |
| Servicio | Asterisk (PJSIP) |
| IP de la VM | **[COMPLETAR]** |
| Puertos usados | 5060/udp (SIP), 10000-10100/udp (RTP) |
| Extensiones | 1001, 1002 |

---

## Paso 1. Verificar la base (Alpine + Docker)

**Objetivo:** confirmar que Docker y Docker Compose funcionan y anotar la IP de la VM.

```bash
docker --version
docker compose version
service docker status
ip a
```

- `ip a` muestra la IP que usarán los clientes SIP como servidor.
- Si falta Compose: `apk add docker-cli-compose`.

**Resultado esperado:** Docker en ejecución y una IP en la red del laboratorio.
**Resultado real:** [COMPLETAR]
**Evidencia:** `evidencias/capturas/01-docker-ip.png`

---

## Paso 2. Crear la estructura del proyecto

**Objetivo:** preparar la carpeta del despliegue.

```bash
mkdir -p ~/asterisk/config
cd ~/asterisk
```

**Resultado esperado:** carpeta `~/asterisk` con subcarpeta `config`.
**Evidencia:** `evidencias/capturas/02-estructura.png`

---

## Paso 3. Crear los archivos de configuración

### 3.1 Dockerfile

Construye una imagen propia sobre Alpine con Asterisk. `-f` mantiene el proceso en primer plano para que el contenedor no se cierre.

```bash
cat > Dockerfile << 'EOF'
FROM alpine:3.20

RUN apk add --no-cache asterisk asterisk-sample-config \
    asterisk-sounds-en asterisk-sounds-moh

CMD ["asterisk", "-f", "-vvv"]
EOF
```

### 3.2 config/pjsip.conf (extensiones SIP)

Cada extensión se compone de tres objetos de PJSIP: **endpoint** (el dispositivo y sus códecs), **auth** (credenciales) y **aor** (dónde se registra).

```bash
cat > config/pjsip.conf << 'EOF'
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0:5060

[1001]
type=endpoint
context=internas
disallow=all
allow=ulaw,alaw
auth=1001-auth
aors=1001
direct_media=no
rtp_symmetric=yes
force_rport=yes
rewrite_contact=yes

[1001-auth]
type=auth
auth_type=userpass
username=1001
password=CambiarClave1001

[1001]
type=aor
max_contacts=1

[1002]
type=endpoint
context=internas
disallow=all
allow=ulaw,alaw
auth=1002-auth
aors=1002
direct_media=no
rtp_symmetric=yes
force_rport=yes
rewrite_contact=yes

[1002-auth]
type=auth
auth_type=userpass
username=1002
password=CambiarClave1002

[1002]
type=aor
max_contacts=1
EOF
```

> **Seguridad:** las contraseñas de este documento son de **ejemplo**. No publicar las reales en el repositorio.

### 3.3 config/extensions.conf (plan de marcado)

Define qué pasa cuando una extensión marca otra (`_100X` coincide con 1000-1009) y una extensión 600 de prueba de eco.

```bash
cat > config/extensions.conf << 'EOF'
[internas]
exten => _100X,1,NoOp(Llamada a ${EXTEN})
 same => n,Dial(PJSIP/${EXTEN},20)
 same => n,Hangup()

exten => 600,1,Answer()
 same => n,Echo()
 same => n,Hangup()
EOF
```

### 3.4 config/rtp.conf (puertos de audio)

Limita el rango de puertos RTP para poder abrirlos en un firewall.

```bash
cat > config/rtp.conf << 'EOF'
[general]
rtpstart=10000
rtpend=10100
EOF
```

### 3.5 docker-compose.yml

`network_mode: host` evita los problemas de NAT de Docker con SIP/RTP (audio en un solo sentido o sin audio).

```bash
cat > docker-compose.yml << 'EOF'
services:
  asterisk:
    build: .
    container_name: asterisk
    network_mode: host
    restart: unless-stopped
    volumes:
      - ./config/pjsip.conf:/etc/asterisk/pjsip.conf
      - ./config/extensions.conf:/etc/asterisk/extensions.conf
      - ./config/rtp.conf:/etc/asterisk/rtp.conf
EOF
```

**Resultado esperado:** los cinco archivos existen (`ls` y `ls config`).
**Evidencia:** `evidencias/capturas/03-archivos.png`

---

## Paso 4. Construir y levantar el contenedor

```bash
docker compose up -d --build
docker ps
docker logs asterisk
```

**Resultado esperado:** el contenedor `asterisk` aparece como `Up`.
**Resultado real:** [COMPLETAR]
**Evidencia:** `evidencias/capturas/04-compose-up.png`

> Al ejecutar `docker logs asterisk` apareció `Error response from daemon: No such container: asterisk`. Esto indica que el contenedor no se creó. Ver el paso 5.

---

## Paso 5. Diagnóstico y solución de dificultades

**Dificultad encontrada:** `No such container: asterisk` al ejecutar `docker logs asterisk`.

**Orden de diagnóstico aplicado:**

```bash
ls
ls config
docker ps -a
docker compose up -d --build
docker build -t asterisk-test .
```

| Revisión | Qué se buscó | Resultado |
|---|---|---|
| `ls` y `ls config` | Nombres exactos de los archivos (`Dockerfile`, `docker-compose.yml`) | [COMPLETAR] |
| Salida de `docker compose up` | Mensaje de error del build | [COMPLETAR] |
| `docker ps -a` | Si el contenedor existe aunque esté detenido | [COMPLETAR] |

**Causa real:** [COMPLETAR con el mensaje exacto del error]
**Solución aplicada:** [COMPLETAR. Si se cambió la versión de Alpine, el repositorio *community* u otra cosa, anotar el comando exacto.]

Una vez resuelto:

```bash
docker compose up -d
docker ps
docker logs asterisk
```

**Evidencia:** `evidencias/capturas/05-error.png` y `evidencias/capturas/05-solucion.png`

---

## Paso 6. Comprobar Asterisk desde su consola

```bash
docker exec -it asterisk asterisk -rvvv
```

Dentro de la consola:

```
pjsip show endpoints
dialplan show internas
exit
```

**Resultado esperado:** 1001 y 1002 aparecen como endpoints (en estado *Unavailable* hasta que se registren los clientes) y el contexto `internas` muestra las reglas.
**Resultado real:** [COMPLETAR]
**Evidencia:** `evidencias/capturas/06-consola-asterisk.png`

---

## Paso 7. Verificar el acceso desde otros equipos

**Objetivo:** demostrar que el servicio es accesible desde la red, no solo desde la VM (requisito de la misión).

En la VM:

```bash
ip a
ip route
netstat -lnup | grep 5060
docker exec -it asterisk asterisk -rx "pjsip show transports"
iptables -L -n
```

En **otro equipo** de la misma red:

```
ping IP-DE-LA-VM
```

Para comprobar que el tráfico SIP llega a Asterisk:

```bash
docker exec -it asterisk asterisk -rvvv
```
```
pjsip set logger on
```

Luego se intenta registrar un softphone y en la consola debe verse un `REGISTER` entrante.

**Resultado esperado:** la VM responde al ping, Asterisk escucha en 5060/udp y se ve el tráfico SIP.
**Resultado real:** [COMPLETAR]
**Evidencia:** `evidencias/capturas/07-ping-otro-equipo.png`, `07-puerto-5060.png`, `07-register-log.png`

> Si hay un firewall que bloquea, abrir 5060/udp y 10000-10100/udp y documentar el comando usado en la bitácora.

---

## Paso 8. Registrar los clientes SIP

Se usan softphones (Linphone, Zoiper o MicroSIP) en **dos equipos o celulares distintos**.

| Dato | Cliente 1 | Cliente 2 |
|---|---|---|
| Usuario | 1001 | 1002 |
| Contraseña | (ver `pjsip.conf`) | (ver `pjsip.conf`) |
| Servidor / dominio | IP de la VM | IP de la VM |
| Puerto / transporte | 5060 / UDP | 5060 / UDP |
| Softphone y equipo usado | [COMPLETAR] | [COMPLETAR] |

Verificación en el servidor:

```bash
docker exec -it asterisk asterisk -rx "pjsip show contacts"
```

**Resultado esperado:** ambos contactos aparecen como **Avail**.
**Resultado real:** [COMPLETAR]
**Evidencia:** `evidencias/capturas/08-contactos.png`, `08-softphone-1001.png`, `08-softphone-1002.png`

---

## Paso 9. Realizar llamadas internas

1. Desde 1001 llamar a **1002** y contestar.
2. Desde 1002 llamar a **1001**.
3. Marcar **600** para la prueba de eco.
4. Con una llamada activa:

```bash
docker exec -it asterisk asterisk -rx "core show channels"
```

| Prueba | Resultado | Audio en ambos sentidos | Evidencia |
|---|---|---|---|
| 1001 → 1002 | [COMPLETAR] | [sí/no] | `09-llamada-1001-1002.png` |
| 1002 → 1001 | [COMPLETAR] | [sí/no] | `09-llamada-1002-1001.png` |
| 600 (eco) | [COMPLETAR] | [sí/no] | `09-eco.png` |

---

## Paso 10. Gestión de extensiones

Para modificar una extensión, se edita `config/pjsip.conf` y se recarga:

```bash
docker exec -it asterisk asterisk -rx "pjsip reload"
docker exec -it asterisk asterisk -rx "pjsip show endpoint 1001"
docker exec -it asterisk asterisk -rx "pjsip show auth 1001-auth"
```

Si el cambio no se refleja (algunos editores reemplazan el archivo y rompen el montaje):

```bash
docker restart asterisk
```

> **Nota:** para añadir una extensión **nueva** se agregan sus tres bloques (endpoint, auth y aor). No volver a agregar 1001 o 1002, porque se duplicarían.

---

## Archivos de despliegue

Todo lo necesario para reproducir el servicio está en la carpeta `despliegue/`:

```
despliegue/
├── Dockerfile
├── docker-compose.yml
└── config/
    ├── pjsip.conf
    ├── extensions.conf
    └── rtp.conf
```

Para reproducirlo en otra VM con Alpine y Docker:

```bash
cd despliegue
docker compose up -d --build
```

---

## Observaciones finales

- Cambios respecto a lo planificado: [COMPLETAR]
- Pendientes: [COMPLETAR]
