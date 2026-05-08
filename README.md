# 📖 PokeDex - Guía de Creación de Cuenta en Azure

## Descripción del Proyecto

PokeDex es una aplicación web estática desarrollada por PumasLab para Pueblo Paleta Inc. Permite explorar diferentes especies de Pokémon, mostrando información detallada sobre sus características y habilidades.

Este repositorio documenta el proceso de despliegue de la aplicación en la nube pública de Microsoft Azure, siguiendo buenas prácticas de DevOps y seguridad web.

---

## ☁️ Plataforma Elegida: Microsoft Azure

Se utilizó Azure Web Apps con el plan gratuito F1 (Free Tier), que permite alojar aplicaciones web sin costo mensual, ideal para entornos de prueba y desarrollo.

---

## 🛠️ Paso a Paso: Creación de Cuenta Personal en Azure

### Paso 1 — Ingresar al portal de Azure
- Navega a https://azure.microsoft.com/es-es/free/
- Haz clic en el botón **"Empieza gratis"**.

### Paso 2 — Iniciar sesión o crear una cuenta Microsoft
- Ingresa con tu cuenta Microsoft personal (correo Outlook, Hotmail, Gmail, etc.).
- Si no tienes cuenta, créala en el momento.

### Paso 3 — Completar el registro de la cuenta gratuita
- Acepta los términos y condiciones.
- Ingresa tu información personal (nombre, país, número de teléfono).
- Proporciona una **tarjeta de crédito válida** para verificar identidad — no se realizan cobros durante los primeros 30 días.
- Una vez aprobado, recibirás acceso a **$200 USD en créditos gratuitos por 30 días**.

### Paso 4 — Acceder al Portal de Azure
- Ingresa a https://portal.azure.com.
- Inicia sesión con la misma cuenta usada en el paso anterior.
- Verifica que la suscripción activa sea **"Free Trial"** o **"Azure subscription 1"**.

✅ Con esto ya tienes acceso completo a la consola de Azure para crear recursos en la nube.

---

## 📋 Información del Recurso Desplegado

| Campo | Valor |
|---|---|
| Nombre de la aplicación | app-pokedex-web-prod-jp |
| Grupo de recursos | jp-pokedex-prod |
| Región | Central US |
| Sistema operativo | Windows |
| Pila de ejecución | .NET v9.0 |
| Plan de App Service | ASP-jppokedexprod-9ac4 (Free F1) |
| URL pública | https://app-pokedex-web-prod-jp-fbd5gnbagvhjfjgc.centralus-01.azurewebsites.net/ |

---

## 🔒 Seguridad

La aplicación fue escaneada en [securityheaders.com](https://securityheaders.com) obteniendo una calificación de **A**, con los siguientes encabezados HTTP configurados:

- ✅ Strict-Transport-Security
- ✅ X-Frame-Options
- ✅ X-Content-Type-Options
- ✅ Referrer-Policy
- ✅ Permissions-Policy
- ✅ Content-Security-Policy

---

## 🧠 Reflexión Técnica

### ¿Qué vulnerabilidades previenen los encabezados implementados?

Los encabezados de seguridad configurados protegen contra ataques comunes:

- **Content-Security-Policy** previene ataques de Cross-Site Scripting (XSS) al controlar qué recursos puede cargar la aplicación.
- **Strict-Transport-Security** obliga el uso de HTTPS, protegiendo contra ataques de tipo "man-in-the-middle".
- **X-Content-Type-Options: nosniff** evita que el navegador interprete archivos con un tipo MIME diferente al declarado.
- **X-Frame-Options: DENY** protege contra ataques de Clickjacking impidiendo que la app se incruste en iframes externos.
- **Referrer-Policy: no-referrer** minimiza la fuga de información sensible en las cabeceras HTTP.

### ¿Qué aprendí sobre la relación entre despliegue y seguridad web?

El despliegue de una aplicación en la nube no termina cuando la URL es accesible. La seguridad debe ser parte integral del proceso desde el inicio. Configurar correctamente las cabeceras HTTP es una práctica de bajo costo pero de alto impacto que puede marcar la diferencia entre una aplicación vulnerable y una robusta.

### ¿Qué desafíos encontré en el proceso?

- Configurar los encabezados de seguridad en Azure Web Apps requirió modificar el archivo `web.config` directamente desde la consola Kudu.
- Entender la diferencia entre el plan gratuito F1 y los planes de pago fue importante para optimizar costos.
- Subir los archivos mediante la interfaz drag-and-drop de Kudu fue sencillo, pero requiere verificar que todos los archivos se hayan cargado correctamente.

---

## 🔗 Recursos Útiles

- [Portal de Azure](https://portal.azure.com)
- [Documentación de Azure Web Apps](https://learn.microsoft.com/es-es/azure/app-service/)
- [Security Headers Scanner](https://securityheaders.com)
- [Azure Free Trial](https://azure.microsoft.com/es-es/free/)
