## 🛡️ Implementación de Controles de Hardening (CIS Benchmark)

Este rol transforma una instalación base en un entorno endurecido siguiendo estándares de la industria.

### 🔐 Endurecimiento de SSH
- **Deshabilitar Root Login:** Minimiza el impacto de un compromiso de credenciales. Los atacantes suelen buscar el usuario `root` por defecto.
- **Key-Based Authentication Only:** Se elimina el vector de ataque de "Fuerza Bruta" por diccionario al deshabilitar contraseñas.
- **Puerto Personalizado:** Reducción drástica del escaneo masivo de bots en internet.

---
### 🧱 Cortafuegos Dinámico (UFW)
- **Política de Denegación por Defecto (Default Deny):** Aplicamos el principio de **Zero Trust** a nivel de red. Solo los flujos de tráfico explícitamente autorizados (SSH Seguro, HTTP, HTTPS) son permitidos.
- **Micro-segmentación:** El servidor solo expone los servicios necesarios para su función como servidor web.

> [!important] Nota sobre Gestión de Secretos
> El puerto de gestión se define mediante la variable `vault_ssh_port`, la cual reside en un archivo cifrado con **AES-256** vía Ansible Vault, garantizando que la configuración sensible no sea expuesta en el control de versiones.