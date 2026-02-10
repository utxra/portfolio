## 📖 Zero Clear-Text Policy

El uso de **Ansible Vault** garantiza la confidencialidad de los datos sensibles. Incluso si el repositorio es interceptado o publicado en GitHub, los secretos permanecen inaccesibles gracias al cifrado **AES-256**. Esto permite una integración segura en flujos de **GitOps** y CI/CD.

## 🤫 Operativa con Secretos (Vault File)

En este proyecto, hemos optado por cifrar archivos completos de variables para mantener una organización limpia dentro de la jerarquía de Ansible.

- **Creación Inicial:** `ansible-vault create group_vars/all/vault.yml`
- **Edición Segura:** `ansible-vault edit group_vars/all/vault.yml`
- **Visualización (Solo lectura):** `ansible-vault view group_vars/all/vault.yml`

> [!key] Integración en Tareas 
> Así es como desacoplamos la seguridad de la lógica. La tarea no conoce el valor, solo llama a la variable protegida:
> ```yaml
> - name: "SSH: Configurar puerto personalizado"
>   lineinfile:
>     path: /etc/ssh/sshd_config
>     regexp: '^#?Port'
>     line: "Port {{ vault_ssh_port }}"
>   notify: restart ssh
> ```
> **Razón de Seguridad:** Aplicamos "Seguridad por Oscuridad" para mitigar el ruido de ataques automatizados y fuerza bruta en el puerto estándar (22).


