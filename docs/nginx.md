# Guía de Despliegue: Zensical y Nginx

## 2. Inicialización de la Documentación con Zensical y uv
Para la redacción técnica se ha utilizado **Zensical**, gestionando las dependencias de Python de forma aislada y rápida mediante `uv`.

* **Inicializar el entorno y añadir Zensical:**
  ```bash
  uv init --app
  uv add --dev zensical
  ```

* **Generar la estructura base:**
  ```bash
  uv run zensical new .
  ```

* **Probar el servidor de documentación en local:**
  ```bash
  uv run zensical serve
  ```
  > **Verificación:** Abrir el navegador en `http://localhost:8000` para comprobar que el entorno de desarrollo de Zensical responde correctamente.

---

## 3. Instalación y Configuración del Servidor Web Nginx
Nginx se encarga de servir de forma eficiente los archivos estáticos generados por la documentación al puerto web estándar (puerto 80).

* **Instalación de Nginx:**
  ```bash
  sudo apt update
  sudo apt install nginx -y
  ```
  > **Verificación:** Comprobar que el servicio está activo con `sudo systemctl status nginx`.

* **Compilación de la documentación estática:**
  ```bash
  uv run zensical build
  ```
  > **Verificación:** Comprobar que se ha generado la carpeta `site/` con los archivos `.html` compilados.

* **Despliegue de los archivos en la ruta raíz de Nginx:**
  ```bash
  sudo rm -rf /var/www/html/*
  sudo cp -r site/* /var/www/html/
  ```
  > **Verificación:** Ejecutar `ls /var/www/html/` y comprobar que aparecen los archivos estáticos de la documentación.

---

## 4. Verificación Final en Producción
* **Reiniciar el servicio de Nginx para asegurar que aplica los cambios:**
  ```bash
  sudo systemctl restart nginx
  ```
  > **Verificación:** Abrir el navegador web e introducir la dirección `http://localhost` (presionando `Ctrl + F5` para evitar la caché). Debe visualizarse la página de inicio personalizada de la documentación.