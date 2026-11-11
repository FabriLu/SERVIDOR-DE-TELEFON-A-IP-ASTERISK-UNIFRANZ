# 05 - Bitácora de actividades: Servidor de telefonía IP con Asterisk

> **Responsable de esta bitácora:** Responsable de continuidad **[nombre: COMPLETAR]**
> **Regla:** se actualiza en cada sesión de trabajo, no al final. Solo se registra lo que realmente se hizo. Lo que aún no se ha ejecutado queda como **Pendiente**, nunca como hecho.

## Datos generales

| Campo | Valor |
|---|---|
| Cuadrilla N° | [COMPLETAR] |
| Proyecto | Servidor de telefonía IP – Asterisk (Alpine Linux + Docker) |
| Docente | Ing. Diego Cárdenas |
| Entorno | VM en servidor Proxmox, Alpine Linux, Docker |
| Repositorio | [COMPLETAR: enlace] |

## Registro de actividades

| N° | Fecha | Hora | Responsable | Actividad | Dificultad | Solución | Pendiente | Evidencia |
|---|---|---|---|---|---|---|---|---|
| 1 | 11/11/2026 | [COMPLETAR] | [COMPLETAR] | Lectura de la misión y selección del proyecto: Asterisk (Alpine + Docker) | Ninguna | N/A | Enviar correo al docente con nombres y roles (si aún no se hizo) | [COMPLETAR] |
| 2 | 11/11/2026 | [COMPLETAR] | [COMPLETAR] | Planificación de la estructura del repositorio GitHub (carpetas `docs/`, `evidencias/`, `despliegue/`) | Ninguna | N/A | Crear el repositorio público y agregar colaboradores | [COMPLETAR] |
| 3 | 11/11/2026 | [COMPLETAR] | [COMPLETAR] | Creación de la carpeta `~/asterisk` y de los archivos `Dockerfile`, `docker-compose.yml` y `config/` (`pjsip.conf`, `extensions.conf`, `rtp.conf`) en la VM | [COMPLETAR si hubo] | [COMPLETAR] | Confirmar que los 5 archivos existen (`ls`, `ls config`) | [COMPLETAR] |
| 4 | 11/11/2026 | [COMPLETAR] | [COMPLETAR] | Intento de levantar el contenedor con `docker compose up -d --build` y revisión con `docker logs asterisk` | `Error response from daemon: No such container: asterisk` (el contenedor no se creó) | [COMPLETAR: causa real y comando aplicado] | Repetir el despliegue hasta ver el contenedor en `Up` | Captura del error |
| 5 | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | Verificación de la consola de Asterisk (`pjsip show endpoints`, `dialplan show internas`) | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] |
| 6 | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | Prueba de acceso a la VM desde otro equipo (`ping`, puerto 5060, `pjsip set logger on`) | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] |
| 7 | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | Registro de los softphones 1001 y 1002 (`pjsip show contacts`) | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] |
| 8 | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | Llamadas internas 1001 ↔ 1002 y prueba de eco (600) | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] |
| 9 | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | Registro del Prompt Espejo y de las decisiones sobre las sugerencias de la IA | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] |
| 10 | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | Revisión final del repositorio y entrega del enlace en Moodle | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] | [COMPLETAR] |

> Agrega una fila nueva por cada sesión o actividad. Si una actividad se repite por un fallo, registra cada intento por separado.

## Dificultades y soluciones

| N° | Dificultad | Síntoma o mensaje exacto | Causa identificada | Solución aplicada | Cómo se verificó | Estado |
|---|---|---|---|---|---|---|
| 1 | El contenedor no se crea | `Error response from daemon: No such container: asterisk` al ejecutar `docker logs asterisk` | [COMPLETAR: mensaje de `docker compose up`] | [COMPLETAR] | `docker ps` muestra `asterisk` en `Up` | [Resuelta / En curso] |
| 2 | [COMPLETAR] | | | | | |

## Decisiones técnicas tomadas

| Decisión | Alternativa considerada | Motivo | Verificación |
|---|---|---|---|
| Imagen propia sobre Alpine (`Dockerfile`) | Imagen ya hecha de Docker Hub | Encaja con el requisito "Alpine + Docker" y deja el despliegue reproducible | [COMPLETAR] |
| Protocolo PJSIP para las extensiones | `chan_sip` (heredado) | [COMPLETAR: confirmar con la documentación oficial] | [COMPLETAR] |
| `network_mode: host` | Red bridge con puertos publicados | Evitar problemas de NAT con audio SIP/RTP | [COMPLETAR: probar ambas opciones y anotar el resultado] |

## Tareas pendientes

- [ ] Enviar correo al docente con nombres completos y roles de los cinco integrantes
- [ ] Crear el repositorio público y agregar a los colaboradores
- [ ] Resolver el error `No such container` y dejar el contenedor en `Up`
- [ ] Verificar el acceso desde otro equipo (ping, puerto 5060, registro SIP)
- [ ] Registrar al menos dos clientes SIP
- [ ] Realizar y documentar llamadas internas con audio en ambos sentidos
- [ ] Exportar el reporte de ActivityWatch (si está disponible en la estación) e indicar el periodo
- [ ] Completar `06-prompt-espejo.md` y `07-conclusiones.md`
- [ ] Reemplazar contraseñas reales por ejemplos antes de publicar
- [ ] Verificar que el repositorio sea público y esté actualizado, y entregar el enlace en Moodle

## Participación por integrante

| Integrante | Rol | Actividades realizadas (N° de bitácora) |
|---|---|---|
| [COMPLETAR] | Coordinador | |
| [COMPLETAR] | Operador técnico | |
| [COMPLETAR] | Analista e investigador | |
| [COMPLETAR] | Responsable de documentación | |
| [COMPLETAR] | Responsable de continuidad | |
