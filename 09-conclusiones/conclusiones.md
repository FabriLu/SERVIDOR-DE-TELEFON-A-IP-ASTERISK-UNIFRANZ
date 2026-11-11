# Conclusiones

## 1. Estado final
- Contenedor de Asterisk sobre Alpine Linux en Docker: **[funcionando]**
- Extensiones SIP configuradas: **[1001, 1002, ...]**
- Clientes registrados (`pjsip show contacts`): **[cuántos y con qué softphone]**
- Llamadas internas entre extensiones: **[exitosas / con problemas: detallar]**
- Audio en ambos sentidos: **[sí / no]**
- Acceso desde otro equipo de la red: **[comprobado con captura N°__]**

## 2. Cumplimiento de los objetivos
| Objetivo de la misión | Estado | Evidencia |
|---|---|---|
| Configurar extensiones SIP | [cumplido / parcial / pendiente] | [captura o archivo] |
| Registrar al menos dos clientes | [ ] | [ ] |
| Realizar llamadas internas | [ ] | [ ] |
| Demostrar acceso desde otro equipo | [ ] | [ ] |

## 3. Dificultades y soluciones
| Dificultad | Causa real | Solución aplicada |
|---|---|---|
| El contenedor no se creó (`No such container: asterisk` al ejecutar `docker logs`) | [COMPLETAR: lo que mostró `docker compose up`] | [COMPLETAR] |
| [otra dificultad real] | | |

## 4. Aprendizajes técnicos
- Una extensión SIP en PJSIP se compone de tres objetos relacionados: *endpoint*, *auth* y *aor*.
- La señalización (SIP, puerto 5060) y el audio (RTP, rango de puertos configurado) viajan por canales distintos, por eso un cliente puede registrarse y aun así no tener audio.
- Docker aísla la red del contenedor. En VoIP, el NAT entre contenedor, VM y clientes es una fuente frecuente de fallos de audio, y la elección del modo de red tiene consecuencias reales. **[COMPLETAR: qué modo usaron y qué observaron]**
- Para diagnosticar se debe seguir un orden: estado del contenedor, salida del despliegue, logs y consola de Asterisk (`pjsip show endpoints`, `pjsip set logger on`).
- Reproducir el despliegue con archivos (`Dockerfile`, `docker-compose.yml`, configuraciones) permite que otra persona levante el mismo servicio.

## 5. Utilidad en un entorno profesional
Una central telefónica IP interna permite comunicación entre empleados sin costo por llamada interna, centraliza la administración de extensiones y sirve de base para funciones como buzón de voz, colas o troncales SIP hacia proveedores. Con Docker, el servicio puede replicarse o restaurarse rápidamente tras una falla. **[COMPLETAR: un ejemplo concreto que ustedes consideren aplicable]**

## 6. Seguridad
- Las contraseñas de las extensiones del repositorio son de ejemplo; las reales no se publicaron.
- Limitaciones de esta implementación: **[COMPLETAR con honestidad: por ejemplo, sin cifrado SIP/TLS, sin restricción por IP, contraseñas simples, exposición de puertos]**.

## 7. Uso de la IA
Se utilizó una sola conversación de IA con el Prompt Espejo. Las sugerencias aceptadas, modificadas y descartadas, con su justificación, están en `06-prompt-espejo.md`. **[COMPLETAR: un caso concreto donde la IA no acertó o hubo que corregirla]**

## 8. Tareas pendientes y mejoras futuras
- **[COMPLETAR lo que realmente falta]**
- Posibles mejoras: cifrado con TLS/SRTP, buzón de voz, restricción de acceso por IP, copia de seguridad de la configuración, más extensiones o un menú IVR.

## 9. Conclusión general
**[COMPLETAR en 3 o 4 líneas, con sus palabras: qué lograron, qué aprendieron y qué haría diferente la cuadrilla.]**
