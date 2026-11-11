# Dificultades y soluciones: Servidor de telefonía IP con Asterisk (Alpine + Docker)

> **Regla de este documento:** solo se registran dificultades que **realmente ocurrieron**, con el mensaje exacto del error y la evidencia. La sección "Dificultades probables" es una guía de preparación: si una no ocurrió, no se copia a la tabla principal.

## Resumen

| N° | Dificultad | Área | Estado |
|---|---|---|---|
| 1 | El contenedor no se crea (`No such container: asterisk`) | Docker | [Resuelta / En curso: COMPLETAR] |
| 2 | [COMPLETAR] | | |

---

## Dificultad 1: El contenedor `asterisk` no existe

**Fecha:** 11/11/2026   **Responsable:** [COMPLETAR]

**Síntoma (mensaje exacto):**

```
Error response from daemon: No such container: asterisk
```

Apareció al ejecutar `docker logs asterisk` dentro de `~/asterisk`, después de intentar levantar el servicio.

**Qué significa:** Docker no tiene ningún contenedor con ese nombre. El problema no está en Asterisk sino antes: el despliegue (`docker compose up`) falló o nunca se ejecutó correctamente, probablemente en el `build` de la imagen.

**Diagnóstico realizado (en este orden):**

```bash
ls
ls config
docker ps -a
docker compose up -d --build
docker build -t asterisk-test .
```

| Revisión | Qué se buscaba | Qué se observó |
|---|---|---|
| `ls` y `ls config` | Que existan `Dockerfile` (con D mayúscula), `docker-compose.yml` y los 3 archivos de `config/` | [COMPLETAR] |
| `docker compose up -d --build` | El mensaje de error del build, al final de la salida | [COMPLETAR] |
| `docker ps -a` | Si el contenedor existe aunque esté detenido | [COMPLETAR] |
| `docker build -t asterisk-test .` | El error completo del build por separado | [COMPLETAR] |

**Causa identificada:** [COMPLETAR con el mensaje exacto del error]

**Solución aplicada:** [COMPLETAR con el comando o cambio exacto]

**Verificación:**

```bash
docker compose up -d
docker ps
docker logs asterisk
```

Resultado esperado: el contenedor `asterisk` aparece como `Up` y los logs muestran el inicio de Asterisk.
Resultado real: [COMPLETAR]

**Evidencia:** `evidencias/capturas/05-error.png`, `evidencias/capturas/05-solucion.png`

**Aprendizaje:** [COMPLETAR con sus palabras. Por ejemplo, por qué conviene revisar primero el estado del contenedor y la salida del despliegue antes de leer los logs.]

---

## Plantilla para nuevas dificultades

Copia este bloque por cada dificultad real.

```markdown
## Dificultad N: [título corto]

**Fecha:**   **Responsable:**

**Síntoma (mensaje exacto):**
(pegar el error)

**Diagnóstico realizado:**
(comandos y qué se observó con cada uno)

**Causa identificada:**

**Solución aplicada:**
(comando o cambio exacto)

**Verificación:**
(comando y resultado)

**Evidencia:** (ruta de la captura)

**Aprendizaje:**
```

---

## Dificultades probables en este proyecto (guía de preparación)

Estas situaciones son habituales en un despliegue de Asterisk en Alpine + Docker. **No son parte del registro** hasta que ocurran de verdad.

| Situación | Cómo se manifiesta | Qué revisar | Posible solución |
|---|---|---|---|
| Paquete `asterisk` no encontrado en Alpine | `unable to select packages: asterisk` durante el build | `apk search asterisk`; si el repositorio *community* está habilitado en `/etc/apk/repositories` | Habilitar *community* o usar otra versión de Alpine en el `FROM` |
| `docker compose` no existe | `'compose' is not a docker command` | `docker compose version` | `apk add docker-cli-compose` |
| Docker no está corriendo | `Cannot connect to the Docker daemon` | `service docker status` | `service docker start` |
| El contenedor se cierra o reinicia | Estado `Restarting` o `Exited` | `docker logs asterisk` | Corregir el error de sintaxis del `.conf` indicado en el log |
| Sin red o DNS en la VM durante el build | `apk add` se queda esperando o falla por red | `ping -c 3 dl-cdn.alpinelinux.org` | Revisar red/DNS de la VM |
| Los clientes no se registran | El softphone no pasa a "registrado"; `pjsip show contacts` vacío | Que estén en la misma red; IP del servidor; `pjsip set logger on` para ver si llega el `REGISTER` | Corregir IP/credenciales; revisar firewall de la VM y de Proxmox |
| Se registran pero no hay audio | La llamada conecta pero se oye silencio o solo un sentido | Rango RTP (10000-10100/udp), NAT, `rtp_symmetric`/`force_rport`, `network_mode` | Mantener `network_mode: host` y abrir los puertos RTP |
| Cambio en `pjsip.conf` que no se aplica | La extensión sigue con los datos anteriores | `pjsip show endpoint 1001` | `pjsip reload` o `docker restart asterisk` |
| Extensión duplicada | Error o comportamiento extraño en `pjsip show endpoints` | Que `1001` o `1002` no estén definidas dos veces | Eliminar el bloque repetido y recargar |
| La VM no responde al ping desde otro equipo | Sin respuesta desde el otro equipo | Misma red; red de la VM en modo bridge; firewall de Proxmox | Ajustar la red de la VM o consultar al docente del laboratorio |

---

## Lecciones aprendidas del proyecto

[COMPLETAR al final, con sus palabras: 3 a 5 lecciones que salieron de las dificultades reales.]
