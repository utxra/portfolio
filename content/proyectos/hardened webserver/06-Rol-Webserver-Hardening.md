## 🎯 Objetivo del Rol

En esta etapa, **he desplegado** el servidor web Nginx aplicando un enfoque de "Reducción de la Superficie de Ataque". Mi meta no es solo servir contenido, sino asegurar que el servidor no revele información sensible y proteja a los usuarios mediante cabeceras de seguridad activa.

---
## 🛡️ Medidas de Seguridad Implementadas

### 1. Mitigación de Fingerprinting

**He configurado** la directiva `server_tokens off;` para ocultar la versión específica de Nginx en las cabeceras de respuesta y en las páginas de error.

>**Razón técnica:** Al evitar que un atacante identifique la versión exacta, dificulto la búsqueda de vulnerabilidades específicas (CVEs) asociadas a dicha versión, aumentando el coste del reconocimiento inicial.

### 2. Inyección de Cabeceras de Seguridad
**He creado** un archivo de configuración específico (`security_hardening.conf`) para forzar al navegador del cliente a adoptar medidas de protección estrictas:
```yml
- name: "NGINX: Hardening de cabeceras y seguridad"
  copy:
    dest: /etc/nginx/conf.d/security_hardening.conf
    content: |
      # Ocultar versión de Nginx (Mitiga Server Fingerprinting)
      server_tokens off;
      # Protección contra Clickjacking
      add_header X-Frame-Options "SAMEORIGIN";
      # Protección contra XSS
      add_header X-XSS-Protection "1; mode=block";
      # Prevenir Sniffing de tipos MIME
      add_header X-Content-Type-Options "nosniff";
    mode: '0644'
  notify: restart nginx
```

---
#### 🚀 Validación
Una vez ejecutado, la validación se realiza mediante el comando `curl -I http://192.168.56.10`. El objetivo es que las cabeceras mencionadas aparezcan en la respuesta HTTP y que la cabecera `Server` solo diga `nginx` sin números de versión.
Para confirmar que mi automatización ha surtido efecto, **he verificado** la presencia de estas cabeceras mediante peticiones directas al servidor. La ausencia de la versión de Nginx en la cabecera `Server` confirma el éxito del hardening de aplicación.
