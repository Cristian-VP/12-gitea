

**Práctica 12\. Despliegue de un servidor Git privado con Gitea** 

Lee detenidamente cada uno de los puntos antes de realizar las tareas solicitadas. Revisa los recursos incluidos. 

Esta práctica no consiste únicamente en usar comandos básicos de Git. El objetivo es desplegar y administrar una plataforma Git propia, similar a un GitHub/GitLab privado, usando **Gitea** y **Docker**. 

**Objetivo de la práctica** 

• Instalar y configurar un servicio de control de versiones self-hosted mediante Docker. 

• Crear usuarios, repositorios privados y permisos básicos en Gitea. 

• Sincronizar un repositorio local con el servidor Git desplegado. 

• Trabajar con ramas, commits, pull requests y merge desde una plataforma web. • Documentar la instalación, configuración, uso y medidas básicas de seguridad aplicadas. 

**1\. Preparación del servidor y comprobaciones iniciales (1,5P)** 

Conéctate por SSH al Ubuntu Server utilizado en prácticas anteriores o crea una VM Ubuntu Server nueva. 

Realiza las siguientes tareas: 

• Actualiza el sistema y comprueba que tienes conectividad a Internet. • Instala Docker y verifica que el servicio queda activo y habilitado en el arranque. • Comprueba que Docker Compose está disponible mediante el comando docker compose version. 

• Habilita en el firewall el acceso por SSH y los puertos que usará Gitea: 3000/TCP para la interfaz web y 2222/TCP para SSH Git. 

• Aporta capturas claras del update, instalación/verificación de Docker, docker compose version y estado del firewall. 

1 / 8  
Comandos orientativos: 

sudo apt update && sudo apt upgrade \-y 

sudo apt install docker.io \-y 

sudo systemctl enable \--now docker 

docker \--version 

docker compose version 

sudo ufw allow ssh 

sudo ufw allow 3000/tcp 

sudo ufw allow 2222/tcp 

sudo ufw enable 

sudo ufw status verbose 

**2\. Despliegue de Gitea con Docker Compose (2P)** Crea un directorio de trabajo para la práctica, por ejemplo /opt/gitea o \~/gitea. Dentro del directorio, crea un archivo docker-compose.yml para desplegar Gitea. El contenedor debe cumplir lo siguiente: 

• Exponer la interfaz web de Gitea en el puerto 3000\. 

• Exponer el acceso SSH de Git en el puerto 2222\. 

• Usar un volumen persistente para que los datos de Gitea no se pierdan al reiniciar el contenedor. 

• Arrancar automáticamente salvo que se detenga manualmente. 

Ejemplo orientativo de docker-compose.yml: 

services: 

gitea: 

image: gitea/gitea:latest 

container\_name: gitea 

restart: unless-stopped 

ports: 

\-"3000:3000" 

\-"2222:22" 

volumes: 

\- ./gitea:/data 

Arranca el servicio y verifica su estado: 

mkdir \-p \~/gitea 

cd \~/gitea 

nano docker-compose.yml 

docker compose up \-d 

2 / 8  
docker compose ps 

docker compose logs \--tail=50 

Accede desde el navegador a: 

http://IP\_DEL\_SERVIDOR:3000 

Se deben aportar capturas de: 

• Archivo docker-compose.yml. 

• Ejecución de docker compose up \-d. 

• Resultado de docker compose ps. 

• Logs del contenedor. 

• Pantalla inicial de instalación de Gitea en el navegador. 

**3\. Configuración inicial de Gitea (1,5P)** 

Completa la instalación inicial desde la interfaz web de Gitea. 

Para simplificar, se puede usar SQLite como base de datos, salvo que el profesor indique otra cosa. 

Tareas solicitadas: 

• Comprobar que la URL base de Gitea apunta a la IP correcta del servidor y al puerto 3000\. 

• Crear un usuario administrador. 

• Crear un usuario normal de trabajo. 

• Crear un repositorio privado llamado practica-gitea. 

• Comprobar que el repositorio no es público. 

Se deben aportar capturas de: 

• Pantalla de configuración inicial. 

• Usuario administrador creado. 

• Usuario normal creado. 

• Repositorio privado creado. 

**Importante:** no se deben incluir contraseñas reales en la memoria. Si aparecen en una captura, deben ocultarse. 

**4\. Sincronización de un repositorio local con Gitea (2P)** En tu equipo o en la propia VM, clona el repositorio creado desde Gitea.   
Crea un proyecto web mínimo o reutiliza un proyecto sencillo de otra asignatura. Debe incluir al menos: 

3 / 8  
• index.html 

• style.css 

• README.md 

Realiza un primer commit y súbelo al repositorio de Gitea. 

Comandos orientativos: 

git clone http://IP\_DEL\_SERVIDOR:3000/usuario/practica-gitea.git 

cd practica-gitea 

echo "\<h1\>Práctica Gitea\</h1\>" \> index.html 

echo "body { font-family: sans-serif; }" \> style.css 

nano README.md 

git status 

git add . 

git commit \-m "Primer commit de la práctica" 

git push 

git log \--oneline 

git remote \-v 

El archivo README.md debe contener, como mínimo: 

• Descripción del proyecto. 

• Pasos realizados para desplegar Gitea. 

• Comandos Git utilizados. 

• URL del repositorio. 

• Explicación de los puertos utilizados. 

• Medidas básicas de seguridad aplicadas. 

Se deben aportar capturas de: 

• git status. 

• git log \--oneline. 

• git remote \-v. 

• git push. 

• Repositorio en Gitea mostrando los archivos subidos. 

**5\. Ramas, Pull Request y Merge (1,5P)** 

Crea una rama nueva llamada mejora-readme. 

En esa rama, modifica el archivo README.md añadiendo un apartado llamado: Seguridad básica aplicada 

Comandos orientativos: 

4 / 8  
git checkout \-b mejora-readme 

nano README.md 

git add README.md 

git commit \-m "Amplía documentación de seguridad" 

git push \-u origin mejora-readme 

Desde la interfaz web de Gitea: 

• Crea un Pull Request desde mejora-readme hacia main o master. 

• Revisa los cambios. 

• Realiza el merge desde la interfaz web. 

• Comprueba en el historial que aparecen los commits correspondientes. Se deben aportar capturas de: 

• Rama creada. 

• Pull Request abierto. 

• Comparación de cambios. 

• Pull Request fusionado. 

• Historial final del repositorio. 

**6\. Seguridad, accesibilidad y documentación final (1,5P)** Realiza una revisión básica del servicio desplegado. 

Tareas solicitadas: 

• Comprueba que el repositorio es privado. 

• Comprueba que el usuario normal no tiene permisos de administración global. • Documenta qué puertos están abiertos y para qué sirve cada uno. 

• Explica dónde se guardan los datos persistentes de Gitea en el servidor. • Explica qué pasaría si se elimina la carpeta del volumen persistente. • Propón una copia de seguridad básica del directorio de datos de Gitea. • Incluye una tabla final con usuario, rol, repositorio, visibilidad, URL del servicio y puertos utilizados. 

Comandos orientativos: 

docker compose ps 

sudo ufw status verbose 

ls \-la \~/gitea 

tar \-czf backup-gitea.tar.gz \~/gitea/gitea 

Se deben aportar capturas de: 

• Estado del contenedor. 

• Estado del firewall. 

5 / 8  
• Directorio donde se guardan los datos persistentes. 

• Archivo de backup creado. 

• Configuración de visibilidad del repositorio. 

**BONUS (1P)** 

Configura una clave SSH para poder clonar y hacer push sin introducir usuario y contraseña. Tareas solicitadas: 

• Genera una clave SSH. 

• Añade la clave pública al usuario de Gitea. 

• Clona el repositorio usando la URL SSH de Gitea y el puerto 2222\. 

• Realiza un cambio, commit y push usando SSH. 

Comandos orientativos: 

ssh-keygen \-t ed25519 \-C "alumno@gitea" 

cat \~/.ssh/id\_ed25519.pub 

git clone ssh://git@IP\_DEL\_SERVIDOR:2222/usuario/practica-gitea.git 

Se deben aportar capturas de: 

• Clave pública añadida en Gitea. 

• Clonado por SSH. 

• Push realizado correctamente mediante SSH. 

**Importante:** no se debe entregar nunca la clave privada. 

**Recursos** 

• Documentación oficial de Gitea \- Instalación con Docker: 

https://docs.gitea.com/installation/install-with-docker 

• Documentación oficial de Gitea: https://docs.gitea.com/ 

• Sitio oficial de Gitea: https://about.gitea.com/ 

• Documentación oficial de Docker: https://docs.docker.com/ 

**Nota** 

• Cada pregunta requiere múltiples acciones. No basta una única captura. • Se recomienda copiar cada enunciado y listar cada una de las tareas solicitadas para realizarlas paso a paso. 

• Las capturas deben mostrar ventanas completas y comandos legibles. • No se aceptarán capturas recortadas de forma que no se pueda verificar qué alumno, máquina o servicio se está usando. 

• No se deben publicar contraseñas, tokens ni claves privadas en el documento. 6 / 8  
**Condiciones de entrega** 

• La práctica se debe entregar de forma individual. Cada alumno debe presentar sus propias respuestas. Sin embargo, se puede trabajar en equipo. 

• Se debe entregar un documento de texto (.pdf, .docx, .odt, etc.) con los ejercicios correctamente ordenados, identificados y numerados. 

• El documento debe tener por título DAW\_P12\_Apellido1\_Apellido2. • En cada página del documento debe aparecer el nombre completo del alumno. • Mostrar capturas de ventanas completas y claras que demuestren la realización de la práctica por parte del alumno. 

• Aportar siempre una breve descripción debajo de cada captura. 

• La nota comprenderá un valor numérico entre 0 y 10\. 

• La fecha límite de entrega es la indicada en Google Classroom. 

• Se podrá entregar hasta 72 horas más tarde de la fecha límite, pero con penalización sobre la puntuación. En ese caso no será posible aspirar al 10\. 

**Resumen de puntuación** 

Puntuació 

Apartado   
n 

1\. Preparación del servidor y comprobaciones iniciales 1,5P 

2\. Despliegue de Gitea con Docker Compose 2P 

3\. Configuración inicial de Gitea 1,5P 

4\. Sincronización de repositorio local con Gitea 2P 

5\. Ramas, Pull Request y Merge 1,5P 

6\. Seguridad, accesibilidad y documentación final 1,5P 

BONUS. Acceso por SSH con clave pública 1P extra 

7 / 8  
8 / 8