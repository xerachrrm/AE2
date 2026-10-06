---
layout: default
title: "Instalación y Despliegue - Blog Jekyll"
---

permalink: /

Despliegue de Web Estática con Jekyll y Nginx

Bienvenido a la documentación técnica del despliegue. Este espacio detalla el proceso completo de instalación, configuración, resolución de incidencias y despliegue del blog personal utilizando Jekyll y el servidor web Nginx en un entorno Linux (Ubuntu).

1. Instalación de Dependencias y Ruby / Jekyll

Para preparar el entorno de Jekyll en el sistema, se instalaron las gemas y dependencias necesarias a nivel de usuario:

Estructura del proyecto
```
jekyll new blog --skip-bundle
cd blog
```

Configuración del entorno de Ruby

Se añadió la ruta de los ejecutables de Ruby al archivo de configuración de la terminal (~/.bashrc) para que los comandos de Bundler y Jekyll estén disponibles globalmente:
```
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
```

2. Resolución de Incidencias en la Compilación

Durante la compilación inicial del sitio, se abordaron y corrigieron los siguientes puntos clave:

Fecha del post por defecto: Se ajustó el Front Matter del archivo de ejemplo dentro de _posts/ para asegurar un formato de fecha válido compatible con la gema de Jekyll:

---
layout: post
title: "¡Mi primer post en el blog!"
date: 2026-10-06 12:00:00 +0200
---


Generación del sitio estático: Compilación de las fuentes Markdown a HTML plano mediante el comando:

```
bundle exec jekyll build
```

Resultado: Se genera el directorio _site/ conteniendo todo el sitio listo para producción (index.html, 404.html, recursos estáticos, etc.).


3. Configuración del Servidor Web Nginx (Puerto 8080)

Para alojar la web estática de Jekyll de forma independiente, se configuró un bloque server dedicado en Nginx escuchando en el puerto 8080.

Creación del archivo de configuración:
```
sudo nano /etc/nginx/sites-available/jekyll-blog
```

Contenido del bloque virtual:
```
server {
    listen 8080;
    server_name localhost;

    root /home/xerach/Documentos/AE2/blog/_site;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Habilitación del sitio y validación:
```
sudo ln -s /etc/nginx/sites-available/jekyll-blog /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

4. Gestión de Permisos de Archivos y Directorios

Dado que Nginx opera bajo el usuario de servicio (www-data) y los archivos del blog residen en el directorio personal (/home/xerach/...), fue necesario otorgar permisos de lectura y ejecución en las rutas intermedias para prevenir errores de acceso (403 Forbidden o 404 Not Found):
```
chmod +rx /home/xerach
chmod +rx /home/xerach/Documentos
chmod +rx /home/xerach/Documentos/AE2
chmod +rx /home/xerach/Documentos/AE2/blog
chmod -R +r /home/xerach/Documentos/AE2/blog/_site
sudo systemctl restart nginx
```

Nota de acceso: Una vez aplicados los permisos, el blog queda accesible de forma local en:
http://localhost:8080

5. Compilación final

Compilación final: Asegúrate de regenerar el sitio estático para reflejar cualquier cambio reciente:
```
bundle exec jekyll build
```
