---
title: Laboratorio Automatizado DFIR
---
# Despliegue de Laboratorio Automatizado para el Análisis Forense de Malware

**Descripción:** Diseño y automatización de la arquitectura de contención para un equipo de analistas DFIR, permitiendo la detonación segura de muestras reales (ej. WannaCry) mediante infraestructura como código.

>[!SUCCESS] Impacto del Proyecto 
>**Estandarización y seguridad:** Automatización del despliegue de entornos de análisis idénticos para todo el equipo, garantizando la **contención total** de malware real y eliminando errores de configuración manual.

---

## El Contexto y Mi Rol

Como parte del Trabajo Final de Máster, lideré el **área de Infraestructura y Operaciones**. El equipo necesitaba un entorno donde pudieran analizar malware sin comprometer sus equipos personales ni la red de trabajo.

**Mi responsabilidad:** Diseñar, securizar y distribuir un laboratorio "llave en mano" que fuera idéntico para todos los miembros del equipo, garantizando la **contención total** y la **reversibilidad inmediata**.


> [!abstract] Arquitectura del Entorno
> 
>```mermaid
>graph TD
 >   subgraph Internet
 >       C2[Servidores C2 / Malware Host]
 >   end
>
>    subgraph "Host Machine (Tu PC)"
>        direction TB
>        VBox[VirtualBox Engine]
>        Vagrant[Vagrant Controller]
>    end
>
>    subgraph "Isolated Lab (Windows 10)"
>        direction TB
>        MW[Malware Detonation]
>        Tools[ProcMon / Wireshark]
>        Hardening[Reverse-Hardening Scripts]
>    end
>
>    %% Relaciones
>    Vagrant -->|Despliegue IaC| VBox
>    VBox -->|Crea| Isolated
>    Internet -.->|BLOQUEADO / NAT OFF| MW
  >  MW <--> Tools
>    Hardening -.->|Desactiva| Tools
 >   
 >   style Internet fill:#f96,stroke:#333,stroke-dasharray: 5 5
>    style MW fill:#f66,stroke:#333,stroke-width:2px
>```

## 💻 Stack Tecnológico

Utilicé un enfoque de **IaC** para que el laboratorio fuera reproducible y ligero:
- **Virtualización:** VirtualBox 7 y Vagrant.
- **SO:** Windows 10 Enterprise (Custom Box optimizada).
- **Automatización:** PowerShell para el _Reverse-Hardening_ del sistema.
- **Tooling:** Integración de Sysinternals, Wireshark y PeStudio.

---

## 🔒 Retos Técnicos y Soluciones de Seguridad

He aplicado los mismos **desplegables** que en tus otras notas para facilitar la lectura rápida:

> [!simple]- **Reverse-Hardening (Defensas a Cero)**
> 
> - **Reto:** Evitar que Windows bloquee el malware para observar su comportamiento real.
>     
> - **Solución:** Scripts de PowerShell que desactivan **Microsoft Defender (incluyendo Tamper Protection)** y el Firewall de forma sistemática.
>     

> [!simple]- **Aislamiento de Red "Failsafe"**
> 
> - **Reto:** Prevenir la propagación de ransomware (como WannaCry) a la red local.
>     
> - **Solución:** Configuración estricta de red **host-only** en Vagrant, bloqueando cualquier contacto accidental con servidores C2 externos.
>     

> [!simple]- **Gestión de Estado y Snapshots**
> 
> - **Reto:** Limpiar el entorno rápidamente tras una infección.
>     
> - **Solución:** Sistematización de snapshots que permiten restaurar el laboratorio a un estado virgen en menos de 10 segundos.


---

## 🚀 Impacto y Resultados

- **Colaboración Eficiente:** Los 3 analistas del equipo desplegaron sus laboratorios en minutos, asegurando que todos trabajaban sobre el mismo escenario.

- **Seguridad Probada:** Durante el análisis de **WannaCry**, el diseño de red evitó cualquier intento de propagación lateral.

- **Contribución a la Comunidad:** El laboratorio está publicado y disponible para su uso público.

---

## Media

Aquí puedes ver el laboratorio en funcionamiento y acceder a la Box oficial:

>[!INFO]
>Aquí dejo el enlace de la **Box** del laboratorio actualmente publicado en **HCP** ([HashiCorp Cloud](https://www.hashicorp.com/es)).
>- https://portal.cloud.hashicorp.com/vagrant/discover/capstone/win10-dfir-lab

Demostración del levantamiento del laboratorio con **Vagrant**.
![[vagrant-up-capstone-lab.gif]]

Escritorio del laboratorio con algunas herramientas abiertas.
![[herramientas-capstone-lab.png]]
