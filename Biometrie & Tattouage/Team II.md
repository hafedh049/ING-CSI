# SecureGuard - Enterprise Security System

SecureGuard is a comprehensive enterprise security management system with a Python backend and AI-powered features. The system provides advanced security monitoring with biometric authentication, access control, and intelligent surveillance capabilities.
## Table of Contents

- [Backend Features](#backend-features)
- [Team Members and Responsibilities](#team-members-and-responsibilities)
- [Flask Routes Implementation](#flask-routes-implementation)
- [Installation](#installation)
- [Environment Setup](#environment-setup)
- [Pre-trained Models](#pre-trained-models)
- [Technologies](#technologies)
## Backend Features

- **Camera System**: AI-powered camera management, video processing, and surveillance
- **Biometric Processing**: Facial recognition and fingerprint analysis
- **Access Control**: Intelligent access point management and monitoring
- **Security Analytics**: Anomaly detection and security incident analysis
- **Reporting System**: Automated report generation and data visualization
- **Integration Layer**: Hardware integration and API services
## Team Members and Responsibilities

### Hafedh GUENICHI

- **Camera Management System**
  - Camera registration and configuration API
  - Video stream processing with pre-trained models
  - Camera health monitoring
  - RTSP stream handling
  - Recording management and storage optimization

- **Video Analytics Pipeline**
  - Integration of pre-trained motion detection models
  - Object detection with YOLOv5/v8
  - Person tracking with DeepSORT
  - Suspicious activity detection
### Sarra KHADHRAOUI

- **Biometric Processing Engine**
  - Integration of pre-trained facial recognition models (DeepFace)
  - Fingerprint processing with pre-trained models
  - Biometric template management
  - Matching service implementation
  - Anti-spoofing detection

- **Identity Verification System**
  - Multi-factor biometric verification
  - Confidence scoring
  - Biometric data encryption
  - Template management
### Amel MABROUKI

- **Access Control System**
  - Access point management API
  - Door lock control service
  - Access scheduling engine
  - Access event processing
  - Integration with physical security devices
 
- **Security Event Processing**
  - Event stream processing
  - Real-time alert generation
  - Event correlation engine
  - Notification dispatch service
### Nesrine GANOUNI

- **Security Analytics Engine**
  - Integration of pre-trained anomaly detection models
  - Behavioral analysis with pre-trained models
  - Security scoring system
  - Intrusion detection service
  - Pattern recognition for suspicious activities

- **Reporting System**
  - Automated report generation
  - Data aggregation services
  - Statistical analysis
  - Visualization data preparation
  - Scheduled reporting service
### Ibtihel SALHI

- **System Integration Layer**
  - Hardware integration services
  - Third-party security system connectors
  - API gateway implementation
  - WebSocket services for real-time updates
  - Integration with building management systems

- **Database and Storage Management**

  - Database optimization
  - Data retention policies
  - Backup and recovery services
  - Data migration tools
  - High-availability configuration
## Flask Routes Implementation

### Camera Management System (Hafedh GUENICHI)

```python

from flask import Flask, request, jsonify, Response

import cv2

import numpy as np

from models.object_detection import YOLODetector

import time

  

app = Flask(__name__)

yolo_detector = YOLODetector(model_path="models/yolov8n.pt")

  

# List all cameras

@app.route('/api/cameras', methods=['GET'])

def list_cameras():

    # In production, fetch from database

    cameras = [

        {"id": 1, "name": "Main Entrance", "location": "Front Building", "status": "online", "ip": "192.168.1.101"},

        {"id": 2, "name": "Parking Lot", "location": "North Side", "status": "online", "ip": "192.168.1.102"},

        {"id": 3, "name": "Server Room", "location": "IT Department", "status": "online", "ip": "192.168.1.103"}

    ]

    return jsonify({"cameras": cameras})

  

# Register a new camera

@app.route('/api/cameras', methods=['POST'])

def add_camera():

    data = request.json

    # Validate required fields

    if not all(k in data for k in ["name", "location", "ip", "username", "password"]):

        return jsonify({"error": "Missing required fields"}), 400

    # In production, save to database and return the new camera with ID

    new_camera = {

        "id": 4,  # Generated ID

        "name": data["name"],

        "location": data["location"],

        "ip": data["ip"],

        "status": "online"

    }

    return jsonify({"camera": new_camera}), 201

  

# Get camera details

@app.route('/api/cameras/<int:camera_id>', methods=['GET'])

def get_camera(camera_id):

    # In production, fetch from database

    camera = {"id": camera_id, "name": "Main Entrance", "location": "Front Building", "status": "online"}

    return jsonify({"camera": camera})

  

# Update camera

@app.route('/api/cameras/<int:camera_id>', methods=['PUT'])

def update_camera(camera_id):

    data = request.json

    # In production, update in database

    updated_camera = {

        "id": camera_id,

        "name": data.get("name", "Main Entrance"),

        "location": data.get("location", "Front Building"),

        "status": data.get("status", "online")

    }

    return jsonify({"camera": updated_camera})

  

# Delete camera

@app.route('/api/cameras/<int:camera_id>', methods=['DELETE'])

def delete_camera(camera_id):

    # In production, delete from database

    return jsonify({"message": f"Camera {camera_id} deleted successfully"})

  

# Stream camera with object detection

@app.route('/api/cameras/<int:camera_id>/stream', methods=['GET'])

def stream_camera(camera_id):

    def generate_frames():

        # In production, connect to actual camera stream

        # For demo, we'll use a sample video or webcam

        cap = cv2.VideoCapture(0)  # Use 0 for webcam or video file path

        while True:

            success, frame = cap.read()

            if not success:

                break

            # Perform object detection

            detections = yolo_detector.detect(frame)

            # Draw bounding boxes

            for det in detections:

                x1, y1, x2, y2, conf, cls = det

                cv2.rectangle(frame, (int(x1), int(y1)), (int(x2), int(y2)), (0, 255, 0), 2)

                cv2.putText(frame, f"{yolo_detector.classes[int(cls)]}: {conf:.2f}",

                           (int(x1), int(y1) - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

            # Convert to JPEG

            ret, buffer = cv2.imencode('.jpg', frame)

            frame = buffer.tobytes()

            yield (b'--frame\r\n'

                   b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')

            time.sleep(0.1)  # Control frame rate

    return Response(generate_frames(), mimetype='multipart/x-mixed-replace; boundary=frame')

  

# Get recordings for a camera

@app.route('/api/cameras/<int:camera_id>/recordings', methods=['GET'])

def get_recordings(camera_id):

    # Optional date filtering

    start_date = request.args.get('start_date')

    end_date = request.args.get('end_date')

    # In production, fetch from storage system

    recordings = [

        {"id": 1, "camera_id": camera_id, "timestamp": "2023-06-15T08:30:00", "duration": 3600, "size_mb": 720},

        {"id": 2, "camera_id": camera_id, "timestamp": "2023-06-15T09:30:00", "duration": 3600, "size_mb": 720},

        {"id": 3, "camera_id": camera_id, "timestamp": "2023-06-15T10:30:00", "duration": 3600, "size_mb": 720}

    ]

    return jsonify({"recordings": recordings})

  

# Analyze video for objects/people

@app.route('/api/cameras/<int:camera_id>/analyze', methods=['POST'])

def analyze_video(camera_id):

    # Get parameters

    object_types = request.json.get('object_types', ['person'])  # Default to person detection

    time_range = request.json.get('time_range', {})

    # In production, process video from storage

    # For demo, return sample results

    analysis_results = {

        "camera_id": camera_id,

        "objects_detected": {

            "person": 12,

            "car": 5,

            "bag": 8

        },

        "timestamps": {

            "person": ["2023-06-15T08:35:21", "2023-06-15T08:42:15"]

        }

    }

    return jsonify({"analysis": analysis_results})
### Biometric Processing Engine (Sarra KHADHRAOUI)

```python
from flask import Flask, request, jsonify
import numpy as np
import cv2
from deepface import DeepFace
import os
from werkzeug.utils import secure_filename

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = 'uploads/'
os.makedirs(app.config['UPLOAD_FOLDER'], exist_ok=True)

# Enroll a face
@app.route('/api/biometrics/face/enroll', methods=['POST'])
def enroll_face():
    if 'image' not in request.files:
        return jsonify({"error": "No image provided"}), 400
    
    file = request.files['image']
    user_id = request.form.get('user_id')
    
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    if file.filename == '':
        return jsonify({"error": "No selected file"}), 400
    
    # Save the uploaded file
    filename = secure_filename(f"{user_id}_face.jpg")
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    file.save(filepath)
    
    try:
        # Extract face embedding using DeepFace
        embedding = DeepFace.represent(filepath, model_name="Facenet")
        
        # In production, save embedding to database
        
        return jsonify({
            "success": True,
            "user_id": user_id,
            "message": "Face enrolled successfully"
        })
    except Exception as e:
        return jsonify({"error": str(e)}), 500

# Verify a face
@app.route('/api/biometrics/face/verify', methods=['POST'])
def verify_face():
    if 'image' not in request.files:
        return jsonify({"error": "No image provided"}), 400
    
    file = request.files['image']
    user_id = request.form.get('user_id')
    
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    # Save the uploaded file
    filename = secure_filename("verify_face.jpg")
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    file.save(filepath)
    
    try:
        # Get the enrolled face path
        enrolled_face_path = os.path.join(app.config['UPLOAD_FOLDER'], f"{user_id}_face.jpg")
        
        if not os.path.exists(enrolled_face_path):
            return jsonify({"error": "User face not enrolled"}), 404
        
        # Verify face using DeepFace
        result = DeepFace.verify(filepath, enrolled_face_path)
        
        return jsonify({
            "verified": result["verified"],
            "confidence": result["distance"],
            "user_id": user_id
        })
    except Exception as e:
        return jsonify({"error": str(e)}), 500

# Enroll a fingerprint
@app.route('/api/biometrics/fingerprint/enroll', methods=['POST'])
def enroll_fingerprint():
    if 'image' not in request.files:
        return jsonify({"error": "No fingerprint image provided"}), 400
    
    file = request.files['image']
    user_id = request.form.get('user_id')
    finger_id = request.form.get('finger_id', 'right_index')  # Default to right index finger
    
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    # Save the uploaded file
    filename = secure_filename(f"{user_id}_{finger_id}.jpg")
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    file.save(filepath)
    
    try:
        # In a real implementation, process the fingerprint image and extract minutiae
        # For demo, we'll simulate this process
        
        # Read the image
        img = cv2.imread(filepath, cv2.IMREAD_GRAYSCALE)
        
        # Enhance the fingerprint image
        # img = cv2.equalizeHist(img)
        
        # Extract features (in production, use a proper fingerprint SDK)
        # For demo, we'll just simulate this
        
        return jsonify({
            "success": True,
            "user_id": user_id,
            "finger_id": finger_id,
            "message": "Fingerprint enrolled successfully"
        })
    except Exception as e:
        return jsonify({"error": str(e)}), 500

# Verify a fingerprint
@app.route('/api/biometrics/fingerprint/verify', methods=['POST'])
def verify_fingerprint():
    if 'image' not in request.files:
        return jsonify({"error": "No fingerprint image provided"}), 400
    
    file = request.files['image']
    user_id = request.form.get('user_id')
    finger_id = request.form.get('finger_id', 'right_index')
    
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    # Save the uploaded file
    filename = secure_filename("verify_fingerprint.jpg")
    filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
    file.save(filepath)
    
    try:
        # Get the enrolled fingerprint path
        enrolled_fp_path = os.path.join(app.config['UPLOAD_FOLDER'], f"{user_id}_{finger_id}.jpg")
        
        if not os.path.exists(enrolled_fp_path):
            return jsonify({"error": "User fingerprint not enrolled"}), 404
        
        # In a real implementation, match the fingerprints
        # For demo, we'll simulate this process with a random match score
        match_score = np.random.uniform(0.7, 0.99) if np.random.random() > 0.2 else np.random.uniform(0.1, 0.6)
        
        return jsonify({
            "verified": match_score > 0.7,
            "confidence": float(match_score),
            "user_id": user_id,
            "finger_id": finger_id
        })
    except Exception as e:
        return jsonify({"error": str(e)}), 500
```

### Access Control System (Amel MABROUKI)

```python
from flask import Flask, request, jsonify
import datetime
import uuid

app = Flask(__name__)

# List all access points
@app.route('/api/access-points', methods=['GET'])
def list_access_points():
    # In production, fetch from database
    access_points = [
        {"id": 1, "name": "Main Entrance", "location": "Front Building", "status": "locked", "type": "Door"},
        {"id": 2, "name": "Server Room", "location": "IT Department", "status": "locked", "type": "Door"},
        {"id": 3, "name": "Parking Gate", "location": "North Side", "status": "unlocked", "type": "Gate"}
    ]
    return jsonify({"access_points": access_points})

# Get access point details
@app.route('/api/access-points/<int:point_id>', methods=['GET'])
def get_access_point(point_id):
    # In production, fetch from database
    access_point = {
        "id": point_id,
        "name": "Main Entrance",
        "location": "Front Building",
        "status": "locked",
        "type": "Door",
        "last_accessed": "2023-06-15T08:30:00",
        "security_level": "high"
    }
    return jsonify({"access_point": access_point})

# Lock an access point
@app.route('/api/access-points/<int:point_id>/lock', methods=['POST'])
def lock_access_point(point_id):
    # In production, send command to hardware
    # For demo, just return success
    return jsonify({
        "success": True,
        "access_point_id": point_id,
        "status": "locked",
        "timestamp": datetime.datetime.now().isoformat()
    })

# Unlock an access point
@app.route('/api/access-points/<int:point_id>/unlock', methods=['POST'])
def unlock_access_point(point_id):
    # Validate user has permission
    user_id = request.json.get('user_id')
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    # In production, verify user permission and send command to hardware
    # For demo, just return success
    return jsonify({
        "success": True,
        "access_point_id": point_id,
        "status": "unlocked",
        "timestamp": datetime.datetime.now().isoformat(),
        "unlocked_by": user_id
    })

# Get access events with filtering
@app.route('/api/access-events', methods=['GET'])
def get_access_events():
    # Get filter parameters
    user_id = request.args.get('user_id')
    access_point_id = request.args.get('access_point_id')
    start_date = request.args.get('start_date')
    end_date = request.args.get('end_date')
    status = request.args.get('status')  # 'granted' or 'denied'
    
    # In production, query database with filters
    # For demo, return sample data
    events = [
        {
            "id": 1,
            "timestamp": "2023-06-15T08:30:00",
            "user_id": "user123",
            "user_name": "John Doe",
            "access_point_id": 1,
            "access_point_name": "Main Entrance",
            "action": "entry",
            "status": "granted"
        },
        {
            "id": 2,
            "timestamp": "2023-06-15T09:15:00",
            "user_id": "user456",
            "user_name": "Jane Smith",
            "access_point_id": 2,
            "access_point_name": "Server Room",
            "action": "entry",
            "status": "denied"
        }
    ]
    
    # Apply filters (in production, this would be done in the database query)
    if user_id:
        events = [e for e in events if e["user_id"] == user_id]
    if access_point_id:
        events = [e for e in events if e["access_point_id"] == int(access_point_id)]
    if status:
        events = [e for e in events if e["status"] == status]
    
    return jsonify({"events": events})

# Request access to a point
@app.route('/api/access-points/<int:point_id>/request-access', methods=['POST'])
def request_access(point_id):
    data = request.json
    user_id = data.get('user_id')
    auth_method = data.get('auth_method', 'card')  # card, fingerprint, face
    
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    # In production, verify user permission and authentication
    # For demo, simulate access decision
    access_granted = True  # In real system, this would be based on permissions
    
    # Create access event
    event_id = str(uuid.uuid4())
    timestamp = datetime.datetime.now().isoformat()
    
    # In production, save to database
    
    if access_granted:
        # In production, send unlock command to hardware
        return jsonify({
            "success": True,
            "access_granted": True,
            "event_id": event_id,
            "timestamp": timestamp,
            "message": "Access granted"
        })
    else:
        return jsonify({
            "success": True,
            "access_granted": False,
            "event_id": event_id,
            "timestamp": timestamp,
            "message": "Access denied"
        })

# Set access schedule
@app.route('/api/access-points/<int:point_id>/schedule', methods=['POST'])
def set_schedule(point_id):
    data = request.json
    
    # Validate required fields
    if not all(k in data for k in ["days", "start_time", "end_time"]):
        return jsonify({"error": "Missing required fields"}), 400
    
    # In production, save schedule to database
    schedule = {
        "access_point_id": point_id,
        "days": data["days"],  # e.g., ["Monday", "Tuesday", "Wednesday"]
        "start_time": data["start_time"],  # e.g., "09:00"
        "end_time": data["end_time"],  # e.g., "17:00"
        "created_at": datetime.datetime.now().isoformat()
    }
    
    return jsonify({
        "success": True,
        "schedule": schedule
    })
```

### Security Analytics Engine (Nesrine GANOUNI)

```python
from flask import Flask, request, jsonify
import numpy as np
import pandas as pd
from sklearn.ensemble import IsolationForest
import datetime
import json
import matplotlib.pyplot as plt
import io
import base64

app = Flask(__name__)

# Initialize pre-trained anomaly detection model
isolation_forest = IsolationForest(contamination=0.05, random_state=42)
# In production, load a pre-trained model
# isolation_forest = joblib.load('models/isolation_forest.pkl')

# Get current security score
@app.route('/api/analytics/security-score', methods=['GET'])
def get_security_score():
    # In production, calculate based on various security metrics
    # For demo, return a sample score
    return jsonify({
        "overall_score": 85,
        "components": {
            "access_control": 90,
            "camera_system": 80,
            "biometric_system": 85,
            "intrusion_detection": 75
        },
        "trend": "+2%",
        "timestamp": datetime.datetime.now().isoformat()
    })

# List detected anomalies
@app.route('/api/analytics/anomalies', methods=['GET'])
def list_anomalies():
    # Get filter parameters
    start_date = request.args.get('start_date')
    end_date = request.args.get('end_date')
    severity = request.args.get('severity')
    
    # In production, query database with filters
    # For demo, return sample data
    anomalies = [
        {
            "id": 1,
            "timestamp": "2023-06-15T02:30:00",
            "type": "unusual_access_pattern",
            "description": "Multiple access attempts outside business hours",
            "severity": "high",
            "location": "Server Room",
            "user_id": "user123",
            "status": "open"
        },
        {
            "id": 2,
            "timestamp": "2023-06-15T14:45:00",
            "type": "camera_blind_spot",
            "description": "Camera 3 has been repositioned creating a blind spot",
            "severity": "medium",
            "location": "Parking Lot",
            "status": "resolved"
        }
    ]
    
    # Apply filters (in production, this would be done in the database query)
    if severity:
        anomalies = [a for a in anomalies if a["severity"] == severity]
    
    return jsonify({"anomalies": anomalies})

# Run anomaly detection on access logs
@app.route('/api/analytics/detect-access-anomalies', methods=['POST'])
def detect_access_anomalies():
    # In production, this would process real access logs
    # For demo, we'll use sample data
    
    # Sample access data (in production, this would come from the request or database)
    access_data = [
        {"user_id": "user123", "timestamp": "2023-06-15T08:30:00", "access_point": "Main Entrance", "day_of_week": 0, "hour": 8},
        {"user_id": "user123", "timestamp": "2023-06-15T17:30:00", "access_point": "Main Entrance", "day_of_week": 0, "hour": 17},
        {"user_id": "user456", "timestamp": "2023-06-15T09:15:00", "access_point": "Server Room", "day_of_week": 0, "hour": 9},
        {"user_id": "user123", "timestamp": "2023-06-15T02:30:00", "access_point": "Server Room", "day_of_week": 0, "hour": 2},
        {"user_id": "user789", "timestamp": "2023-06-15T23:45:00", "access_point": "Main Entrance", "day_of_week": 0, "hour": 23}
    ]
    
    # Convert to DataFrame
    df = pd.DataFrame(access_data)
    
    # Extract features for anomaly detection
    features = df[['day_of_week', 'hour']].values
    
    # Fit and predict
    # In production, the model would be pre-trained on historical data
    isolation_forest.fit(features)
    predictions = isolation_forest.predict(features)
    
    # Find anomalies (where prediction is -1)
    anomalies = []
    for i, pred in enumerate(predictions):
        if pred == -1:
            anomalies.append({
                "access_data": access_data[i],
                "anomaly_score": float(isolation_forest.score_samples([features[i]])[0]),
                "timestamp": datetime.datetime.now().isoformat()
            })
    
    return jsonify({
        "success": True,
        "anomalies_detected": len(anomalies),
        "anomalies": anomalies
    })

# Generate security report
@app.route('/api/analytics/security-report', methods=['POST'])
def generate_security_report():
    data = request.json
    report_type = data.get('report_type', 'daily')
    start_date = data.get('start_date')
    end_date = data.get('end_date')
    
    # In production, generate actual report from data
    # For demo, return sample report data
    
    # Sample data for charts
    access_by_hour = {
        "hours": list(range(24)),
        "counts": [5, 2, 1, 0, 0, 3, 15, 42, 67, 53, 48, 50, 62, 45, 53, 58, 43, 32, 12, 8, 5, 3, 2, 1]
    }
    
    # Create a simple chart
    plt.figure(figsize=(10, 5))
    plt.bar(access_by_hour["hours"], access_by_hour["counts"])
    plt.title('Access Events by Hour')
    plt.xlabel('Hour of Day')
    plt.ylabel('Number of Access Events')
    
    # Save plot to a bytes buffer
    buffer = io.BytesIO()
    plt.savefig(buffer, format='png')
    buffer.seek(0)
    
    # Convert to base64 for embedding in JSON
    chart_data = base64.b64encode(buffer.getvalue()).decode('utf-8')
    
    report = {
        "report_id": "SEC-2023-06-15",
        "report_type": report_type,
        "generated_at": datetime.datetime.now().isoformat(),
        "period": {
            "start_date": start_date or "2023-06-15T00:00:00",
            "end_date": end_date or "2023-06-15T23:59:59"
        },
        "summary": {
            "total_access_events": 512,
            "authorized_access": 498,
            "unauthorized_attempts": 14,
            "anomalies_detected": 3,
            "security_score": 85
        },
        "charts": {
            "access_by_hour": chart_data
        },
        "recommendations": [
            "Review after-hours access to Server Room",
            "Update camera positioning in Parking Lot",
            "Consider additional biometric verification for high-security areas"
        ]
    }
    
    return jsonify({
        "success": True,
        "report": report
    })

# Behavioral analysis
@app.route('/api/analytics/behavior-analysis', methods=['POST'])
def behavior_analysis():
    user_id = request.json.get('user_id')
    
    if not user_id:
        return jsonify({"error": "User ID is required"}), 400
    
    # In production, fetch user's historical access patterns
    # For demo, use sample data
    
    # Sample user behavior profile
    behavior_profile = {
        "user_id": user_id,
        "typical_access_times": {
            "Monday": ["08:30-17:30"],
            "Tuesday": ["08:45-17:15"],
            "Wednesday": ["08:30-17:30"],
            "Thursday": ["08:30-17:30"],
            "Friday": ["08:30-16:30"]
        },
        "typical_access_points": ["Main Entrance", "Office Area", "Cafeteria"],
        "unusual_activities": [
            {
                "timestamp": "2023-06-10T02:30:00",
                "description": "Server Room access outside business hours",
                "severity": "medium"
            }
        ],
        "risk_score": 15  # 0-100, higher is riskier
    }
    
    return jsonify({
        "success": True,
        "behavior_profile": behavior_profile
    })
```

### System Integration Layer (Ibtihel SALHI)

```python
from flask import Flask, request, jsonify
import requests
import json
import datetime
import threading
import time
import os

app = Flask(__name__)

# Mock database for demo purposes
# In production, use a real database
mock_db = {
    "devices": {
        "cameras": [
            {"id": 1, "name": "Main Entrance", "ip": "192.168.1.101", "status": "online"},
            {"id": 2, "name": "Parking Lot", "ip": "192.168.1.102", "status": "online"},
            {"id": 3, "name": "Server Room", "ip": "192.168.1.103", "status": "online"}
        ],
        "access_points": [
            {"id": 1, "name": "Main Entrance", "status": "locked"},
            {"id": 2, "name": "Server Room", "status": "locked"},
            {"id": 3, "name": "Parking Gate", "status": "unlocked"}
        ],
        "biometric_readers": [
            {"id": 1, "name": "Main Entrance Reader", "type": "fingerprint", "status": "online"},
            {"id": 2, "name": "Server Room Reader", "type": "facial", "status": "online"}
        ]
    },
    "integrations": {
        "building_management": {"url": "http://bms.example.com/api", "status": "connected"},
        "alarm_system": {"url": "http://alarm.example.com/api", "status": "connected"},
        "notification_service": {"url": "http://notifications.example.com/api", "status": "connected"}
    }
}

# List all integrated systems
@app.route('/api/integrations', methods=['GET'])
def list_integrations():
    return jsonify({"integrations": mock_db["integrations"]})

# Check integration status
@app.route('/api/integrations/<system_name>/status', methods=['GET'])
def check_integration_status(system_name):
    if system_name not in mock_db["integrations"]:
        return jsonify({"error": f"Integration {system_name} not found"}), 404
    
    # In production, actually check the connection
    integration = mock_db["integrations"][system_name]
    
    # Simulate checking connection
    try:
        # In production, make an actual request to the system
        # requests.get(integration["url"] + "/health", timeout=5)
        
        return jsonify({
            "system": system_name,
            "status": integration["status"],
            "last_checked": datetime.datetime.now().isoformat()
        })
    except Exception as e:
        return jsonify({
            "system": system_name,
            "status": "disconnected",
            "error": str(e),
            "last_checked": datetime.datetime.now().isoformat()
        })

# Register a new integration
@app.route('/api/integrations', methods=['POST'])
def add_integration():
    data = request.json
    
    # Validate required fields
    if not all(k in data for k in ["name", "url", "api_key"]):
        return jsonify({"error": "Missing required fields"}), 400
    
    # In production, validate the connection and save to database
    # For demo, just add to mock_db
    mock_db["integrations"][data["name"]] = {
        "url": data["url"],
        "status": "connected",
        "added_at": datetime.datetime.now().isoformat()
    }
    
    return jsonify({
        "success": True,
        "integration": {
            "name": data["name"],
            "url": data["url"],
            "status": "connected"
        }
    }), 201

# Send notification through integrated system
@app.route('/api/notifications/send', methods=['POST'])
def send_notification():
    data = request.json
    
    # Validate required fields
    if not all(k in data for k in ["recipients", "message", "priority"]):
        return jsonify({"error": "Missing required fields"}), 400
    
    # In production, actually send the notification through the integrated service
    # For demo, simulate sending
    notification = {
        "id": "notif-" + str(int(time.time())),
        "recipients": data["recipients"],
        "message": data["message"],
        "priority": data["priority"],
        "sent_at": datetime.datetime.now().isoformat(),
        "status": "sent"
    }
    
    return jsonify({
        "success": True,
        "notification": notification
    })

# Webhook endpoint for receiving events from integrated systems
@app.route('/api/webhooks/<system_name>', methods=['POST'])
def receive_webhook(system_name):
    data = request.json
    
    # In production, validate the webhook signature
    
    # Process the webhook data based on the source system
    if system_name == "alarm_system":
        # Handle alarm system events
        if data.get("event_type") == "alarm_triggered":
            # In production, create alert and notify security personnel
            print(f"Alarm triggered at {data.get('location')}")
    elif system_name == "building_management":
        # Handle building management system events
        if data.get("event_type") == "door_forced_open":
            # In production, create security incident
            print(f"Door forced open at {data.get('location')}")
    
    return jsonify({"success": True, "received_at": datetime.datetime.now().isoformat()})

# Database backup endpoint
@app.route('/api/database/backup', methods=['POST'])
def backup_database():
    # In production, actually perform database backup
    # For demo, simulate backup process
    
    # Start a background thread to simulate backup
    def perform_backup():
        time.sleep(5)  # Simulate backup taking time
        print("Database backup completed")
    
    threading.Thread(target=perform_backup).start()
    
    return jsonify({
        "success": True,
        "backup_started_at": datetime.datetime.now().isoformat(),
        "message": "Backup process started"
    })

# Get system health status
@app.route('/api/system/health', methods=['GET'])
def system_health():
    # In production, check actual system components
    # For demo, return sample data
    
    components = [
        {"name": "API Server", "status": "healthy", "response_time_ms": 42},
        {"name": "Database", "status": "healthy", "connections": 15},
        {"name": "Storage", "status": "healthy", "usage_percent": 68},
        {"name": "Camera Subsystem", "status": "healthy", "active_streams": 24},
        {"name": "Access Control Subsystem", "status": "healthy", "active_points": 18}
    ]
    
    return jsonify({
        "overall_status": "healthy",
        "timestamp": datetime.datetime.now().isoformat(),
        "components": components
    })
```

## Installation

```bash
# Clone the repository
git clone https://github.com/your-organization/secureguard-backend.git

# Navigate to the project directory
cd secureguard-backend

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the application
python app.py
```

## Environment Setup

Create a `.env` file in the root directory with the following variables:

```bash
# Flask Configuration
FLASK_APP=app.py
FLASK_ENV=development
SECRET_KEY=your-secret-key-here

# Database Configuration
DATABASE_URL=postgresql://user:password@localhost:5432/secureguard

# Pre-trained Model Paths
YOLO_MODEL_PATH=models/yolov8n.pt
FACE_RECOGNITION_MODEL=models/facenet_keras.h5
FINGERPRINT_MODEL=models/fingerprint_model.pkl
ANOMALY_DETECTION_MODEL=models/isolation_forest.pkl

# Storage Paths
UPLOAD_FOLDER=uploads
RECORDING_STORAGE=recordings

# Integration Services
NOTIFICATION_SERVICE_URL=http://notification-service:8090/api
ACCESS_CONTROL_HARDWARE_API=http://access-control-system:8080/api
```

## Pre-trained Models

The system uses the following pre-trained models:

1. **Object Detection**: YOLOv8 (nano version)
2. Download from: [https://github.com/ultralytics/ultralytics](https://github.com/ultralytics/ultralytics)
3. Place in: `models/yolov8n.pt`
4. **Facial Recognition**: DeepFace/FaceNet
5. Uses the DeepFace library which downloads models automatically
6. No manual download required
7. **Fingerprint Processing**:
8. For demonstration purposes, a simple image processing approach is used
9. In production, use a specialized fingerprint SDK
10. **Anomaly Detection**: Isolation Forest
11. Trained on historical access patterns
12. For demo, the model is trained on-the-fly
## Technologies

### Core Technologies

- **Backend Framework**: Flask
- **Database**: PostgreSQL, SQLite (for demo)
- **Task Queue**: Celery (for background tasks)
- **Container Orchestration**: Docker
### AI and Machine Learning

- **Computer Vision**: OpenCV, YOLOv8
- **Facial Recognition**: DeepFace
- **Anomaly Detection**: scikit-learn (Isolation Forest)
- **Video Processing**: FFmpeg
### Hardware Integration

- **Access Control**: REST APIs
- **Camera Systems**: RTSP, ONVIF
- **IoT Devices**: MQTT