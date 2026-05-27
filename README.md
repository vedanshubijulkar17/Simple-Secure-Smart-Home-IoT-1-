# Simple Secure Smart Home IoT Architecture -1 🏠⚙️

An end-to-end, 4-layer IoT Command & Control architecture designed to securely manage edge devices from a real-time web dashboard. Built using asynchronous C++ callbacks, Python data logging, and encrypted MQTT WebSockets.

## 🏗️ System Architecture (The 4 Layers)

This project strictly adheres to an industrial 4-layer IoT methodology:

1. **Layer 1: The Edge (Hardware/Sensors)**
   * **Tech:** ESP32, C++ (`PubSubClient`, `WiFiClientSecure`).
   * **Function:** A virtual ESP32 acts as a smart appliance. It connects to secure Wi-Fi, enforces SSL verification, and utilizes asynchronous callback functions to execute incoming commands with zero latency.
2. **Layer 2: The Transport (Network/Routing)**
   * **Tech:** HiveMQ Cloud (MQTT Broker).
   * **Function:** Handles encrypted message routing over port `8883` (Standard) and `8884` (WebSockets). Implements strict Access Management (username/password) to prevent unauthorized publishers.
3. **Layer 3: Processing (Backend Audit Server)**
   * **Tech:** Python (`paho-mqtt` v2 API).
   * **Function:** A backend script running independently that subscribes to the command topic. It acts as an immutable audit logger, catching every command sent through the broker and generating a timestamped log to track system activity.
4. **Layer 4: Application (User Interface)**
   * **Tech:** HTML, CSS, Vanilla JavaScript (`paho-mqtt` WebSocket library).
   * **Function:** A lightweight, real-time web dashboard. It establishes a secure WebSocket connection to the MQTT broker, allowing the user to publish control commands directly from the browser.

---

> ⚠️ **IMPORTANT CLONING NOTE: Cloud Credentials**
> To run this project locally, you must create a free [HiveMQ Cloud Cluster](https://www.hivemq.com/mqtt-cloud-broker/). 
> In all three source files (`esp32_smart_light.ino`, `backend_logger.py`, and `index.html`), you must replace the placeholder `"REPLACE_WITH_YOUR_EXACT_HIVEMQ_URL"` and the `username`/`password` variables with your own active credentials. 
> *Note: Ensure your broker URL is just the raw domain name (e.g., `e12345.s1.eu.hivemq.cloud`) with no `https://` prefix or trailing slashes.*

---

## 🚀 How to Run the Project

### 1. The Edge Simulator
* Open [Wokwi ESP32 Simulator](https://wokwi.com/).
* Copy the code from `/edge_device/esp32_smart_light.ino`.
* Ensure you add a virtual LED to Pin 2.
* Add the `PubSubClient` library to the Wokwi project.
* Update the Wi-Fi and HiveMQ credentials in the code, then press **Play**.

### 2. The Python Backend
* Ensure Python 3.x is installed on your machine.
* Install the Paho-MQTT library: `pip install paho-mqtt`
* Navigate to the backend folder: `cd backend_server`
* Update your HiveMQ credentials in the script.
* Run the logger: `python backend_logger.py`
* The terminal will begin auditing and printing incoming commands.

### 3. The Web Dashboard
* Navigate to the `/web_application` folder.
* Update your HiveMQ credentials and secure WebSocket port (`8884`) in the script.
* Open `index.html` directly in any modern web browser.
* Wait for the "Connected to Cloud!" status.
* Click the buttons to send commands and watch the Wokwi LED toggle while the Python terminal logs the activity.

## 🛠️ Technical Stack
* **Languages:** C/C++ (Embedded), Python, JavaScript, HTML/CSS
* **Protocols:** MQTT, WebSockets, SSL/TLS Encryption
* **Tools:** Wokwi Simulator, HiveMQ Cloud Broker
