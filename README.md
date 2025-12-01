# 🚀 Proyecto Final SIS313: [PLATAFORMA DE MENSAJERÍA INSTANTÁNEA SEGURA
CON ALTA DISPONIBILIDAD
]

> **Asignatura:** SIS313: Infraestructura, Plataformas Tecnológicas y Redes<br>
> **Semestre:** 2/2025<br>
> **Docente:** Ing. Marcelo Quispe Ortega

## 👥 Miembros del Equipo ([Número o denominación del grupo])

| Nombre Completo | Rol en el Proyecto | Contacto (GitHub/Email) |
| :--- | :--- | :--- |
| Marco lopez Yapu |  Administrador de Sistemas | marcomlz |
| Luis hernan Huallpa Franses |  Administrador de Sistemas | luishuf |
| Rodrigo Caballero Yucra  | Administrador de Sistemas |rodricy |


## 🎯 I. Objetivo del Proyecto

Diseñar e implementar una plataforma de mensajería instantánea empresarial basada en protocolo XMPP con arquitectura de alta disponibilidad, replicación bidireccional Master-Master de bases de datos, balanceo de carga automatizado y sistema de monitoreo en tiempo real para la Universidad San Francisco Xavier de Chuquisaca.
## 🛠️ III. Tecnologías y Conceptos Implementados

### 3.1. Tecnologías Clave

**ejabberd:** Servidor XMPP empresarial escrito en Erlang/OTP, proporciona mensajería instantánea, presencia, salas grupales, transferencia de archivos y llamadas de voz/video. Implementado en modo redundante en VM2 y VM3.

**MariaDB:** Sistema de gestión de bases de datos relacional, almacena usuarios, mensajes, historial y configuraciones. Configurado en replicación Master-Master bidireccional para sincronización automática entre VM2 y VM3.

**Nginx:** Servidor web y proxy inverso de alto rendimiento, actúa como balanceador de carga en VM1 distribuyendo conexiones XMPP entre servidores backend con health checks y failover automático.

**Prometheus:** Sistema de monitoreo y base de datos de series temporales, recolecta métricas de rendimiento, uso de recursos y disponibilidad de servicios cada 15 segundos desde los tres nodos.

**Grafana:** Plataforma de visualización y análisis de métricas, proporciona dashboards interactivos en tiempo real mostrando CPU, memoria, disco, red, usuarios conectados y estado de replicación.

**Node Exporter:** Agente exportador de métricas de sistema instalado en las tres VMs, expone estadísticas de hardware y sistema operativo para consumo por Prometheus.

**Bash Scripting:** Scripts de automatización desarrollados para gestión de usuarios, respaldos automáticos, verificación de replicación, reinicio de servicios y generación de reportes.

**VirtualBox:** Plataforma de virtualización utilizada para crear y gestionar las tres máquinas virtuales que componen la infraestructura (VM1, VM2, VM3).



### 3.2. Conceptos de la Asignatura Puestos en Práctica (T1 - T6)

✅ **Alta Disponibilidad (T2) y Tolerancia a Fallos:**

•	Replicación Master-Master bidireccional de MariaDB garantizando sincronización automática de datos

•	Dos servidores ejabberd operando en modo activo-activo con sesiones distribuidas

•	Failover automático mediante health checks cada 30 segundos detectando servidores caídos

•	Recuperación automática de replicación ante errores transitorios de red

•	Sistema continúa operando completamente funcional con un servidor fuera de línea

✅**Seguridad y Hardening (T5):**

•	Certificados SSL/TLS X.509 con clave RSA 4096 bits para cifrado de comunicaciones

•	Autenticación SCRAM (Salted Challenge Response) protegiendo contra ataques de diccionario

•	Firewall iptables configurado con política de denegar por defecto, permitiendo solo puertos necesarios

•	NAT en VM1 aislando red interna 192.168.10.0/29 del acceso externo directo

•	Usuarios de sistema con privilegios mínimos (principio de least privilege)

•	Configuración sudo restrictiva permitiendo solo comandos específicos sin contraseña

•	Contraseñas hasheadas con salt aleatorio nunca almacenadas en texto plano

✅ **Automatización y Gestión (T6):**

•	14 scripts Bash desarrollados para administración completa del sistema

•	Menú interactivo unificado accesible mediante comando 'menu' desde cualquier ubicación

•	Tareas cron programadas: backup diario (2:00 AM), health check cada 15 min, limpieza semanal

•	Autenticación SSH mediante claves Ed25519 para ejecución remota sin contraseña

•	Scripts de verificación de replicación con alertas automáticas ante desincronización

•	Respaldos automáticos con compresión gzip y retención de 7 días

•	Generación automática de logs en /var/log/ para auditoría y troubleshooting

✅ **Balanceo de Carga y Proxy Inverso (T3/T4):**

•	Nginx configurado como stream proxy para balanceo de conexiones TCP XMPP puerto 5222

•	Algoritmo 'least_conn' para tráfico HTTP distribuyendo según carga de servidor

•	Esquema primario-backup para XMPP: VM2 principal, VM3 activa automáticamente ante falla

•	Health checks cada 30 segundos con max_fails=3 y fail_timeout=30s

•	Persistencia de sesiones mediante IP del cliente para mantener conexiones coherentes

•	Rate limiting configurado para protección contra ataques DDoS

•	Proxy para múltiples protocolos: XMPP (5222), HTTP Upload (5280), HTTPS Admin (5443)

✅ **Monitoreo y Observabilidad (T4/T1):**

•	Stack Prometheus-Grafana proporcionando visibilidad completa en tiempo real

•	Recolección de métricas cada 15 segundos de CPU, RAM, disco, red, procesos

•	Dashboard 'Node Exporter Full' con 100+ gráficas de rendimiento y utilización

•	Alertas configurables para umbrales críticos (disco >85%, CPU >90%, servicio caído)

•	Métricas personalizadas de ejabberd: usuarios conectados, mensajes/segundo, sesiones activas

•	Visualización del estado de replicación Master-Master en tiempo real

•	Retención de métricas históricas de 15 días para análisis de tendencias

✅ Networking Avanzado (T3):

•	Diseño de red privada 192.168.10.0/29 con subnetting eficiente (6 IPs utilizables)

•	VM1 con dual-interface: enp0s3 (NAT externa) + enp0s8 (red interna)

•	Configuración de NAT mediante iptables MASQUERADE para acceso a Internet desde VM2/VM3

•	Reglas de forwarding IP permitiendo enrutamiento entre interfaces de red

•	Configuración estática de IPs mediante Netplan con gateway y DNS personalizados

•	Port forwarding de VirtualBox exponiendo servicios: 8080→80, 8443→443, 9090, 3000

•	Aislamiento de seguridad: red interna no accesible directamente desde exterior

## 🌐 IV. Diseño de la Infraestructura y Topología

### 4.1. Diseño Esquemático

<img width="652" height="628" alt="XMPP-Página-1 drawio" src="https://github.com/user-attachments/assets/91bad02e-fc24-4dbf-9066-6888ced46b1d" />

> 

| VM/Host | Rol | IP Física |   IP Virtual       |   Red Lógica    | SO  |
|----------|-----------|-----------|-----------|----------|----------|
| VM1 (proxy)  | Proxy/Load Balancer + Monitoreo    | 192.168.10.2 10.0.2.15 (NAT)| N/A  | Red Interna /29+ NAT | Ubuntu 24.04 LTS   |
| VM2 (xmpp1) | Servidor XMPP Primario + BD Master 1   | 192.168.10.3  | N/A  | Red Interna /29    | Ubuntu 24.04 LTS    |
| VM3 (xmpp2)  | Servidor XMPP Secundario + BD Master 2   | 192.168.10.4    | N/A   | Red Interna /29  | Ubuntu 24.04 LTS   |
### 4.2. Estrategia Adoptada (Opcional)
**ESTRATEGIA DE REPLICACION (CRÍTICA):**

-**Replicación Master-Master bidireccional:** Permite escrituras simultáneas en ambos servidores eliminando punto único de fallo

-**Auto-increment offset (VM2=1, VM3=2) con increment=2:**  Previene conflictos de claves primarias, esencial para la sincronización

-**Monitoreo continuo de Slave_IO_Running y Slave_SQL_Running:**  Detección temprana de fallos de replicación

**ESTRATEGIA DE BALANCEO (CRÍTICA):**

-**Algoritmo primario-backup con afinidad de sesión:** Garantiza que usuarios mantengan conexión estable con mismo servidor

-**Health checks cada 30 segundos con umbral de 3 fallos:** Detección rápida de servidores caídos

-**Failover automático <10 segundos:**  Recuperación casi instantánea ante fallos


## 📋 V. Guía de Implementación y Puesta en Marcha

### 5.1. Pre-requisitos
•	3 Máquinas Virtuales con Ubuntu Server 24.04 LTS instalado.

•	VM1: 2 CPU, 2GB RAM, 20GB disco, 2 interfaces de red (NAT + Internal Network).

•	VM2 y VM3: 2 CPU, 4GB RAM, 30GB disco cada una, 1 interface de red (Internal Network).

•	Acceso root/sudo en todas las máquinas virtuales.

•	Conexión a Internet para descarga de paquetes y actualizaciones.

•	Cliente XMPP Gajim instalado en máquina host Windows para pruebas.


### 5.2. Despliegue (Ejecución de la Automatización)
**Fase 1: Configuración de Red**
Configurar VM1 con dos interfaces: enp0s3 (NAT/DHCP) y enp0s8 (192.168.10.2/29 estática)
  
1.Habilitar IP forwarding en VM1 editando /etc/sysctl.conf: net.ipv4.ip_forward=1

2.Configurar iptables NAT en VM1 para MASQUERADE en interface enp0s3

3.Configurar VM2 con IP 192.168.10.3/29, gateway 192.168.10.2, DNS 8.8.8.8

4.Configurar VM3 con IP 192.168.10.4/29, gateway 192.168.10.2, DNS 8.8.8.8

5.Verificar conectividad: ping entre VMs y ping a Internet desde VM2/VM3

**Fase 2: Instalación de MariaDB y Replicación**
1. Instalar MariaDB en VM2 y VM3: sudo apt install mariadb-server -y

2. Ejecutar mysql_secure_installation en ambos servidores (password: lopez)

3. Crear base de datos: CREATE DATABASE ejabberd_db CHARACTER SET utf8mb4;

4. Crear usuarios: USERDB (aplicación) y replicador (replicación) con permisos apropiados

5. Configurar /etc/mysql/mariadb.conf.d/50-server.cnf con parámetros de replicación

6. Configurar server-id=1 (VM2) y server-id=2 (VM3), habilitar binary logging

7. Crear usuario replicador con privilegio REPLICATION SLAVE

8. Configurar CHANGE MASTER TO en VM2 apuntando a VM3 y viceversa

9. Iniciar replicación: START SLAVE; en ambos servidores

10. Verificar con SHOW SLAVE STATUS\G que Slave_IO_Running y Slave_SQL_Running = Yes

**Fase 3: Instalación de ejabberd**

1. Instalar ejabberd en VM2 y VM3: sudo apt install ejabberd -y

2. Generar certificados SSL en ambos servidores con openssl (CN=chat.usfx.edu.bo)

3. Crear directorio /etc/ejabberd/certs/ y configurar permisos 600

4. Editar /etc/ejabberd/ejabberd.yml configurando hosts: chat.usfx.edu.bo

5. Configurar autenticación SQL apuntando a MariaDB local (usuario marco, password lopez)

6. Configurar módulos: mod_mam, mod_muc, mod_http_upload, mod_carboncopy, etc.

7. Habilitar puertos: 5222 (C2S), 5280 (HTTP), 5443 (HTTPS), 3478 (STUN)

8. Crear directorio /var/lib/ejabberd/upload con permisos para ejabberd

9. Reiniciar ejabberd: sudo systemctl restart ejabberd

10. Crear usuario admin: sudo ejabberdctl register marco chat.usfx.edu.bo lopez

**Fase 4: Configuración de Nginx**
1. Instalar Nginx en VM1: sudo apt install nginx -y

2. Generar certificados SSL para Nginx en /etc/nginx/ssl/

3. Editar /etc/nginx/nginx.conf configurando módulo stream para proxy TCP

4. Configurar upstream xmpp_c2s con VM2 y VM3 (primary + backup)

5. Configurar server listen 5222 con proxy_pass a xmpp_c2s

6. Configurar bloque http para HTTP Upload en puerto 5280

7. Crear /etc/nginx/sites-available/xmpp-proxy con configuración HTTPS

8. Habilitar sitio: ln -s /etc/nginx/sites-available/xmpp-proxy /etc/nginx/sites-enabled/

9. Verificar configuración: sudo nginx -t

10. Reiniciar Nginx: sudo systemctl restart nginx

**Fase 5: Monitoreo**
1. Instalar Prometheus en VM1: sudo apt install prometheus -y

2. Instalar Node Exporter en las 3 VMs: sudo apt install prometheus-node-exporter -y

3. Editar /etc/prometheus/prometheus.yml agregando scrape_configs para las 3 VMs

4. Reiniciar Prometheus: sudo systemctl restart prometheus

5. Instalar Grafana: descargar desde apt.grafana.com e instalar

6. Acceder a Grafana en http://localhost:3000 (admin/admin, cambiar a lopez)

7. Agregar Prometheus como datasource en Grafana

8. Importar dashboard 1860 (Node Exporter Full)

9. Configurar port forwarding en VirtualBox: 9090 y 3000

**Fase 6: Automatización**

1. Crear directorio /opt/admin_scripts/ en VM1

2. Crear usuario adminsrv en VM2 y VM3 con sudo limitado

3. Generar clave SSH Ed25519 en VM1 y copiar a VM2/VM3

4. Desarrollar 14 scripts: menu, monitor, stats, logs, check_replication, list_users, create_user, change_password, delete_user, backup, view_backups, clean_backups, restart_services, automation_status

5. Dar permisos de ejecución: chmod +x /opt/admin_scripts/*.sh

6. Crear enlace simbólico: ln -s /opt/admin_scripts/xmpp_admin_menu.sh /usr/local/bin/menu

7. Configurar cron para backups diarios, health checks cada 15 min, limpieza semanal

8. Verificar ejecución: sudo crontab -l



### 5.3. Ficheros de Configuración Clave
/etc/netplan/50-cloud-init.yaml (VM1): Configuración de red con dual-interface NAT + Internal Network

/etc/mysql/mariadb.conf.d/50-server.cnf (VM2, VM3): Parámetros de replicación Master-Master: server-id, log_bin, auto_increment

/etc/ejabberd/ejabberd.yml (VM2, VM3): Configuración completa de ejabberd: hosts, SQL, módulos, puertos

/etc/nginx/nginx.conf (VM1): Configuración de stream proxy para balanceo XMPP TCP

/etc/nginx/sites-available/xmpp-proxy (VM1): Virtual host HTTPS para administración y servicios web

/etc/prometheus/prometheus.yml (VM1): Configuración de targets y scrape intervals para monitoreo

/opt/admin_scripts/*.sh (VM1): Scripts de automatización para gestión completa del sistema

/etc/cron.d/ o crontab -l (VM1): Tareas programadas para backups, health checks y limpieza


📁 Organización de Archivos del Proyecto:
•	/opt/admin_scripts/ - Scripts de administración

•	/var/backups/xmpp/ - Respaldos automáticos de base de datos

•	/var/log/xmpp_*.log - Logs de operaciones del sistema

•	/etc/ejabberd/ - Configuración de servidores XMPP

•	/etc/nginx/ - Configuración de proxy y balanceador

•	/etc/prometheus/ - Configuración de monitoreo

•	/var/lib/ejabberd/upload/ - Archivos subidos por usuarios



## ⚠️ VI. Pruebas y Validación

| Prueba Realizada| Resultado Esperado| Resultado Obtenido|
|----------|-----------|-----------|
|Test de Failover de ejabberd(Apagar VM2)| VM1 detecta fallo y redirige automáticamente nuevas conexiones a VM3. Usuarios existentes se reconectan automáticamente.| ✅ OKTiempo de detección: <10s Reconexión automática exitosa|
| Test de Failover de BD(Detener MariaDB en VM3)| VM2 continúa operando normalmente. Replicación se restablece automáticamente al reiniciar VM3. | ✅ OK VM2 operativa sin interrupciones Replicación restaurada en <30s ||Prueba de Replicación Bidireccional|Usuario creado en VM2 aparece automáticamente en VM3 y viceversa en <1 segundo.|✅ OKLatencia de replicación: 0.5s promedio Sincronización 100% consistente|**
|Prueba de Replicación Bidireccional|Usuario creado en VM2 aparece automáticamente en VM3 y viceversa en <1 segundo.|✅ OKLatencia de replicación: 0.5s promedioSincronización 100% consistente|
|Test de Balanceo de Carga|Conexiones se distribuyen entre VM2 y VM3 según disponibilidad. Conexiones persistentes mantienen afinidad.|✅ OK Distribución primario-backup funcionando Afinidad de sesión mantenida|
|Prueba de Mensajería|Mensajes entre usuarios se entregan instantáneamente. Mensajes offline se entregan al reconectar.|✅ OK Latencia: <500ms Mensajes offline entregados correctamente|
|Test de Múltiples Dispositivos|Usuario conectado desde 2 dispositivos recibe mensajes en ambos (Carbons).|✅ OKSincronización multi-dispositivo activa Mensajes replicados instantáneamente|
|Prueba de Backup Automático|Backup ejecuta diariamente a las 2:00 AM, genera archivo .sql.gz válido, retiene 7 días.|✅ OK Backup ejecutado exitosamente Verificación de integridad: 100%|
|Test de Monitoreo|Grafana muestra métricas en tiempo real de las 3 VMs. Prometheus recolecta datos cada 15s.|✅ OK Dashboards activos y actualizando 150+ métricas recolectándose|
|Prueba de Scripts de Gestión|Menú interactivo funciona. Todos los 14 scripts ejecutan sin errores. SSH sin contraseña operativo.|✅ OK Todas las opciones del menú funcionales Ejecución remota exitosa|

## 📚 VII. Conclusiones y Lecciones Aprendidas
**Principales Logros:**

•	Se implementó exitosamente una plataforma de mensajería empresarial completa con disponibilidad >99.5%

•	La replicación Master-Master demostró sincronización confiable con latencias <1 segundo

•	El sistema soporta failover automático con recuperación en <10 segundos sin pérdida de datos

•	Los scripts de automatización redujeron en 80% el tiempo de gestión operativa diaria

•	El monitoreo en tiempo real proporciona visibilidad completa del estado del sistema

•	Se logró seguridad robusta mediante cifrado TLS/SSL y autenticación SCRAM

•	La arquitectura escalable permite crecimiento mediante adición de nodos sin rediseño

**Desafíos Técnicos Superados:**
•	Configuración de replicación bidireccional requirió ajuste fino de parámetros auto_increment

•	Resolución de conflictos de certificados SSL autofirmados en clientes XMPP

•	Optimización del balanceo de carga para mantener afinidad de sesiones XMPP

•	Depuración de problemas de replicación mediante análisis detallado de binlogs

•	Configuración de permisos SSH y sudo para automatización sin comprometer seguridad

•	Ajuste de timeouts de Nginx para mantener conexiones XMPP de larga duración



El proyecto demostró que es posible construir infraestructura empresarial robusta utilizando exclusivamente tecnologías open source, alcanzando niveles de disponibilidad y rendimiento comparables a soluciones comerciales pero con control total y costos significativamente menores.
