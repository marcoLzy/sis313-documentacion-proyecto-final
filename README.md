# 🚀 Proyecto Final SIS313: [Título del Proyecto]

> **Asignatura:** SIS313: Infraestructura, Plataformas Tecnológicas y Redes<br>
> **Semestre:** 2/2025<br>
> **Docente:** Ing. Marcelo Quispe Ortega

## 👥 Miembros del Equipo ([Número o denominación del grupo])

| Nombre Completo | Rol en el Proyecto | Contacto (GitHub/Email) |
| :--- | :--- | :--- |
| Marco lopez Yapu |  Administrador de Sistemas | [marcomlz] |
| Luis hernan Huallpa Franses |  Administrador de Sistemas | [luishuf] |
| Rodrigo Caballero Yucra  | Administrador de Sistemas | [rodricy] |


## 🎯 I. Objetivo del Proyecto

Diseñar e implementar una plataforma de mensajería instantánea empresarial basada en protocolo XMPP con arquitectura de alta disponibilidad, replicación bidireccional Master-Master de bases de datos, balanceo de carga automatizado y sistema de monitoreo en tiempo real para la Universidad San Francisco Xavier de Chuquisaca.
Objetivos Específicos:
•	Configurar una infraestructura virtualizada de tres nodos con redundancia en capa de procesamiento
•	Implementar replicación Master-Master bidireccional entre bases de datos MariaDB
•	Establecer balanceo de carga mediante Nginx para distribución inteligente de conexiones
•	Integrar sistema de monitoreo con Prometheus y Grafana para observabilidad completa
•	Desarrollar scripts de automatización para gestión, respaldos y verificación de integridad
•	Garantizar seguridad mediante cifrado TLS/SSL y autenticación SCRAM
•	Lograr disponibilidad superior al 99.5% con recuperación automática ante fallos
## 💡 II. Justificación e Importancia
Este proyecto resuelve la necesidad institucional de contar con una plataforma de comunicación segura, confiable y bajo control administrativo propio, eliminando la dependencia de servicios externos comerciales que presentan limitaciones en privacidad, control de datos y costos recurrentes.

## 🛠️ III. Tecnologías y Conceptos Implementados

### 3.1. Tecnologías Clave
ejabberd: Servidor XMPP empresarial escrito en Erlang/OTP, proporciona mensajería instantánea, presencia, salas grupales, transferencia de archivos y llamadas de voz/video. Implementado en modo redundante en VM2 y VM3.
MariaDB: Sistema de gestión de bases de datos relacional, almacena usuarios, mensajes, historial y configuraciones. Configurado en replicación Master-Master bidireccional para sincronización automática entre VM2 y VM3.
Nginx: Servidor web y proxy inverso de alto rendimiento, actúa como balanceador de carga en VM1 distribuyendo conexiones XMPP entre servidores backend con health checks y failover automático.
Prometheus: Sistema de monitoreo y base de datos de series temporales, recolecta métricas de rendimiento, uso de recursos y disponibilidad de servicios cada 15 segundos desde los tres nodos.
Grafana: Plataforma de visualización y análisis de métricas, proporciona dashboards interactivos en tiempo real mostrando CPU, memoria, disco, red, usuarios conectados y estado de replicación.
Node Exporter: Agente exportador de métricas de sistema instalado en las tres VMs, expone estadísticas de hardware y sistema operativo para consumo por Prometheus.
Bash Scripting: Scripts de automatización desarrollados para gestión de usuarios, respaldos automáticos, verificación de replicación, reinicio de servicios y generación de reportes.
VirtualBox: Plataforma de virtualización utilizada para crear y gestionar las tres máquinas virtuales que componen la infraestructura (VM1, VM2, VM3).



### 3.2. Conceptos de la Asignatura Puestos en Práctica (T1 - T6)

Marca con un ✅ los temas avanzados de la asignatura que fueron implementados:

* **Alta Disponibilidad (T2) y Tolerancia a Fallos:** [Describir cómo: Ej. Replicación DB y uso de Keepalived para failover.]
* **Seguridad y Hardening (T5):** [Describir cómo: Ej. Uso de Firewall (UFW), Hardening SSH, Certificados SSL/TLS.]
* **Automatización y Gestión (T6):** [Describir cómo: Ej. Scripts de Backup (DRP) o Playbooks de Ansible para la configuración.]
* **Balanceo de Carga/Proxy (T3/T4):** [Describir cómo: Ej. Nginx/HAProxy para distribución de tráfico y health checks.]
* **Monitoreo (T4/T1):** [Describir cómo: Ej. Uso de Prometheus/Grafana para métricas en tiempo real.]
* **Networking Avanzado (T3):** [Describir cómo: Ej. Implementación de VLANs o Enrutamiento Estático.]

## 🌐 IV. Diseño de la Infraestructura y Topología

### 4.1. Diseño Esquemático

Incluye un diagrama de la topología final. Muestra claramente la segmentación de red, las IPs utilizadas, y los flujos de tráfico.

> 
| VM/Host | Rol | IP Física | IP Virtual (si aplica) | Red Lógica | SO |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VM-LB-M** | Proxy / Load Balancer MASTER | 192.168.10.2 | 192.168.10.1 (VIP) | Red 10 | Ubuntu 22.04 |
| **VM-DB-M** | Base de Datos (Maestro) | 192.168.20.2 | N/A | Red 20 | Debian 12 |
| **VM-DB-S** | Base de Datos (Esclavo) | 192.168.20.3 | N/A | Red 20 | Debian 12 |
| **VM-MON** | Monitoreo (Grafana/Prometheus) | 192.168.10.3 | N/A | Red 10 | Ubuntu 22.04 |

### 4.2. Estrategia Adoptada (Opcional)

Describe la estrategia de diseño y las decisiones críticas.

* **Estrategia de Replicación:** [Ej. Se optó por la replicación asíncrona de MariaDB debido a la menor latencia, priorizando la separación de lectura/escritura con ProxySQL.]
* **Estrategia de Hardening:** [Ej. Se aplicaron los estándares CIS de hardening mediante un playbook de Ansible para la automatización de la seguridad inicial.]

## 📋 V. Guía de Implementación y Puesta en Marcha

Documenta los pasos esenciales para que cualquier persona pueda replicar el proyecto (instalación, configuración de ficheros clave, comandos).

### 5.1. Pre-requisitos
* [Ej. 4 VMs con Ubuntu 22.04 y acceso root/sudo.]
* [Ej. Repositorio git clonado en cada VM.]

### 5.2. Despliegue (Ejecución de la Automatización)
1.  **Instalación:** Instalar Ansible en la máquina de control.
2.  **Configuración:** Editar el archivo de inventario (`hosts.ini`) con las IPs.
3.  **Ejecución:** Ejecutar el playbook principal: `ansible-playbook setup.yml`.

### 5.3. Ficheros de Configuración Clave
* `/etc/ansible/playbooks/db_cluster.yml`: Playbook para la replicación y ProxySQL.
* `/etc/nginx/sites-available/proxy.conf`: Configuración del Balanceador y Hardening TLS.
* `/etc/keepalived/keepalived.conf`: Configuración del Failover (MASTER/BACKUP).

**Incluir además los archivos de configuración y software a utilizar dentro del proyecto y organizados en carpetas.**

## ⚠️ VI. Pruebas y Validación

| Prueba Realizada | Resultado Esperado | Resultado Obtenido |
| :--- | :--- | :--- |
| Test de Failover de la BD (Apagar Maestro) | El esclavo debe tomar las escrituras o el servicio debe seguir activo. | [OK/FALLIDO] |
| Prueba de Carga/Estrés (Balanceo) | El tráfico se distribuye equitativamente entre los servidores de aplicación. | [OK/FALLIDO] |
| Test de Seguridad (SSL/Firewall) | El acceso HTTP debe redirigirse a HTTPS y el Firewall debe bloquear todos los puertos excepto 443. | [OK/FALLIDO] |

## 📚 VII. Conclusiones y Lecciones Aprendidas

[Resumen de los principales logros y desafíos técnicos superados. ¿Qué harían diferente?]
