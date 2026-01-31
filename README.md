# IoT Sensor Data Monitoring System (Security-Focused)
implemented with Hussein Mohammad in 2025

This project demonstrates a secure IoT system for real-time sensor data monitoring using ESP32 microcontrollers and a Flask server. It implements cryptographic and network defenses to ensure data integrity, authenticity, and availability in the face of common IoT attacks.

---

##  Overview

- Collects sensor data using:
  - **DHT11** for temperature and humidity
  - **MQ2** for gas detection
- Transmits data to a Flask server over HTTP
- Secures data with **HMAC-SHA256** to ensure integrity and authenticity
- Mitigates DoS attacks using **firewall rules**
- Stores data in **MySQL**
- Publishes data to **Node-RED dashboard** via **MQTT (HiveMQ Cloud)**

---

##  Key Security Features

| Threat                 | Mitigation                         |
|------------------------|-------------------------------------|
| Denial of Service (DoS)| iptables/UFW firewall rules         |
| Packet Sniffing        | Hash prevents modification validity |

---

##  System Components

###  Hardware

- **ESP32** – Sensor data collection and signing
- **MQ2 Gas Sensor**
- **DHT11 Sensor**

###  Software & Services

- **Flask** – HTTP server with hash verification logic
- **MySQL** – Data storage
- **Node-RED** – Real-time dashboard via MQTT
- **HiveMQ Cloud** – MQTT broker
- **Wireshark** – Attack simulation
- **iptables/UFW** – Network defense

---

##  Data Flow

(first security scenario)

1. **ESP32** collects temperature, humidity, and gas levels.
2. Sends data + hash as JSON via HTTP to Flask server.
3. Flask accepts the data from specefic IP address and rejects any data from other IP addresses.
4. then the Flask server store the data in Data base and send it to the broker.
5. Node-RED subscribes to MQTT topic and displays data live.

....................................

( second security scenario)

1. **ESP32** collects temperature, humidity, and gas levels.
2. It generates a **HMAC-SHA256 hash** using a secret key.
3. Sends data + hash as JSON via HTTP to Flask server.
4. Flask verifies the hash:
   - If valid → stores in MySQL and publishes to MQTT broker.
   - If invalid → rejects request.
4. then the Flask server store the data in Data base and send it to the broker.
5. Node-RED subscribes to MQTT topic and displays data live.

---

##  Attack Simulations

###  DoS Attack

- Simulated by flooding Flask server
- Mitigation:
  - **Rate limiting**
  - **Firewall rules** to block IPs

###  injection attack

- Simulated with tools like Wireshark to know the packet contents and then send incorrect values (data) with same formate of the correct packet.
- Mitigation:
  - Flask rejects altered data with invalid hash
  - HMAC ensures integrity and authenticity
 
- <img width="980" height="493" alt="Sytem architecture" src="https://github.com/user-attachments/assets/7254369b-11ca-4c9c-b769-da2fc1719971" />

