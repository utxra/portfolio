## 🎯 Mi Estrategia de Verificación

Una vez finalizado el despliegue automatizado, **he procedido** a realizar una auditoría técnica exhaustiva. Mi objetivo es garantizar que la "intención de seguridad" definida en mi código se traduce en una "postura de seguridad" real y verificable. No confío únicamente en que el despliegue haya terminado sin errores; **compruebo** cada control implementado.

## 🛠️ Prueba 1: Auditoría de Red (Reconocimiento Externo)

**He realizado** un escaneo completo de los 65,535 puertos TCP del servidor utilizando `nmap`. Esta prueba simula la fase de reconocimiento de un atacante para verificar la efectividad de mi Firewall (UFW) y la ofuscación del servicio SSH.

### Resultados del Escaneo:
```bash
$ nmap -p- -sV 192.168.56.10
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE  SERVICE VERSION
80/tcp   open   http    nginx
443/tcp  closed https
2222/tcp open   ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.13
```
### Mi Análisis Técnico:

- **Superficie de Ataque Mínima:** **He verificado** que 65,532 puertos están en estado `filtered`, lo que confirma que mi política de **Default Deny** en el firewall es hermética.
- **Gestión Segura:** **He validado** que el puerto estándar SSH (22) es invisible, obligando al uso del puerto seguro `2222` definido en mi bóveda de secretos.
- **Estado de Red:** El puerto 443 aparece como `closed` pero visible; esto es correcto, ya que **he abierto** el tráfico en el firewall pero aún no he levantado un servicio escuchando con certificados SSL/TLS.

---
## 🛠️ Prueba 2: Auditoría de Aplicación (Hardening HTTP)

**He inspeccionado** la respuesta del servidor web para confirmar que las directivas de seguridad inyectadas en Nginx están activas y protegiendo al cliente final.

### Análisis de Cabeceras con `curl`:
```bash
$ curl -I http://192.168.56.10
HTTP/1.1 200 OK
Server: nginx
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
```

### 🛡️ Prueba 3: Auditoría de Sistema (Lynis)

Para elevar el estándar de cumplimiento, **he ejecutado** Lynis, una herramienta de auditoría de seguridad para sistemas Linux reconocida en la industria.

- **Métrica Obtenida:** **He alcanzado** un **Hardening Index de 65**.
- **Análisis Técnico:** Este resultado confirma que las medidas aplicadas en el rol `common` (gestión de parches, eliminación de servicios innecesarios y configuración de actualizaciones automáticas) han generado una base robusta.
- **Valor para el Negocio:** Al automatizar esta métrica, **garantizo** que cualquier desviación en la seguridad del servidor pueda ser detectada y corregida en segundos, permitiendo mantener un cumplimiento continuo (_Continuous Compliance_).
![[Pasted image 20260210002247.png]]

### Hallazgos de Seguridad:

- **Mitigación de Fingerprinting:** **He comprobado** que la cabecera `Server` solo muestra `nginx`. He ocultado exitosamente la versión del software, dificultando la búsqueda de exploits específicos para la versión 1.18 o similares.
- **Defensa Activa:** **He confirmado** que las cabeceras de protección contra Clickjacking (X-Frame-Options) y XSS están presentes en la respuesta.
- **Consistencia:** Gracias al uso del parámetro `always` en mi configuración de Nginx, **he garantizado** que estas cabeceras se envíen incluso en códigos de error, manteniendo la protección en todo momento.

---
## 🖼️ Evidencia Visual (Acceso Web)

Como comprobación final, **he accedido** al servidor a través del navegador. El resultado es un acceso limpio y seguro que confirma la correcta interpretación de las políticas por parte del cliente.
![[Pasted image 20260209234425.png]]

---

## 📊 Conclusión de la Auditoría

Tras estas pruebas, **he validado** que la infraestructura es resiliente. **He logrado** reducir la superficie de ataque a nivel de red y aplicar capas de seguridad profunda en la aplicación, cumpliendo con los objetivos de hardening propuestos al inicio del proyecto.