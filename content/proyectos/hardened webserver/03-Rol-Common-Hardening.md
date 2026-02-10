## 🎯 Objetivo

Establecer la "Línea Base de Seguridad" (Baseline) en el servidor. Un sistema recién instalado tiene servicios innecesarios y parches pendientes que aumentan la superficie de ataque. Este rol reduce ese riesgo desde el minuto uno.

## 🛠️ Tareas y Lógica de Seguridad

### 1. Gestión de Vulnerabilidades (Patch Management)

- **Tarea:** `apt upgrade: dist`.
- **Razón:** No todas las vulnerabilidades se cierran con un `update` simple. El `dist-upgrade` maneja cambios de dependencias críticos para parches del Kernel.
- **Compliance:** Alineado con **CIS Control 7** (Gestión continua de vulnerabilidades).

### 2. Reducción de la Superficie de Ataque

- **Tarea:** Instalación de `security_packages` (UFW, Fail2Ban).
- **Razón:** Un servidor web no necesita herramientas de desarrollo o servicios de impresión. Instalamos solo lo estrictamente necesario para la defensa.
- **Principio:** **Least Functionality** (NIST 800-53).

### 3. Automatización de Parches (MTTP)

- **Tarea:** Configuración de `unattended-upgrades`.
- **Razón:** El **Mean Time to Patch** (Tiempo medio de parcheo) es una métrica crítica. Si se publica un exploit un viernes noche, el servidor se parcheará solo sin esperar al lunes.

---

## 📊 Matriz de Trazabilidad (Compliance)

|**Componente**|**Control CIS**|**Descripción**|
|---|---|---|
|**APT Update**|1.1|Asegura que el software proviene de fuentes íntegras y actualizadas.|
|**Auto-upgrades**|1.2|Implementación de actualizaciones críticas en menos de 24h.|
|**Common Tools**|4.1|Instalación de utilidades de auditoría y red esenciales.|

## 🛠️ Higiene del Tooling (Best Practices)

Para asegurar un despliegue profesional y predecible, se han aplicado correcciones de "deuda técnica" en la configuración:

- **Explicit Python Discovery:** Definición de `ansible_python_interpreter` en el inventario para eliminar avisos de ambigüedad.
- **Fact Discovery Hardening:** Migración a la sintaxis moderna `ansible_facts['os_family']`, garantizando compatibilidad con versiones futuras de Ansible y mayor velocidad de ejecución.

>[!important] Validación de Idempotencia 
>Una de las mayores ventajas de seguridad de Ansible es la **idempotencia**.
>- **Prueba realizada:** Ejecutar el playbook dos veces seguidas. 
>- **Resultado esperado:** En la segunda ejecución, todas las tareas deben marcar `ok` (verde) y ninguna `changed` (amarillo).
>- **Valor de Ciberseguridad:** Esto garantiza que la configuración de seguridad es estable y que no hay "deriva de configuración" (configuration drift), asegurando que el estado del servidor siempre coincide con nuestra política definida.
>![[Pasted image 20260209181448.png]]

> [!info] Nota sobre Arquitectura de Variables
> Se ha migrado de un archivo único `group_vars/all.yml` a un directorio `group_vars/all/`. 
> - **Razón:** Permite separar las variables públicas de los secretos cifrados (Vault), facilitando la mantenibilidad y la seguridad del código. 
> - **Lección Aprendida:** Ansible fusiona automáticamente todos los archivos dentro del directorio del grupo, permitiendo una organización lógica sin perder el alcance global de las variables.


