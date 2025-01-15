# Security Tools Web App with SFTP, Chatroom, and Gemini Chatbot

This repository contains a Python backend built with Flask and Flask-SocketIO and a Flutter-based frontend for a web application. The application integrates security tools, an SFTP service, a real-time chatroom, and Gemini chatbot API.

---

## Features

### 1. Security Tools
- **Password Generator**: Generate strong, random passwords.
- **Hashing Services**: Hash plaintext and identify hash types.
- **Key Pair Generator**: Generate RSA key pairs for encryption.
- **Network Sniffer**: Analyze network traffic with Scapy.
- **Wordlist Generator**: Create custom wordlists for security audits.

### 2. SFTP Service
- Manage and transfer files securely via the SFTP protocol.

### 3. Chatroom
- Real-time messaging with Flask-SocketIO.

### 4. Gemini Chatbot
- Integrated chatbot powered by Gemini API for enhanced user interaction.

---

## Setup Instructions

### Prerequisites
Ensure you have the following installed:
- Python 3.13+
- Flutter SDK (3.27.1)
- PIP (Python Package Manager)

---

### Backend Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-repo/security-tools-app.git
   cd security-tools-app/backend
   ```

2. **Create a Virtual Environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the Application**:
   ```bash
   python apis.py
   ```
   The backend will be available at `http://0.0.0.0:5000`.

5. **Environment Variables**:
   Create a `.env` file to configure the Gemini API key and other sensitive information.
   ```env
   GEMINI_API_KEY=your_api_key_here
   ```

---

### Flutter Installation

1. **Install Flutter**:
   - Follow the official guide: [Flutter Installation Guide](https://flutter.dev/docs/get-started/install).

2. **Verify Installation**:
   ```bash
   flutter doctor
   ```

3. **Run the Frontend**:
   Navigate to the frontend folder:
   ```bash
   cd ../frontend
   flutter pub get
   flutter run
   ```

---

## Backend Code Snippets

### 1. Password Generator
```python
def generate_random_password(length=12):
    characters = ascii_letters + digits + punctuation
    return ''.join(choice(characters) for _ in range(length))

@socketio.on("generate_password")
def handle_generate_password(data):
    length = data.get("length", 12)
    password = generate_random_password(length)
    emit("generate_password", {"status": "success", "password": password})
```

### 2. Port Scanner
```python
@socketio.on("port_scan")
def port_scan(data):
    ip = data.get("ip")
    ports = data.get("ports", range(1, 1025))
    nm = PortScanner()
    nm.scan(ip, ",".join(map(str, ports)))
    open_ports = [port for port in ports if nm[ip]['tcp'][port]['state'] == 'open']
    emit("port_scan", {"status": "success", "open_ports": open_ports})
```

---

## Flutter Code Snippets

### 1. SFTP Integration
```dart
void connectSFTP() {
  // Implement SFTP connection using dart:io or a package like sftp_client
}
```

### 2. Chatroom WebSocket Listener
```dart
void connectSocket() {
  final socket = io.io('http://localhost:5000', <String, dynamic>{
    'transports': ['websocket'],
  });

  socket.on('message', (data) => print('Message received: $data'));
  socket.emit('send_message', {'content': 'Hello from Flutter'});
}
```

---

## Notes

- Ensure the backend and frontend are running simultaneously.
- For real-time features, ensure Socket.IO is correctly configured in both the backend and frontend.
- Always secure sensitive data using environment variables.

---

## License
This project is licensed under the MIT License.
