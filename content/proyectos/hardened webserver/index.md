---
title: Hardened Web Infrastructure
description: Orquestación de seguridad y hardening automatizado con Ansible.
---
En este proyecto, **he asumido** la responsabilidad de diseñar y ejecutar una estrategia de hardening automatizada. Mi objetivo ha sido transformar un servidor Linux estándar en una fortaleza digital, aplicando el principio de **Defensa en Profundidad**.

## 🚀 Hitos del Proyecto
- **Automatización Total:** **He orquestado** tres niveles de seguridad (Sistema, Red y Aplicación) mediante roles de Ansible.
- **Gestión de Secretos:** **He implementado** cifrado AES-256 para proteger el acceso de gestión.
- **Auditoría Probada:** **He validado** cada control, alcanzando un **Hardening Index de 65** en Lynis.
- **Consolidación de conocimientos**: Gracias a este proyecto personal he consolidado los conocimientos obtenidos principalmente en un curso de [Openwebinars](https://openwebinars.net/cursos/ansible-despliegues/) sobre Ansible con el cual he obtenido esta [[certificaciones|certificación]].

## 📂 Bitácora de Desarrollo
**He organizado** mi documentación para que puedas seguir mi proceso paso a paso:

### Fase 1: Preparación y Arquitectura
- [[01-Configuración-Entorno|Configuración del Entorno]]: Mi setup inicial con WSL2 y Vagrant.
- [[02-Estructura-y-Arquitectura|Estructura del Proyecto]]: El diseño modular de mis roles de Ansible.

### Fase 2: Implementación de Seguridad
- [[03-Rol-Common-Hardening|Línea Base (Common)]]: Parches automáticos y reducción de superficie de ataque.
- [[04-Rol-Security-Hardening|Blindaje de Red (Security)]]: Hardening de SSH y política restrictiva de firewall.
- [[05-Gestion-de-Secretos|Ansible Vault]]: Mi política de "Cero Texto Plano".

### Fase 3: Capa de Aplicación y Resiliencia
- [[06-Rol-Webserver-Hardening|Hardening de Nginx]]: Cabeceras de seguridad y mitigación de huella.
- [[06.1-Troubleshooting-Nginx|Resolución de Incidentes]]: Análisis y corrección del error de conexión (403/Connection Refused).

### Fase 4: Validación Final
- [[07-Auditoría-y-Validación-Técnica|Auditoría Técnica]]: Resultados de Nmap, Curl y métricas de Lynis.

---

[Repositorio de GitHub](https://github.com/utxra/hardened-webserver-infrastructure)