---
title: "Chemistry - Writeup"
date: 2025-03-14
author: "Eric González"
categories: 
  - HackTheBox
tags:
  - Fácil
  - CTF
image: "https://github.com/user-attachments/assets/7d025e82-9537-45b9-90d4-4ed977cfd0a1"

---

![{12B74D84-9ED8-42D3-99D4-1A50B3ACA804}](https://github.com/user-attachments/assets/e188d137-f034-4eda-b26c-ae95809ee4aa)


## Introducción

 Es una máquina de dificultad fácil en Linux que muestra una vulnerabilidad de Ejecución Remota de Código (RCE) en la librería de Python pymatgen (CVE-2024-23346) al cargar un archivo CIF malicioso en el sitio web CIF Analyzer alojado en el objetivo. Después de descubrir y descifrar los hashes, autenticamos en el objetivo a través de SSH como el usuario rosa. Para la escalada de privilegios, aprovechamos una vulnerabilidad de Path Traversal que lleva a una lectura arbitraria de archivos en una librería de Python llamada AioHTTP (CVE-2024-23334), que se usa en la aplicación web que corre internamente, para leer la bandera de root.

## Enumeración

Escaneamos el objetivo con `nmap` y nos muestra que hay 2 puertos abiertos.

```bash
nmap -sS --min-rate 5000 -n -Pn 10.10.11.38 -oG allPorts
```

![{FEA05A23-E00A-45BC-96D1-058487B481E3}](https://github.com/user-attachments/assets/37016364-10db-4510-bf3b-7c7b91d02f3a)

```bash
extractPorts allPorts
```

![{BA06B84B-8A4A-4DFF-A588-4C16546598BE}](https://github.com/user-attachments/assets/381252b3-0f57-4df2-bf82-f7aeb11a6820)

El puerto `22` está ejecutando `OpenSSH` y el puerto `5000` está ejecutando un servidor web `Python Werkzeug httpd 3.0.3`. Al visitar el puerto `5000`, vemos una página web de `CIF Analyzer` donde podemos iniciar sesión y registrarnos.

```bash
nmap -sCV -p22,5000 -n -Pn 10.10.11.38 -oN results
```

![{FEEABD5A-0EA9-4C09-B358-A7A8E80F6B65}](https://github.com/user-attachments/assets/3dbe8982-acf8-403d-8fff-9821f19cfad1)


Después de registrarnos e iniciar sesión, llegamos a un panel de control.

![{ABCA6DC8-2B0E-41B8-935C-30BC9C978C69}](https://github.com/user-attachments/assets/339c683d-c392-411c-9df6-65f9c61e7847)

![{686686D4-73B5-4AA2-9350-AF52169FBBD8}](https://github.com/user-attachments/assets/79a6ad28-9ec5-4c9a-8d8f-469d5fae4b3a)

## Explotación

Podemos cargar archivos en el servidor. Como nos está pidiendo un archivo CIF, investigamos qué son esos archivos.

> **Información:** Los archivos CIF, o Archivos de Información Cristalográfica, son archivos basados en texto que se utilizan para almacenar datos cristalográficos. Son ampliamente utilizados en ciencia de materiales, química y biología estructural para describir estructuras cristalinas.

Al buscar sobre la explotación de archivos CIF, encontramos una advertencia de seguridad en GitHub acerca de la ejecución arbitraria de código al analizar archivos CIF. Además, nos proporciona un PoC (Prueba de Concepto) que podemos usar.

![{F45A83AD-7D20-4DBB-B78C-0E7D342FA1AE}](https://github.com/user-attachments/assets/6aefbdf7-7af9-4583-b1b6-b8d30cb976fe)

Lo modificamos para que nos mande 5 `pings` (PAQUETES ICMP).

![{EB8A63C4-6D57-44F3-9641-A425CD0040A6}](https://github.com/user-attachments/assets/4fbc025a-db70-49fe-84bc-846b4774bf6d)

Vemos que los recibimos correctamente.

![{28EC3528-3420-49DA-9B96-4442949F1C65}](https://github.com/user-attachments/assets/6792d033-5b28-4613-9759-f7554af69c7e)

Ahora que sabemos que funciona, lo modificamos para obtener una reverse shell.

![{E41683D2-99E5-40B3-A5B2-2FB474623DC8}](https://github.com/user-attachments/assets/27b5f531-bbb1-4efc-8a12-834ddbbad4c6)

Nos ponemos en escucha, lo subimos y al darle a `view` obtenemos la shell!

![{D8A7F9B3-A929-4681-9F26-0682BE463736}](https://github.com/user-attachments/assets/e8a0ca07-c549-4069-a86a-798dda34f304)

![{7E4A4993-037A-4129-B04A-14AA59CC5F34}](https://github.com/user-attachments/assets/fb52a618-0de4-4b55-8439-08881bfb82d6)

## Post Explotación

Actualizamos la shell a TTY y comenzamos a explorar. Encontramos un archivo database.db dentro del directorio instance. En el archivo de la base de datos, encontramos algunos hashes de contraseñas de usuarios.

![{D1AA6E78-D51B-453C-ADCC-0B3A53CB21D5}](https://github.com/user-attachments/assets/633ee2e5-ee99-4137-84ba-aaf682e93547)

![{CA300ED7-D422-4A59-8817-39079B63B34E}](https://github.com/user-attachments/assets/6668fcb6-9d4f-4428-b42e-33aee2276eac)

Miramos el archivo /etc/passwd y podemos ver que hay un usuario rosa.

![{74F9CE26-1F5D-4FE2-8BC9-501DCE6D7550}](https://github.com/user-attachments/assets/18883124-bbf2-4f14-b3d7-9ac264ac3dbc)

Guardamos y probamos de crackear el hash que encontramos de `rosa`.

![{0E8BDCAC-2B8D-4260-A0E4-1EFFC4229D47}](https://github.com/user-attachments/assets/cdbcba9a-44e1-4d99-9c72-8f3da279c43a)

Nos conectamos por SSH en el objetivo con la contraseña que recuperamos para el usuario `rosa`.

## Privilege Escalation

Al echar un vistazo a los puertos abiertos desde dentro de la sesión SSH, vemos que el puerto 8080 está abierto internamente en localhost.

![{883E2369-6D30-4EA7-ABF9-C34B3E41E6C9}](https://github.com/user-attachments/assets/768c85fc-aa3f-43dc-a944-e48503305cd6)

Hacemos un forward de ese puerto con SSH para poder acceder a él desde nuestra máquina local.

```bash
ssh -fNL 127.0.0.1:8081:127.0.0.1:8080 rosa@10.10.11.38
```

Después de iniciar el reenvío del puerto a través de SSH, podemos acceder a la aplicación web interna. Usando la contraseña del usuario rosa, podemos autentificarnos con éxito en la aplicación web alojada en el puerto 8080.

![{BB40759A-4AEE-4232-BAFA-D36E415D021D}](https://github.com/user-attachments/assets/83460bc5-4afb-4bc4-b9df-524069ec90b1)

Un escaneo de `nmap` en el puerto reenviado revela la versión de `AioHTTP`, también puedes verlo capturando la petición con `BurpSuite`.

![{75076BFC-E710-434F-B66E-C43DD3827198}](https://github.com/user-attachments/assets/2ea46400-dfd8-49cb-9255-4ae0d10f3ec9)

Al buscar en línea exploits para esta versión, descubrimos este [repositorio](https://github.com/z3rObyte/CVE-2024-23334-PoC) de GitHub que habla sobre una vulnerabilidad de Lectura Arbitraria de Archivos. Esta vulnerabilidad ocurre debido a cómo `AioHTTP` maneja las solicitudes de recursos estáticos. Al hacer fuzzing en el sitio, descubrimos que hay una carpeta llamada `assets` que maneja todos los recursos estáticos.

`
feroxbuster -u http://127.0.0.1:8080/ -w /usr/share/wordlists/dirb/directorylist-medium-2.3.txt
<SNIP>
404 GET 1l 3w 14c Auto-filtering found 404-like response
and created new filter; toggle off with --dont-filter
200 GET 88l 171w 1380c
http://127.0.0.1:8080/assets/css/style.css
200 GET 5l 83w 59344c
http://127.0.0.1:8080/assets/css/all.min.css
403 GET 1l 2w 14c http://127.0.0.1:8080/assets
200 GET 72l 171w 2491c
http://127.0.0.1:8080/assets/js/script.js
200 GET 2l 1294w 89501c
http://127.0.0.1:8080/assets/js/jquery-3.6.0.min.js
200 GET 20l 3036w 205637c
http://127.0.0.1:8080/assets/js/chart.js
200 GET 153l 407w 5971c http://127.0.0.1:8080/
403 GET 1l 2w 14c http://127.0.0.1:8080/assets/js/
403 GET 1l 2w 14c http://127.0.0.1:8080/assets/css/
403 GET 1l 2w 14c http://127.0.0.1:8080/assets/
403 GET 1l 2w 14c http://127.0.0.1:8080/assets/js
403 GET 1l 2w 14c http://127.0.0.1:8080/assets/cssf
`

Esta versión de `AioHTTP` es vulnerable al recorrido de ruta si el administrador del sistema configuró rutas de aplicación con `follow_symlinks=True`. Esto hace que el servidor web valide que el recurso solicitado cae bajo la raíz web de la aplicación.

![{0C752EB8-ACBA-47C3-8D36-B4DF99D7A881}](https://github.com/user-attachments/assets/b2c23903-d3ee-4a6a-af14-8e9d13329d1d)

Copiamos la clave privada de `root`, le damos permisos `600` y nos conectamos como `root`.

![{0FB7D5F1-8400-44CB-86E0-CE0636D1B7A6}](https://github.com/user-attachments/assets/8c2ac92c-2fe9-43d4-ba3e-ff328ef41542)

![{6A941282-D4C6-44C7-BC3C-B2FE4E63AEC6} 1](https://github.com/user-attachments/assets/b71f9fd1-5eef-4121-9564-a96964169211)




