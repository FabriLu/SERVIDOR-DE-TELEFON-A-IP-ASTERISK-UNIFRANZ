# ========== 1. Verificar la base (Alpine + Docker) ==========
docker --version
docker compose version
service docker status
ip a                      # anotar la IP de la VM

# ========== 2. Crear el proyecto ==========
mkdir -p ~/asterisk/config
cd ~/asterisk

# ========== 3. Crear los archivos ==========
# Dockerfile
cat > Dockerfile << 'EOF'
FROM alpine:3.20

RUN apk add --no-cache asterisk asterisk-sample-config \
    asterisk-sounds-en asterisk-sounds-moh

CMD ["asterisk", "-f", "-vvv"]
EOF

# config/pjsip.conf (extensiones 1001 y 1002)
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

# config/extensions.conf
cat > config/extensions.conf << 'EOF'
[internas]
exten => _100X,1,NoOp(Llamada a ${EXTEN})
 same => n,Dial(PJSIP/${EXTEN},20)
 same => n,Hangup()

exten => 600,1,Answer()
 same => n,Echo()
 same => n,Hangup()
EOF

# config/rtp.conf
cat > config/rtp.conf << 'EOF'
[general]
rtpstart=10000
rtpend=10100
EOF

# docker-compose.yml
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

# ========== 4. Construir y levantar ==========
docker compose up -d --build
docker ps
docker logs asterisk          # <- el que ejecutaste; dio "No such container"

# ========== 5. Diagnóstico del error (sugerido) ==========
ls
ls config
docker ps -a
docker build -t asterisk-test .
docker compose up -d
docker ps
docker logs asterisk

# ========== 6. Comprobar desde la consola de Asterisk ==========
docker exec -it asterisk asterisk -rvvv
#   dentro: pjsip show endpoints
#           dialplan show internas
#           exit

# ========== 7. Accesibilidad de la VM desde otros equipos ==========
ip a
ip route
netstat -lnup | grep 5060
docker exec -it asterisk asterisk -rx "pjsip show transports"
# En OTRO equipo:
#   ping IP-DE-LA-VM            (Windows: ping IP-DE-LA-VM)
#   ping -c 4 IP-DE-LA-VM       (Linux/Mac)
iptables -L -n                  # solo para revisar si hay reglas

# Log SIP en vivo, mientras el softphone intenta registrarse
docker exec -it asterisk asterisk -rvvv
#   dentro: pjsip set logger on

# ========== 8. Verificar registro y llamadas ==========
docker exec -it asterisk asterisk -rx "pjsip show contacts"
docker exec -it asterisk asterisk -rx "core show channels"   # con llamada activa

# ========== 9. Gestión de la extensión 1001 ==========
docker exec -it asterisk asterisk -rx "pjsip reload"
docker exec -it asterisk asterisk -rx "pjsip show endpoint 1001"
docker exec -it asterisk asterisk -rx "pjsip show auth 1001-auth"
docker restart asterisk         # solo si el reload no toma el cambio
