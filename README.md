# 🛡️ Ataque ARP Spoofing - Man in the Middle (MITM)

📹 [Video demostración](https://youtu.be/oj0Hj4uqqew) | 📂 [Repositorio](https://github.com/info-elianny/Ataque-ARP_MITM)

---

## 📌 Objetivo del Laboratorio

Simular un ataque de ARP Spoofing (Man-in-the-Middle) en una red local utilizando **Kali Linux** como atacante y **Windows 7** como víctima, con el fin de comprender cómo un atacante puede interceptar, monitorear y manipular el tráfico de red mediante el envenenamiento de las tablas ARP de los dispositivos.

### Topología de Red
![Topología de Red](https://i.postimg.cc/QNHzyM2v/ARP-1.png)

---

## 🎯 Objetivo del Script

Implementar un ataque de ARP Spoofing (Man-in-the-Middle) mediante el envío de respuestas ARP falsas entre una víctima y el gateway, con el propósito de posicionar al equipo atacante (Kali Linux) como intermediario en la comunicación, permitiendo la interceptación y análisis del tráfico de red mientras se mantiene la conectividad de la víctima (Windows 7) mediante el uso de **IP Forwarding**.

---

## ⚙️ Requisitos

### Hardware y Software

- Un equipo que sirva de **atacante** (Kali Linux)
- Un equipo que sirva de **víctima** (Windows 7)
- Ambos equipos conectados a la **misma red local**
- Un router o **gateway** accesible desde ambos dispositivos
- **Python 3** instalado
- Biblioteca **Scapy** instalada
- Permisos de **administrador (root)** para ejecutar el script

### Instalación de dependencias

```bash
pip install scapy
```

---

## 🔧 Parámetros Utilizados

| Parámetro | Descripción |
|-----------|-------------|
| Interfaz de red | Adaptador de red desde donde se realizará el ataque |
| IP de la víctima | Dirección del equipo a atacar (Windows 7) |
| IP del gateway | Dirección IP del router o puerta de enlace |
| MAC de la víctima | Obtenida automáticamente mediante solicitudes ARP |
| MAC del gateway | Obtenida automáticamente mediante solicitudes ARP |
| IP Forwarding | Función que permite reenviar paquetes entre víctima y gateway |

---

## 🚀 Cómo Ejecutar el Script

```bash
sudo python3 arp_mitm.py
```

> ⚠️ **Debe ejecutarse con permisos root (sudo)**

---

## 📋 Funcionamiento del Script

**Paso 1:** El script comprueba que el usuario tenga permisos de administrador (root), ya que la captura y el envío de tramas de red requieren privilegios elevados.

**Paso 2:** Se solicita al usuario la **interfaz de red**, la **IP de la víctima** y la **IP del gateway**.

**Paso 3:** Mediante solicitudes ARP, el script obtiene automáticamente las **direcciones MAC** de la víctima y del gateway.

**Paso 4:** Se habilita el **IP Forwarding** en Kali Linux para que el tráfico continúe circulando sin interrumpir la comunicación.

**Paso 5:** El script envía **respuestas ARP falsas** tanto a la víctima como al gateway, haciendo que ambos asocien sus IPs con la MAC del atacante. Este proceso se repite cada **2 segundos** para mantener activo el ataque.

---

## 📸 Capturas de Pantalla

### Tabla ARP antes del ataque
![Tabla ARP antes del ataque](https://i.postimg.cc/zGS2gHhL/ARP-CAP-1.png)

### Script en ejecución
![Script en ejecución](https://i.postimg.cc/85sZ9y0c/ARP-CAP-2.png)

### Tabla ARP con el ataque en marcha
![Tabla ARP con el ataque en marcha](https://i.postimg.cc/fTYBGSB2/ARP-CAP-3.png)

---

## 🌐 Documentación de la Red

| Dispositivo | Interfaz | Dirección IP | Máscara de Red |
|-------------|----------|--------------|----------------|
| R-1 (DHCP) | E0/0 | 6.6.1.1 | 255.255.255.0 |
| SW-1 | ---- | ----- | ----- |
| Kali Linux (Atacante) | e1 | 6.6.1.11 | 255.255.255.0 |
| Windows 7 (Víctima) | e0 | 6.6.1.15 | 255.255.255.0 |
| Cloud | net | 192.168.206.135 | 255.255.255.0 |

---

## 🛡️ Contramedidas para Mitigar el Ataque

### 1. Dynamic ARP Inspection (DAI)
Función de seguridad aplicada en los switches que intercepta y valida todos los paquetes ARP. El switch compara cada ARP Reply contra la tabla de DHCP Snooping. Si la combinación de MAC e IP no coincide con una entrada legítima, el paquete es descartado y se genera una alerta en los logs.

![DAI resultado](https://i.postimg.cc/q7DLssM5/ARP-M-1.png)

### 2. ARP Estático
Fijar manualmente la MAC del Gateway en cada host para que no pueda ser modificada por ARPs falsos.

![ARP Estático resultado](https://i.postimg.cc/mD0yqT96/ARP-M-2.png)

### 3. Segmentación de Red (VLANs)
Aislar los dispositivos en diferentes VLANs, lo cual limita el alcance del atacante si logra comprometer un segmento de la red.

![VLANs resultado](https://i.postimg.cc/Pfw4D8sd/ARP-M-3.png)

---

> ⚠️ **Este script es únicamente con fines educativos y de investigación en entornos controlados.**  
> ⚠️ **BY: Elianny**
