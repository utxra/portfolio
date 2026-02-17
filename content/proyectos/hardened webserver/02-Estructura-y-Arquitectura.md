## 🏗️ Arquitectura de Directorios

Como base del proyecto, se ha implementado una estructura modular basada en **Roles**, siguiendo las mejores prácticas de Ansible para entornos de producción.

> [!info] Estructura del Proyecto
> ```Plaintext        
>├── ansible.cfg                        # Configuración global y optimización
>├── group_vars/                        # Variables de grupo (configuración de seguridad global)
>│       ├── main.yml
>│       └── vault.yml
>├── inventory/                         # Definición de entornos (Dev, Prod, Lab)
>│   └── hosts.ini
>├── roles                              # Lógica modular y reutilizable
>│   ├── common/                        # Configuración base del OS y parches
>│   │   ├── files
>│   │   │   └── 20auto-upgrades
>│   │   └── tasks
>│   │       └── main.yml
>│   ├── security                       # Hardening (CIS, SSH, UFW, Fail2Ban)
>│   │   ├── handlers
>│   │   │   └── main.yml
>│   │   └── tasks
>│   │       └── main.yml
>│   └── webserver                      # Despliegue de Nginx/Apache endurecido
>│       ├── handlers
>│       │   └── main.yml
>│       ├── tasks
>│       │   └── main.yml
>│       └── templates
>│           └── hardened_site.conf.j2  
>└── site.yml                           # Playbook maestro (Orquestador)
> ```

## 🛡️ Justificación del Diseño: Modularidad y Seguridad

La elección de una estructura basada en roles no es solo organizativa, responde a principios de **DevSecOps**:

1. **Reutilización (DRY - Don't Repeat Yourself):** El rol `security` es agnóstico a la aplicación. Puede aplicarse tanto a este servidor web como a una base de datos o un nodo de Kubernetes en el futuro.

2. **Principio de Responsabilidad Única:** Cada rol se encarga de una capa de la pila tecnológica. Esto facilita las auditorías de seguridad, permitiendo identificar rápidamente dónde se gestiona cada control (ej: el hardening de SSH está estrictamente en `roles/security`).

3. **Gestión de Ciclo de Vida:** Permite actualizar la configuración de seguridad (parches del OS en `common`) sin interferir con la lógica de despliegue de la aplicación en `webserver`.

## 🌐 Gestión de Inventario y Conectividad

Para este laboratorio, el **Control Node** (WSL2) se comunica con el **Target Node** (Vagrant VM) mediante una red privada interna.

- **Estrategia de Conexión:** Uso de llaves SSH con permisos restrictivos (`600`) para garantizar la confidencialidad de la identidad.

> [!NOTE] Contexto Real vs. Lab
> * _En este proyecto:_ Conexión directa vía IP privada `192.168.56.10`.
>  * _En producción:_ Esta comunicación se realizaría a través de un **Bastion Host** (Jump Server) o una **VPN/SD-WAN**, asegurando que los puertos de gestión (SSH) nunca estén expuestos a la red pública.

