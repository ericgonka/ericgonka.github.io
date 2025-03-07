---
title: "Brainpan 1 - THM Writeup"
date: 2025-03-07
author: "Eric González"
---

# Brainpan 1 - THM Writeup
![image](https://github.com/user-attachments/assets/8cfd879e-da7c-4b6f-b14e-cb294ba18a94)

## Introducción

**Brainpan 1** es una máquina de nivel principiante/intermedio en TryHackMe (THM) diseñada para practicar explotación de binarios y ejecución de código remoto. En este writeup, analizaremos los pasos clave para comprometer la máquina, desde el reconocimiento hasta la escalación de privilegios.

## Enumeración

Comenzamos con un escaneo de puertos para identificar servicios en ejecución:

```bash
nmap -sC -sV -A -T4 <IP>
