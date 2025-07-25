# 🛡️ Inventario Maestro de Servidores Linux 🛡️

<div align="center">
  <a href="#📖-acerca-del-proyecto"><strong>Acerca del Proyecto</strong></a> ·
  <a href="#🚀-cómo-empezar"><strong>Cómo Empezar</strong></a> ·
  <a href="#🗺️-roadmap"><strong>Roadmap</strong></a>
</div>

<div align="center">
  <img alt="Versión" src="https://img.shields.io/badge/versi%C3%B3n-1.1-blue.svg?style=for-the-badge">
  <img alt="Licencia" src="https://img.shields.io/badge/licencia-Propietaria-red.svg?style=for-the-badge">
  <img alt="Lenguaje" src="https://img.shields.io/badge/lenguaje-Bash-lightgrey.svg?style=for-the-badge">
  <img alt="Mantenido" src="https://img.shields.io/badge/mantenido-S%C3%AD-green.svg?style=for-the-badge">
</div>

---

## 🧭 Tabla de Contenidos

- [Acerca del Proyecto](#📖-acerca-del-proyecto)
- [Construido Con](#🛠️-construido-con)
- [Características Principales](#✨-características-principales)
- [Cómo Empezar](#🚀-cómo-empezar)
- [Salida y Artefactos](#📂-salida-y-artefactos)
- [Roadmap](#🗺️-roadmap)
- [Licencia](#⚖️-licencia)
- [Autores y Contacto](#🤝-autores-y-contacto)

---

## 📖 Acerca del Proyecto

**Inventario Maestro** es un script de auditoría integral, diseñado para administradores de sistemas e ingenieros DevOps. Su objetivo es unificar docenas de comandos y análisis en una sola ejecución, generando un **informe** exhaustivo sobre la salud, configuración y seguridad de los servidores Linux.

Este proyecto nació de la necesidad de estandarizar las revisiones técnicas, reducir el tiempo de análisis manual y proporcionar entregables claros y profesionales.

---

## 🛠️ Construido Con

- 🐚 **Bash**
- ✍️ **Markdown**
- 🌐 **HTML5**
- 🎨 **css**
- ⚡ **Marked.js**

---

## ✨ Características Principales

- 📄 **Informe Dual**: Genera un archivo `.md` técnico y un `.html` moderno.
- 📝 **Resumen Ejecutivo**: Muestra las alertas y puntos clave al inicio del informe.
- 🛡️ **Análisis de Seguridad**: Auditoría de Firewall, configuración de SSH y estado de SELinux.
- 💎 **Alta Disponibilidad**: Detección de clústeres y configuraciones de replicación.
- 🐘 **Bases de Datos**: Análisis avanzado de PostgreSQL y MongoDB.
- 🚀 **DevOps Ready**: Detección de herramientas como Terraform, Docker, Ansible, etc.
- 🌍 **Geolocalización**: Identificación de la ubicación de IPs backend en Nginx y HAProxy.


## 🚀 Cómo Empezar

### 📋 Prerrequisitos

Para ejecutar el script, asegúrate de tener:

- Acceso `sudo` en el servidor.
- `curl` instalado para las funciones de geolocalización.
- `mongosh` (opcional, pero recomendado para un análisis completo de MongoDB).
- Herramientas comunes de Linux: `grep`, `awk`, `sed`, `ps`, `systemctl`, `ip`, etc.

### ⚡ Ejecución del Script

Para ejecutar el inventario, sigue estos pasos:

1.  **Copiar el Código**: Despliega y copia el código completo del script que se encuentra a continuación.
2.  **Crear el Archivo**: En tu servidor, crea un nuevo archivo.
    ```bash
    nano inventario_maestro.sh
    ```
3.  **Pegar y Guardar**: Pega el código dentro del archivo y guárdalo (en `nano`, `Ctrl+X`, luego `Y`, y `Enter`).
4.  **Dar Permisos**: Asigna permisos de ejecución al archivo.
    ```bash
    chmod +x inventario_maestro.sh
    ```
5.  **Ejecutar**: Lanza el script con privilegios de superusuario.
    ```bash
    sudo ./inventario_maestro.sh
    ```
## 📂 Salida y Artefactos
    Al finalizar, el script creará dos archivos en el directorio actual:
      📄 inventario_completo_[hostname]_[fecha].md: El informe técnico completo en formato Markdown, ideal para control de versiones y análisis en terminal.
      🌐 inventario_completo_[hostname]_[fecha].html: Un informe visual, interactivo y fácil de compartir, perfecto para presentaciones y revisiones gerenciales.

## 🗺️ Roadmap
  ✔ Características Actuales:
  
    ✅ Análisis base del sistema: CPU, RAM, Disco, Red.
    
    ✅ Auditoría de seguridad: Firewall, SSH.
    
    ✅ Detección de herramientas DevOps.
    
    ✅ Análisis avanzado de PostgreSQL y MongoDB.
    
    ✅ Generación de informes en formatos .md y .html.

## ⚖️ Licencia
  Este script es software propietario y confidencial de Mercately. Su uso está restringido al personal y a los sistemas autorizados por la compañía.
  Para más información, contacte al equipo de SRE.

## 🤝 Autor y Contacto
  
 José Ramones (SRE) — Realizado







<details>
<summary><strong>👨‍💻 Haga clic aquí para ver/copiar el código completo del script</strong></summary>

```bash
#!/bin/bash

#================================================================================
# SINOPSIS
#   Realiza un inventario técnico y de DevOps exhaustivo de un servidor Linux.
#
# DESCRIPCIÓN
#   Este script unificado combina múltiples herramientas de análisis para generar
#   un informe completo que cubre desde el hardware y rendimiento hasta la
#   seguridad, configuraciones de alta disponibilidad, backups, bases de datos,
#   y herramientas DevOps. Genera un informe detallado en Markdown (.md) y un
#   atractivo reporte visual en HTML (.html).
#
# USO
#   1. Guardar este contenido en un archivo (ej. inventario_maestro.sh).
#   2. Darle permisos de ejecución: chmod +x inventario_maestro.sh
#   3. Ejecutar el script con sudo: sudo ./inventario_maestro.sh
#
# PROPIEDAD
#   Este script es propiedad de Mercately.
#
# REALIZADO POR
#   Jose Ramones
#
#
# VERSIÓN
#   1.1 
#================================================================================

# --- Verificación de Privilegios ---
if [ "$EUID" -ne 0 ]; then
  echo "❌ Error: Este script debe ser ejecutado con privilegios de superusuario (sudo)."
  exit 1
fi

# --- Configuración Inicial y Nombres de Archivo ---
timestamp=$(date +%Y%m%d_%H%M%S)
host=$(hostname)
md_file="inventario_completo_${host}_${timestamp}.md"
html_file="inventario_completo_${host}_${timestamp}.html"
tmp_md_file=$(mktemp)

# Array para almacenar los hallazgos del resumen ejecutivo
declare -a summary_findings

# --- Inicialización del Reporte Temporal ---
echo "# Inventario Completo del Servidor 🛡️" > "$tmp_md_file"
echo "_Generado el: $(date '+%Y-%m-%d %H:%M:%S')_" >> "$tmp_md_file"
echo "_Host: $host | Usuario: $(whoami)_" >> "$tmp_md_file"
echo "" >> "$tmp_md_file"

# --- Funciones Auxiliares ---
agregar_bloque_documentado() {
    echo "" >> "$tmp_md_file"
    echo "## $1" >> "$tmp_md_file"
    echo "_$2_" >> "$tmp_md_file"
    echo "" >> "$tmp_md_file"
    echo '```' >> "$tmp_md_file"
    bash -c "$3" >> "$tmp_md_file" 2>&1
    echo '```' >> "$tmp_md_file"
    echo "" >> "$tmp_md_file"
}

analizar_log_servicio() {
    local service_name="$1"
    local display_name="$2"
    if systemctl list-units --full -all | grep -q "$service_name.service" && systemctl is-active --quiet "$service_name"; then
        local service_status_log
        service_status_log=$(systemctl status --no-pager --full "$service_name" 2>&1)
        local critical_errors
        critical_errors=$(echo "$service_status_log" | grep -iE 'ALERT|CRITICAL|Failed|Permission denied|Cannot bind')
        if [ -n "$critical_errors" ]; then
            summary_findings+=("ALERTA en ${display_name}: Se encontraron errores o alertas críticas en sus logs. Revisar la sección 'Análisis Detallado de Servicios' en el informe.")
        fi
        echo "" >> "$tmp_md_file"
        echo "### Análisis Detallado de Logs: $display_name" >> "$tmp_md_file"
        echo "_Revisión de los últimos eventos y estado completo del servicio._" >> "$tmp_md_file"
        echo "" >> "$tmp_md_file"
        echo '```' >> "$tmp_md_file"
        echo "$service_status_log" >> "$tmp_md_file"
        echo '```' >> "$tmp_md_file"
        echo "" >> "$tmp_md_file"
    fi
}

#================================================================================
# ⭐ NUEVA FUNCIÓN: ANÁLISIS DE CLÚSTER MONGODB ⭐
#================================================================================
analizar_cluster_mongodb() {
    echo "" >> "$tmp_md_file"
    echo "## Estado del Clúster MongoDB 💎" >> "$tmp_md_file"
    echo "_Salud y miembros del replica set (si existe)._" >> "$tmp_md_file"
    echo "" >> "$tmp_md_file"

    if ! command -v mongosh &> /dev/null; then
        echo "ℹ️ No se pudo verificar el clúster: 'mongosh' no está instalado." >> "$tmp_md_file"
        return
    fi

    local mongo_status_json
    mongo_status_json=$(mongosh --quiet --eval "JSON.stringify(rs.status())" 2>/dev/null)

    if [[ $? -eq 0 && "$mongo_status_json" == *"\"members\":"* ]]; then
        echo "| Miembro (Host:Puerto) | Estado | Salud | Uptime (seg) |" >> "$tmp_md_file"
        echo "|---|---|---|---|" >> "$tmp_md_file"

        local members_data
        members_data=$(echo "$mongo_status_json" | grep -o '"members":\[.*\]' | sed 's/"members":\[//;s/\]}//')

        echo "$members_data" | sed 's/},{/}\n{/g' | while IFS= read -r member_json; do
            local name=$(echo "$member_json" | grep -o '"name":"[^"]*' | cut -d'"' -f4)
            local state=$(echo "$member_json" | grep -o '"stateStr":"[^"]*' | cut -d'"' -f4)
            local health=$(echo "$member_json" | grep -o '"health":[0-9.]*' | cut -d':' -f2)
            local uptime=$(echo "$member_json" | grep -o '"uptime":[0-9]*' | cut -d':' -f2)

            # Añade alerta al resumen ejecutivo si un nodo no está saludable
            if [[ "$health" != "1" ]]; then
                summary_findings+=("CRÍTICO: El miembro del clúster MongoDB '${name}' no está saludable (health: ${health})")
            fi

            echo "| ${name} | ${state} | ${health} | ${uptime} |" >> "$tmp_md_file"
        done
    else
        echo "ℹ️ No se detectó un clúster de MongoDB (replica set) activo." >> "$tmp_md_file"
    fi
    echo "" >> "$tmp_md_file"
}

# ==============================================================================
# INICIO DE LA RECOLECCIÓN DE DATOS
# ==============================================================================
echo "⚙️ Recolectando información del sistema y rendimiento..."
# (El resto de tus funciones de recolección permanecen igual)
agregar_bloque_documentado "Sistema Operativo y Kernel" "Información del sistema y versión de kernel." "uname -a; echo; cat /etc/os-release"
agregar_bloque_documentado "Carga del Sistema y Uptime" "Carga promedio y tiempo de actividad del servidor." "uptime"
agregar_bloque_documentado "CPU" "Detalles del procesador." "lscpu"
agregar_bloque_documentado "Memoria y Uso de Disco" "Uso de RAM y discos montados." "free -h; echo; df -hT"
agregar_bloque_documentado "Dispositivos de Bloque" "Particiones y dispositivos detectados." "lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT"
agregar_bloque_documentado "Configuración de Red 🌐" "Interfaces, IPs y tabla de enrutamiento." "ip addr; echo; ip route"
agregar_bloque_documentado "Top 10 Procesos por CPU" "Procesos que más CPU consumen." "ps -eo pid,user,%cpu,%mem,comm --sort=-%cpu | head -n 11"
agregar_bloque_documentado "Top 10 Procesos por Memoria" "Procesos que más memoria consumen." "ps -eo pid,user,%cpu,%mem,comm --sort=-%mem | head -n 11"
agregar_bloque_documentado "Errores Críticos Recientes del Sistema" "Últimos 15 mensajes de error o superiores en los logs." "journalctl -p 0..3 -n 15 --no-pager"

# --- Análisis de Seguridad ---
echo "🔐 Realizando análisis de seguridad..."
(
    echo ""
    echo "## Análisis de Seguridad 🔐"
    echo "_Revisión de configuraciones de firewall, SSH y privilegios._"
    echo ""
    echo '```'
    # Firewall
    echo "--- Verificando Firewall ---"
    if command -v ufw &>/dev/null; then
        UFW_STATUS=$(ufw status | head -n 1)
        echo "-> UFW Status: $UFW_STATUS"
        if [[ "$UFW_STATUS" != "Status: active" ]]; then
            summary_findings+=("CRÍTICO: El firewall UFW está inactivo.")
        fi
    elif command -v firewall-cmd &>/dev/null; then
        echo "-> Firewalld Status: $(firewall-cmd --state)"
    else
        summary_findings+=("CRÍTICO: No se pudo detectar un software de firewall estándar (UFW, Firewalld).")
    fi
    echo ""
    # SSH
    echo "--- Verificando Configuración de SSH ---"
    SSH_CONF="/etc/ssh/sshd_config"
    if [ -f "$SSH_CONF" ]; then
        ROOT_LOGIN=$(grep -E "^\s*PermitRootLogin" "$SSH_CONF" | awk '{print $2}')
        PASS_AUTH=$(grep -E "^\s*PasswordAuthentication" "$SSH_CONF" | awk '{print $2}')
        echo "-> PermitRootLogin: ${ROOT_LOGIN:-no especificado}"
        echo "-> PasswordAuthentication: ${PASS_AUTH:-no especificado}"
        if [[ "$ROOT_LOGIN" == "yes" ]]; then
            summary_findings+=("ALTO: El login de root por SSH está permitido (PermitRootLogin yes).")
        fi
        if [[ "$PASS_AUTH" == "yes" ]]; then
            summary_findings+=("MEDIO: La autenticación por contraseña en SSH está permitida.")
        fi
    fi
    echo ""
    # Sudo
    echo "--- Usuarios con privilegios Sudo ---"
    SUDO_USERS=$(getent group sudo wheel | cut -d: -f4 | sed 's/,/ /g')
    echo "-> Usuarios en 'sudo' o 'wheel': ${SUDO_USERS:-Ninguno}"
    echo ""
    # SELinux/AppArmor
    echo "--- Verificando SELinux / AppArmor ---"
    if command -v sestatus &>/dev/null; then
        sestatus
    elif command -v aa-status &>/dev/null; then
        aa-status
    else
        echo "-> No se encontró SELinux ni AppArmor."
    fi
    echo '```'
) >> "$tmp_md_file"

# --- Verificación de Software y Herramientas ---
echo "🔎 Verificando software y herramientas DevOps..."
(
    echo ""
    echo "## Lenguajes y Herramientas CLI"
    echo "| Herramienta                | Estado           | Versión"
    echo "|------------------------|----------------|-------------------------------------------------"
    check_command() {
        if command -v "$2" &> /dev/null; then
            local version=$($2 --version 2>&1 | head -n 1)
            printf "| %-22s | %-14s | %s\n" "$1" "Encontrado ✅" "$version"
        else
            printf "| %-22s | %-14s | %s\n" "$1" "No encontrado ❌" "N/A"
        fi
    }
    check_command "Python 3" "python3"
    check_command "Go" "go"
    check_command "Node.js" "node"
    check_command "Ruby" "ruby"
    check_command "Terraform" "terraform"
    check_command "Ansible" "ansible"
    check_command "AWS CLI" "aws"
    check_command "Azure CLI" "az"
    check_command "Google Cloud CLI" "gcloud"
) >> "$tmp_md_file"

# --- Detección de Servicios, Agentes y BBDD (Lista Unificada) ---
echo "📡 Detectando servicios, agentes y bases de datos..."
(
    echo ""
    echo "## Estado de Servicios, Agentes y Bases de Datos"
    echo "| Servicio / Agente        | Estado           |"
    echo "|--------------------------|----------------|"
    declare -A unified_services=(
        ["PostgreSQL"]="postgres" ["MySQL/MariaDB"]="mysqld" ["MongoDB"]="mongod" ["Redis"]="redis-server"
        ["HAProxy"]="haproxy" ["Nginx"]="nginx" ["Apache"]="apache2|httpd"
        ["Keepalived"]="keepalived" ["Pacemaker"]="pacemakerd" ["repmgrd"]="repmgrd"
        ["Datadog Agent"]="datadog-agent" ["New Relic Agent"]="newrelic-infra" ["Zabbix Agent"]="zabbix_agentd"
        ["Prometheus Server"]="prometheus" ["Node Exporter"]="node_exporter" ["Grafana Server"]="grafana-server"
        ["Loki"]="loki" ["Checkmk Agent"]="cmk-agent" ["Nagios NRPE"]="nrpe" ["Dynatrace"]="oneagent"
    )
    for name in "${!unified_services[@]}"; do
        process_pattern="${unified_services[$name]}"
        estado="No detectado ❌"
        if (systemctl is-active --quiet "$name" 2>/dev/null) || pgrep -f "$process_pattern" &>/dev/null; then
            estado="Activo ✅"
        fi
        printf "| %-24s | %-14s |\n" "$name" "$estado"
    done
) >> "$tmp_md_file"

# --- Análisis de Alta Disponibilidad (HA) ---
echo "💎 Analizando configuración de Alta Disponibilidad..."
analizar_cluster_mongodb # ⭐ LLAMADA A LA NUEVA FUNCIÓN DE MONGODB
(
    ha_found=false
    echo ""
    echo "## Análisis de Alta Disponibilidad (Otros) 💎"
    echo "_Revisión de Keepalived, replicación de PG, Pacemaker y repmgr._"
    echo '```'

    # Keepalived
    echo "--- Verificando Keepalived ---"
    if pgrep -f "keepalived" > /dev/null; then
        echo "✅ Servicio Keepalived está en ejecución."
        ha_found=true
    else
        echo "ℹ️ Servicio Keepalived no está en ejecución."
    fi

    # Replicación de PG
    echo ""
    echo "--- Verificando Replicación de PostgreSQL ---"
    if pgrep -f "postgres: wal sender" > /dev/null; then
        echo "✅ Se detectaron procesos de replicación de streaming (Servidor PRIMARIO)."
        ha_found=true
    elif pgrep -f "postgres: wal receiver" > /dev/null; then
        echo "✅ Se detectaron procesos de replicación de streaming (Servidor RÉPLICA)."
        ha_found=true
    else
        echo "ℹ️ No se detectaron procesos de replicación de streaming de PostgreSQL."
    fi

    # Pacemaker
    echo ""
    echo "--- Verificando Pacemaker ---"
    if command -v pcs &> /dev/null && pcs status &>/dev/null; then
        echo "✅ Se detectó un clúster de Pacemaker activo."
        ha_found=true
        echo '```'
        echo "_Estado del clúster de Pacemaker:_"
        pcs status
        echo '```'
    else
        echo "ℹ️ No se detectó un clúster de Pacemaker activo."
    fi

    # Repmgr
    echo ""
    echo "--- Verificando repmgr ---"
    if command -v repmgr &>/dev/null; then
        echo "✅ Herramienta 'repmgr' encontrada."
        ha_found=true
        REPMGR_CONF=$(find /etc/repmgr -name "repmgr.conf" 2>/dev/null | head -n 1)

        if [ -n "$REPMGR_CONF" ]; then
            echo "-> Usando archivo de configuración: $REPMGR_CONF"
            echo "-> Obteniendo estado del clúster:"
            echo ""
            if command -v su &> /dev/null; then
                su - postgres -c "repmgr -f \"$REPMGR_CONF\" cluster show" 2>/dev/null
            else
                sudo -u postgres repmgr -f "$REPMGR_CONF" cluster show 2>/dev/null
            fi
        else
            echo "-> AVISO: No se pudo encontrar el archivo de configuración de repmgr en /etc/repmgr."
        fi
    else
        echo "ℹ️ Herramienta 'repmgr' no encontrada."
    fi

    echo '```'

    if [ "$ha_found" = false ]; then
        summary_findings+=("INFO: No se detectaron tecnologías comunes de Alta Disponibilidad (Keepalived, PG-Repl, etc.).")
    fi
) >> "$tmp_md_file"

# (El resto de tus funciones de análisis permanecen igual)
# --- Análisis de Backups y Recuperación ---
echo "💾 Analizando estrategias de backup..."
(
    backup_found=false
    echo ""
    echo "## Análisis de Backups y Recuperación 💾"
    echo '```'
    # Crontab
    echo "--- Verificando Tareas de Backup en Crontab ---"
    CRON_BACKUPS=$( { crontab -l -u root; crontab -l -u postgres; cat /etc/crontab; } 2>/dev/null | grep -E 'pg_dump|pg_basebackup|rsync|tar|borg|restic' --color=never )
    if [ -n "$CRON_BACKUPS" ]; then
        echo "✅ Tareas de backup encontradas en crontab."
        backup_found=true
    else
        echo "ℹ️ No se encontraron comandos de backup comunes en crontabs."
    fi
    # PITR de PostgreSQL
    echo ""
    echo "--- Verificando Point-in-Time Recovery (PITR) de PostgreSQL ---"
    PG_CONF_PITR=$(find /etc/postgresql /var/lib/pgsql -name "postgresql.conf" 2>/dev/null | head -n 1)
    if [ -n "$PG_CONF_PITR" ]; then
        if grep -q -E "^\s*archive_mode\s*=\s*(on|always)" "$PG_CONF_PITR"; then
            echo "✅ PITR de PostgreSQL: Habilitado (archive_mode=on)."
            backup_found=true
        else
            echo "ℹ️ PITR de PostgreSQL: No habilitado (archive_mode=off)."
        fi
    else
        echo "ℹ️ PITR de PostgreSQL: No se encontró postgresql.conf."
    fi
    echo '```'
    if [ "$backup_found" = false ]; then
        summary_findings+=("ALTO: No se detectó ninguna estrategia de backup obvia (crontab, PITR).")
    fi
) >> "$tmp_md_file"


# --- Geolocalización de Backends ---
echo "🌍 Geolocalizando IPs de backends..."
(
    echo ""
    echo "## Geolocalización de Backends 🌍"
    echo "_Intenta encontrar IPs en configs de Nginx/HAProxy y obtener su ubicación._"
    echo '```'
    backend_ips=()
    if command -v nginx &>/dev/null; then
        nginx_hosts=$(grep -rhE --include='*.conf' 'proxy_pass' /etc/nginx/ 2>/dev/null | grep -v '#' | sed -n 's/.*proxy_pass\s\+http[s]*:\/\/\([^;:/]\+\).*/\1/p' | sort -u)
        for host in $nginx_hosts; do [[ -n "$host" && "$host" != "localhost" ]] && backend_ips+=("$host"); done
    fi
    if [ -f /etc/haproxy/haproxy.cfg ]; then
        haproxy_hosts=$(grep '^\s*server' /etc/haproxy/haproxy.cfg | awk '{print $3}' | cut -d':' -f1 | sort -u)
        for host in $haproxy_hosts; do [[ -n "$host" && "$host" != "localhost" ]] && backend_ips+=("$host"); done
    fi

    unique_ips=$(echo "${backend_ips[@]}" | tr ' ' '\n' | sort -u)
    if [ -z "$unique_ips" ]; then
        echo "No se encontraron IPs de backends para geolocalizar."
    else
        for ip in $unique_ips; do
            echo "--- Resolviendo: $ip ---"
            geo_info=$(curl -s "[https://ipinfo.io/$ip/json](https://ipinfo.io/$ip/json)")
            if echo "$geo_info" | grep -q "bogon"; then
                echo "IP: $ip (IP Privada/Bogon, no se puede geolocalizar)"
            else
                echo "$geo_info" | sed 's/"//g; s/,//; s/    //g; s/{//g; s/}//g'
            fi
            echo ""
        done
    fi
    echo '```'
) >> "$tmp_md_file"


# --- Análisis Detallado de Servicios Clave ---
echo "📜 Analizando logs de servicios clave..."
(
    echo ""
    echo "## Análisis Detallado de Servicios 📜"
    analizar_log_servicio "haproxy" "HAProxy"
    analizar_log_servicio "postgresql" "PostgreSQL"
    analizar_log_servicio "nginx" "Nginx"
    analizar_log_servicio "redis-server" "Redis"
    analizar_log_servicio "mongod" "MongoDB" # ⭐ AÑADIDO ANÁLISIS DE LOGS DE MONGODB
) >> "$tmp_md_file"


# --- Inspección Profunda de PostgreSQL ---
echo "🐘 Revisando configuración de PostgreSQL..."
(
    echo ""
    echo "## Inspección Profunda de PostgreSQL 🐘"
    PG_CONF=$(find /etc/postgresql /var/lib/pgsql -name "postgresql.conf" 2>/dev/null | head -n 1)
    if [ -n "$PG_CONF" ]; then
        echo "### Parámetros clave de postgresql.conf"
        echo "_Ruta: \`$PG_CONF\`_"
        echo '```ini'
        grep -E '^\s*(port|listen_addresses|max_connections|shared_buffers|effective_cache_size|work_mem|wal_level|archive_mode|log_destination|log_min_duration_statement)\s*=' "$PG_CONF"
        echo '```'
        PG_HBA=$(dirname "$PG_CONF")/pg_hba.conf
        if [ -f "$PG_HBA" ]; then
            echo "### Reglas de Autenticación de pg_hba.conf"
            echo "_Ruta: \`$PG_HBA\`_"
            echo '```'
            cat "$PG_HBA" | grep -v '^\s*#' | grep -v '^\s*$'
            echo '```'
            if grep -qE "^\s*host\s+all\s+all\s+0\.0\.0\.0/0\s+trust" "$PG_HBA"; then
                summary_findings+=("CRÍTICO: pg_hba.conf permite acceso 'trust' desde CUALQUIER IP. ¡Riesgo extremo!")
            fi
        fi
    else
        echo "> No se encontró el archivo \`postgresql.conf\` en rutas comunes."
    fi
) >> "$tmp_md_file"

# --- Secciones Finales ---
agregar_bloque_documentado "Servicios Activos (systemd)" "Servicios en estado 'running'." "systemctl list-units --type=service --state=running"
agregar_bloque_documentado "Contenedores Docker" "Contenedores activos si Docker está presente." "if command -v docker &>/dev/null; then docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}'; else echo 'Docker no instalado.'; fi"
agregar_bloque_documentado "Puertos en Escucha (TCP/UDP)" "Puertos abiertos y procesos asociados." "ss -tulnp"
agregar_bloque_documentado "Crontab del Sistema y Usuario 'postgres'" "Tareas programadas." "echo '--- /etc/crontab ---'; cat /etc/crontab; echo; echo '--- crontab -u postgres ---'; crontab -l -u postgres 2>/dev/null || echo 'No encontrado o sin permisos.'"


# ==============================================================================
# ENSAMBLAJE FINAL DEL REPORTE
# ==============================================================================
echo "📝 Ensamblando el reporte final..."
{
    echo "# Inventario Completo del Servidor 🛡️"
    echo "_Generado el: $(date '+%Y-%m-%d %H:%M:%S')_"
    echo "_Host: $host | Usuario: $(whoami)_"
    echo ""
    echo "## Resumen Ejecutivo 📝"
    echo "_Hallazgos clave y advertencias de seguridad._"
    echo ""
    if [ ${#summary_findings[@]} -eq 0 ]; then
        echo "✅ **¡Excelente! No se encontraron problemas críticos o advertencias de seguridad importantes.**"
    else
        echo '```'
        for finding in "${summary_findings[@]}"; do
            echo "- $finding"
        done
        echo '```'
    fi
    echo ""
    echo "---"
} > "$md_file"
cat "$tmp_md_file" >> "$md_file"
rm "$tmp_md_file"

# --- Generación de HTML con Estilo Mejorado ---
echo "📄 Generando reporte HTML..."
cat << EOF > "$html_file"
<!DOCTYPE html>
<html lang="es" data-theme="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inventario Completo: ${host}</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@1/css/pico.min.css">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --background-color: #12121e; --header-bg: #1a1a2e; --card-header-bg: #1f2847;
            --card-body-bg: #161e31; --primary-accent: #39FF14; --text-color: #e0e0e0;
            --muted-text-color: #a7a9be; --border-color: #2a3a5e;
        }
        body { background-color: var(--background-color); color: var(--text-color); font-family: 'Inter', sans-serif; padding: 0; }
        main.container { max-width: 1200px; padding: 1rem 2rem; }
        .page-header { text-align: center; margin-bottom: 3rem; padding: 2.5rem 1rem; background: var(--header-bg); border-bottom: 3px solid var(--primary-accent); border-radius: 0 0 15px 15px; }
        .page-header h1 { color: white; border-bottom: none; font-size: 2.8rem; font-weight: 700; margin: 0; }
        #html-content h2 { background-color: var(--card-header-bg); color: white; font-size: 1.3rem; padding: 0.8rem 1.2rem; margin-bottom: 0; border-radius: 8px 8px 0 0; border-bottom: 2px solid var(--border-color); }
        #html-content pre, #html-content table { background-color: var(--card-body-bg); border: 1px solid var(--border-color); border-top: none; border-radius: 0 0 8px 8px; margin-top: 0; margin-bottom: 2.5rem; box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2); overflow-x: auto; }
        #html-content pre { padding: 1.2rem; white-space: pre-wrap; word-wrap: break-word; }
        #html-content table { width: 100%; }
        .status-pill { padding: 6px 14px; border-radius: 50px; font-weight: 600; font-size: 0.85rem; text-transform: uppercase; letter-spacing: 0.5px; display: inline-block; text-align: center; }
        .status-active { color: #12121e; background-color: var(--primary-accent); box-shadow: 0 0 12px #39FF14, 0 0 4px #fff; }
        .status-inactive { color: white; background-color: #e94560; }
        .status-found { color: white; background-color: #5d5fef; }
        /* ⭐ NUEVOS ESTILOS PARA MONGODB ⭐ */
        .status-primary { color: #12121e; background-color: #ffd700; font-weight: 700; box-shadow: 0 0 12px #ffd700; }
        .status-secondary { color: white; background-color: #5d5fef; }
        .status-healthy { color: #12121e; background-color: var(--primary-accent); }
        .status-unhealthy { color: white; background-color: #e94560; }
        .page-footer { text-align: center; margin-top: 2rem; padding-top: 1rem; border-top: 1px solid var(--border-color); font-size: 0.9em; color: var(--muted-text-color); }
    </style>
</head>
<body>
    <main class="container">
        <header class="page-header">
            <h1>Inventario Completo del Servidor: ${host}</h1>
        </header>
        <div id="html-content"></div>
        <textarea id="markdown-content" style="display: none;">
EOF
cat "$md_file" >> "$html_file"
cat << EOF >> "$html_file"
        </textarea>
        <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
        <script>
            document.addEventListener('DOMContentLoaded', function () {
                const markdownContent = document.getElementById('markdown-content').value;
                const htmlContentDiv = document.getElementById('html-content');
                htmlContentDiv.innerHTML = marked.parse(markdownContent);

                // ⭐ FUNCIÓN DE ESTILOS ACTUALIZADA ⭐
                function applyStatusStyles() {
                    const cells = htmlContentDiv.querySelectorAll('td');
                    cells.forEach(cell => {
                        const text = cell.textContent.trim();
                        // Lógica de estilos existente
                        if (text.includes('Activo ✅')) {
                            cell.innerHTML = '<span class="status-pill status-active">Activo</span>';
                        } else if (text.includes('No detectado ❌')) {
                            cell.innerHTML = '<span class="status-pill status-inactive">No Detectado</span>';
                        } else if (text.includes('No encontrado ❌')) {
                            cell.innerHTML = '<span class="status-pill status-inactive">No Encontrado</span>';
                        } else if (text.includes('Encontrado ✅')) {
                            cell.innerHTML = '<span class="status-pill status-found">Encontrado</span>';
                        }
                        // Nueva lógica para la tabla de MongoDB
                        else if (text === 'PRIMARY') {
                            cell.innerHTML = '<span class="status-pill status-primary">PRIMARY</span>';
                        } else if (text === 'SECONDARY') {
                            cell.innerHTML = '<span class="status-pill status-secondary">SECONDARY</span>';
                        } else if (text === '1' && cell.cellIndex === 2) {
                            cell.innerHTML = '<span class="status-pill status-healthy">Saludable</span>';
                        } else if (text !== '1' && !isNaN(text) && cell.cellIndex === 2) {
                             cell.innerHTML = '<span class="status-pill status-unhealthy">No Saludable</span>';
                        }
                    });
                }
                applyStatusStyles();
            });
        </script>
        <footer class="page-footer">
            <p>Este documento es de uso interno y confidencial.</p>
            <p>Realizado por: José Ramones </p>
        </footer>
    </main>
</body>
</html>
EOF

# --- Mensaje Final ---
echo ""
echo "✅ Inventario completo finalizado con éxito."
echo "📄 Archivo Markdown generado: $md_file"
echo "📄 Archivo HTML generado: $html_file"

exit 0
