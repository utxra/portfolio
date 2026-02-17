## 📖 Zero Clear-Text Policy

El uso de **Ansible Vault** garantiza la confidencialidad de los datos sensibles. Incluso si el repositorio es interceptado o publicado en GitHub, los secretos permanecen inaccesibles gracias al cifrado **AES-256**. Esto permite una integración segura en flujos de **GitOps** y CI/CD.

## 🤫 Operativa con Secretos (Vault File)

En este proyecto, hemos optado por cifrar archivos completos de variables para mantener una organización limpia dentro de la jerarquía de Ansible.

- **Creación Inicial:** `ansible-vault create group_vars/all/vault.yml`
- **Edición Segura:** `ansible-vault edit group_vars/all/vault.yml`
- **Visualización (Solo lectura):** `ansible-vault view group_vars/all/vault.yml`

## La Clave está en el Inventario

En mi caso quería ocultar el puerto que usamos para el servicio SSH en el servidor remoto. Para ello la variable que queremos "esconder" es `ansible_port`. Es la variable predeterminada para el uso del puerto SSH del servidor remoto.

Para ello añadí la variable `ansible_port` al archivo `group_vars/all/vault.yml`, con el comando `ansible-vault create group_vars/all/vault.yml`. 

Y gracias a eso no tengo que hacer referencia a la variable.


Como podemos ver en el desglose de información sobre el nodo `webservers`. La variable está siendo descifrada y entendida a la correctamente, aún sin hacer referencia a ella en el inventario o roles.
```json
{
  "admin_user": "vagrant",
  "ansible_host": "192.168.56.10",
  "ansible_port": 2222,
  "ansible_python_interpreter": "/usr/bin/python3",
  "ansible_ssh_common_args": "-o StrictHostKeyChecking=no",
  "ansible_ssh_private_key_file": "~/.ssh/id_vagrant_portfolio",
  "ansible_user": "vagrant",
  "enable_auto_patches": true,
  "nginx_remove_default_site": true,
  "nginx_user": "www-data",
  "security_packages": [
      "ufw",
        "fail2ban",
      "unattended-upgrades",
        "logwatch",
      "curl",
      "vim"
  ],
  "ssh_port": 22
}
```

Contenidos de archivos de variables
***group_vars/all/vault.yml*** (con el comando `cat)
```yaml
$ANSIBLE_VAULT;1.1;AES256
35386131646539396632656139383031626630383237393435373538353263306365313630626238
3339633136303365396334373133653563323837663163610a666633613061316436643265353365
61623565666232663035373137333032626664383038333232343734326134333464333466613932
6136323532646231630a336632643465643430303362323863323263346437316531346262313765
64626162613361383961653737646432656262303337306432616232623939323766
```

Pero si accedemos con `ansible-vault edit group_vars/all/vault.yml`
```yaml
ansible_port: 2222
```

***group_vars/all/main.yml***
```yaml
---
# Configuración de red y acceso
ssh_port: 22
admin_user: vagrant

# Paquetes esenciales de seguridad
security_packages:
  - ufw
  - fail2ban
  - unattended-upgrades
  - logwatch
  - curl
  - vim

# Configuración de parches automáticos
enable_auto_patches: true

#Variables para Nginx
nginx_user: www-data
nginx_remove_default_site: true
```


> [!INFO] Integración en Tareas 
> Así es como desacoplamos la seguridad de la lógica. La tarea no conoce el valor, solo llama a la variable protegida:
> ```yaml
> - name: "SSH: Configurar puerto personalizado"
>   lineinfile:
>     path: /etc/ssh/sshd_config
>     regexp: '^#?Port'
>     line: "Port {{ ansible_port }}"
>   notify: restart ssh
> ```
> **Razón de Seguridad:** Aplicamos "Seguridad por Oscuridad" para mitigar el ruido de ataques automatizados y fuerza bruta en el puerto estándar (22).

