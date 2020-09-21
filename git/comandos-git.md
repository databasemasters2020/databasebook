---
description: Lista de comandos más utilizados
---

# Resumen Comandos Git

| Comando | Descripción |
| :--- | :--- |
| git status | Te mostrará los diferentes estados de los archivos en tu directorio de trabajo y stage. |
| git add | Añade contenido del directorio de trabajo al stage. |
| git rm | Elimina archivo del stage y directorio de trabajo. |
| git mv | Mueve un archivo de un directorio a otro. |
| git commit | Toma todos los contenidos de los archivos a los que se les realiza el seguimiento con git add y guarda el estado en la base de datos de git, este estado puede ser recuperado mediante el id del commit. |
| git log | Muestra el registro de los commits realizados en el proyecto. |
| git reset | Se utiliza para deshacer las cosas. Puede cambiar el stage por un stage asociado a otro commit. Si se utiliza con --hard puedes llegar a perder archivos de forma definitiva, ya que, modificas no solo el stage, si no que también el directorio de trabajo. |
| git checkout -- . | Restaura nuestros archivos al estado del último commit realizado. Este comando es utilizado principalmente para el manejo de ramas, pero ese contenido está fuera del alcance de este tutorial. |
| git clone URL/name.git proyecto | Clona un proyecto de git en la carpeta proyecto. |
| git remote add origin link | Añade el origen remoto asociado a link y lo guarda como origin. |
| git remote -v | Muestra los orígenes remotos añadidos al proyecto. |
| git push origin master | Sube los archivos al repositorio remoto. |
| git pull origin master | Descarga los archivos del repositorio remoto. |



