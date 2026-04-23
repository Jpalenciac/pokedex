# 🚀 Despliegue.md — Proceso de Despliegue de PokeDex en Azure Web Apps

## Información General

| Campo | Valor |
|---|---|
| Aplicación | PokeDex |
| Plataforma | Microsoft Azure Web Apps |
| Tipo de app | Web estática (HTML, CSS, JavaScript) |
| Sistema operativo | Windows |
| Plan de App Service | Free F1 (Shared Infrastructure) |
| Fecha de despliegue | Abril 2026 |

---

## 🗂️ Paso 1 — Descarga del Código Fuente

1. Ingresar al repositorio de GitHub con el código fuente de la aplicación PokeDex.
2. Hacer clic en **Code > Download ZIP** para descargar el proyecto.
3. Extraer el contenido del ZIP en una carpeta local del equipo.

```
Estructura del proyecto extraído:
pokedex/
├── index.html
├── assets/
│   ├── css/
│   ├── js/
│   └── images/
└── web.config
```

---

## 🗂️ Paso 2 — Creación del Grupo de Recursos en Azure

1. Ingresar al [Portal de Azure](https://portal.azure.com).
2. En la barra de búsqueda superior, escribir **"Grupos de recursos"** y seleccionarlo.
3. Hacer clic en **"+ Crear"**.
4. Completar los campos:
   - **Suscripción:** Azure subscription 1
   - **Nombre del grupo de recursos:** `jp-pokedex-prod`
   - **Región:** (US) Central US
5. Hacer clic en **"Revisar y crear"** y luego en **"Crear"**.

---

## 🗂️ Paso 3 — Creación del Recurso Azure Web App

1. En el Portal de Azure, hacer clic en **"+ Crear un recurso"**.
2. Buscar **"Web App"** y seleccionar el servicio de Microsoft.
3. Hacer clic en **"Crear"**.
4. Completar la configuración con los siguientes valores:

| Campo | Valor configurado |
|---|---|
| Suscripción | Azure subscription 1 |
| Grupo de recursos | `jp-pokedex-prod` |
| Nombre | `app-pokedex-web-prod-jp` |
| Publicar | Código |
| Pila de tiempo de ejecución | .NET 9 (STS) |
| Sistema operativo | Windows |
| Región | Central US |
| Plan de Windows | ASP-jppokedexprod-9ac4 (nuevo) |
| Plan de precios | **Free F1 (Shared Infrastructure)** |

5. Hacer clic en **"Revisar y crear"**.
6. Verificar en el resumen que el SKU aparezca como **"Free"** y el precio estimado sea **$0.00/mes**.
7. Hacer clic en **"Crear"** y esperar entre 1 a 3 minutos mientras Azure aprovisiona el recurso.

> ✅ Al finalizar, Azure muestra el mensaje "Se completó la implementación". Hacer clic en **"Ir al recurso"**.

---

## 🗂️ Paso 4 — Subida del Código con Kudu (Advanced Tools)

### 4.1 — Acceder a Kudu

1. Dentro del recurso `app-pokedex-web-prod-jp`, en el menú lateral izquierdo desplazarse hasta la sección **"Herramientas de desarrollo"**.
2. Hacer clic en **"Herramientas avanzadas"** (Advanced Tools).
3. Hacer clic en el botón **"Ir →"** para abrir Kudu en una nueva pestaña.

### 4.2 — Navegar al directorio wwwroot

1. En la interfaz de Kudu, hacer clic en **"Debug Console" > "CMD"**.
2. En la consola de comandos, ejecutar:

```cmd
cd site/wwwroot
```

3. El prompt cambiará a `C:\home\site\wwwroot>` confirmando la navegación correcta.

### 4.3 — Subir los archivos del proyecto

1. Con la carpeta del proyecto abierta en el explorador de archivos del equipo local, seleccionar **todos los archivos y carpetas** extraídos del ZIP.
2. Arrastrarlos y soltarlos directamente en el **explorador de archivos de Kudu** (panel superior de la interfaz CMD).
3. Esperar a que la carga finalice. Los archivos deben aparecer listados en el directorio `/site/wwwroot/`:

```
/site/wwwroot/
├── assets/
└── index.html
```

---

## 🗂️ Paso 5 — Configuración de Encabezados de Seguridad (web.config)

Para mejorar la calificación de seguridad, se configuraron los encabezados HTTP directamente en el archivo `web.config` ubicado en la raíz del proyecto.

### Contenido del archivo web.config

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="Strict-Transport-Security"
             value="max-age=31536000; includeSubDomains" />
        <add name="X-Frame-Options"
             value="DENY" />
        <add name="X-Content-Type-Options"
             value="nosniff" />
        <add name="Referrer-Policy"
             value="no-referrer" />
        <add name="Permissions-Policy"
             value="geolocation=(), microphone=(), camera=()" />
        <add name="Content-Security-Policy"
             value="default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';" />
      </customHeaders>
    </httpProtocol>
  </system.webServer>
</configuration>
```

Este archivo se subió junto con el resto del proyecto en el **Paso 4**.

---

## 🗂️ Paso 6 — Validación del Despliegue

### 6.1 — Verificar que la app carga correctamente

1. En el Portal de Azure, ir a **"Introducción"** (Overview) del recurso `app-pokedex-web-prod-jp`.
2. Copiar la URL del **"Dominio predeterminado"**:
   ```
   https://app-pokedex-web-prod-jp-fbd5gnbagvhjfjgc.centralus-01.azurewebsites.net/
   ```
3. Abrir la URL en el navegador y verificar que la aplicación PokeDex carga correctamente.
4. Comprobar en la consola del navegador (F12) que no haya errores 404 o 500.

### 6.2 — Escaneo de seguridad en securityheaders.com

1. Ingresar a [https://securityheaders.com/](https://securityheaders.com/).
2. Pegar la URL pública de la aplicación en el campo de búsqueda.
3. Hacer clic en **"Scan"**.

**Resultado obtenido:**

| Campo | Resultado |
|---|---|
| Calificación | **A** |
| Fecha del escaneo | 23 Apr 2026, 05:00:50 UTC |
| IP Address | 168.61.152.29 |

**Encabezados verificados como presentes (✅):**

- ✅ Strict-Transport-Security
- ✅ X-Frame-Options
- ✅ X-Content-Type-Options
- ✅ Referrer-Policy
- ✅ Permissions-Policy
- ✅ Content-Security-Policy

> ⚠️ **Nota:** La calificación quedó limitada a **A** (y no A+) porque Azure Web Apps agrega automáticamente algunas configuraciones de sistema que generan una advertencia en el escaneo. Esto es comportamiento esperado en el plan gratuito.

---

## ⚠️ Errores Encontrados y Soluciones

| Error | Causa | Solución |
|---|---|---|
| App mostraba página de bienvenida de Azure al cargar la URL | Los archivos no se subieron a la ruta correcta `/site/wwwroot/` | Verificar en Kudu que los archivos estén directamente en `wwwroot` y no dentro de una subcarpeta |
| Algunos archivos de imágenes no cargaban (error 404) | La carpeta `assets/images/` no se arrastró completamente | Repetir el drag-and-drop asegurándose de incluir todas las subcarpetas |
| Encabezado `Content-Security-Policy` bloqueaba recursos externos | La política CSP era muy restrictiva | Ajustar el valor en `web.config` para permitir las fuentes y scripts necesarios |

---

## 📊 Resumen de Costos

| Plan | Costo mensual | Recursos |
|---|---|---|
| Free F1 (usado) | **$0.00** | 60 min/día de CPU, 1 GB RAM, 1 GB almacenamiento |
| Basic B1 | ~$54.75/mes | 1 instancia dedicada, 1.75 GB RAM |

> Para proyectos de producción con mayor tráfico, se recomienda migrar a un plan **Standard S1** o superior con escalado automático.

---

## 🔗 Referencias

- [Azure Web Apps Documentation](https://docs.microsoft.com/es-es/azure/app-service/)
- [Kudu Advanced Tools](https://github.com/projectkudu/kudu/wiki)
- [Security Headers Scanner](https://securityheaders.com/)
- [OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/)
