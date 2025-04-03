## **Updated Flask Routes and Responsibilities**

| **Feature**                                            | **Assigned To**      | **Routes & Endpoints**                                   | **Description**                                                                                        |
| ------------------------------------------------------ | -------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| 🔹 **Fingerprint Management (ZKTeco Integration)**     | **Hafedh GUENICHI**  | `POST /fingerprint/register`                             | Extract fingerprint data from **ZKTeco**, convert it into a Firestore-compatible format, and store it. |
|                                                        |                      | `POST /fingerprint/verify`                               | Verify a user’s fingerprint upon scanning.                                                             |
|                                                        |                      | `GET /fingerprint/list`                                  | List all registered fingerprint scanners.                                                              |
|                                                        |                      | `DELETE /fingerprint/remove/<user_id>`                   | Remove a fingerprint record from Firestore.                                                            |
| 🔹 **Camera Management (CRUD)**                        | **Sarra KHADHRAOUI** | `POST /camera/add`                                       | Register a new camera (ID, name, location, status: `ON/OFF`).                                          |
|                                                        |                      | `GET /camera/list`                                       | Retrieve a list of all installed cameras.                                                              |
|                                                        |                      | `DELETE /camera/remove/<camera_id>`                      | Remove a camera from Firestore.                                                                        |
|                                                        |                      | `POST /camera/control/<camera_id>`                       | Turn a camera **ON/OFF**.                                                                              |
| 🔹 **Real-time Camera Streaming & Screenshot Capture** | **Ibtihel SALHI**    | `GET /camera/stream/<camera_id>`                         | Provide a WebRTC or RTSP stream link for admins.                                                       |
|                                                        |                      | `POST /camera/screenshot/<camera_id>`                    | Take a screenshot from a camera feed.                                                                  |
|                                                        |                      | `GET /camera/download/<camera_id>?timestamp=<timestamp>` | Download a recorded video at a specific time.                                                          |
| 🔹 **Face Recognition & Dataset Management**           |                      | `POST /face/identify`                                    | Identify a person’s face using ML/DL.                                                                  |
|                                                        |                      | `GET /face/history/<user_id>`                            | Retrieve movement history of a person.                                                                 |
| 🔹 **Access Control & Logs (CRUD)**                    | **Nesrine Ganouni**  | `POST /access/entry`                                     | Log when a person enters a room.                                                                       |
|                                                        |                      | `POST /access/exit`                                      | Log when a person exits a room.                                                                        |
|                                                        |                      | `GET /access/logs`                                       | Retrieve all access logs.                                                                              |
|                                                        |                      | `GET /access/logs/<room_id>`                             | Retrieve logs for a specific room.                                                                     |
| 🔹 **Security Alerts (CRUD)**                          | **Amel MABROUKI**    | `POST /alert/unauthorized_access`                        | Create an alert if an imposter enters a restricted room.                                               |
|                                                        |                      | `POST /alert/suspicious_activity`                        | Store an alert for unusual behavior.                                                                   |
|                                                        |                      | `GET /alert/list`                                        | Retrieve all security alerts.                                                                          |
|                                                        |                      | `DELETE /alert/remove/<alert_id>`                        | Remove an alert from Firestore.                                                                        |

---

## **🛠️ ZKTeco Fingerprint Scanner Integration**

Yes, **ZKTeco fingerprint scanners** can be used to extract fingerprint data and save it to Firestore. Here’s how:

1. **Extract Fingerprint Data**
    
    - Use **ZKTeco SDK** (`zkpy`, `pyzk`, or `zksoftware`) to retrieve fingerprint templates.
        
2. **Convert & Store Data in Firestore**
    
    - Convert the **fingerprint template into a Base64 string** and store it in Firestore under `/fingerprints/{user_id}`.
        
3. **Verify Fingerprint**
    
    - When a person scans their finger, compare the input **against stored templates**.
        

#### **Example Code for Extracting Fingerprints with ZKTeco in Python**

```python
from zk import ZK, const

zk = ZK('192.168.1.201', port=4370, timeout=5)
conn = zk.connect()
users = conn.get_users()

for user in users:
    print(f"UserID: {user.uid}, Name: {user.name}, Fingerprint Data: {user.fingerprint}")

conn.disconnect()
```

Then, save `user.fingerprint` to Firestore.

---

## **🔬 How to Test Without Physical Cameras?**

If you don't have real cameras, **you can use alternative methods** for testing:

1. **Use OpenCV with Pre-recorded Videos**
    
    - Simulate camera feeds using **videos** instead of actual cameras.
        
    - Load a **security footage** and process frames as if it's a real-time feed.
        
    
    ```python
    import cv2
    
    cap = cv2.VideoCapture('test_video.mp4')
    
    while cap.isOpened():
        ret, frame = cap.read()
        if not ret:
            break
        cv2.imshow('Simulated Camera', frame)
        if cv2.waitKey(25) & 0xFF == ord('q'):
            break
    
    cap.release()
    cv2.destroyAllWindows()
    ```
    
2. **Use Virtual Machines (VMs) with Virtual Cameras**
    
    - Use **OBS Studio** to create a **virtual camera** as a simulated video source.
        
    - Then, use OpenCV to capture the stream.
        
3. **Use a Webcam Instead of CCTV Cameras**
    
    - Plug in a **USB webcam** and treat it as a security camera.
        

---

## **🧠 Face Recognition Dataset Requirements**

1. **Each employee/admin must submit at least 15 images from different angles**.
    
2. **Preprocess Images Before Training:**
    
    - Convert images to grayscale.
        
    - Normalize lighting conditions.
        
    - Align facial landmarks.
        

### **Example: How to Train Face Recognition Model**

```python
import face_recognition
import os

dataset_path = 'dataset/employees/'
known_faces = []
known_names = []

for employee in os.listdir(dataset_path):
    employee_images = os.listdir(os.path.join(dataset_path, employee))
    for image_name in employee_images:
        image_path = os.path.join(dataset_path, employee, image_name)
        image = face_recognition.load_image_file(image_path)
        encoding = face_recognition.face_encodings(image)[0]
        known_faces.append(encoding)
        known_names.append(employee)

# Save encodings to Firestore
```

---

## **🛠️ Deployment & Testing Plan**

|**Phase**|**Testing Strategy**|
|---|---|
|✅ **Unit Testing**|Test individual routes using **Postman** or **pytest**.|
|✅ **Integration Testing**|Test how the API interacts with Firestore.|
|✅ **Face Recognition Testing**|Use **OpenCV with test images** and verify accuracy.|
|✅ **Security Testing**|Simulate an imposter to test **unauthorized access detection**.|

---

## **🔑 Final Thoughts**

- **ZKTeco fingerprint scanners** can be integrated using the `zkpy` SDK.
    
- **Cameras can be simulated** using **OpenCV, videos, virtual cameras, or VMs**.
    
- **Face recognition requires at least 15 images per person** for accurate identification.
    
- **Firestore will store fingerprints, logs, access events, and security alerts**.


---

1. **Receive and Store Images in Firestore**
    
2. **Process the Images for Face Recognition**
    
3. **Train the Model for Future Face Comparisons**
    

---
## **🔹 Flask Process for Storing & Training Faces**

### **1️⃣ React Uploads 15+ Images to Flask**

- React **sends 15 images** to `/face/register` for a specific user.
    

### **2️⃣ Flask Saves Images & Extracts Face Encodings**

```python
import face_recognition
import firebase_admin
from firebase_admin import credentials, storage, firestore
import os
import cv2
import numpy as np

# Initialize Firebase
cred = credentials.Certificate("firebase_credentials.json")
firebase_admin.initialize_app(cred, {"storageBucket": "your-bucket.appspot.com"})
db = firestore.client()
bucket = storage.bucket()

def save_face_images(user_id, images):
    user_folder = f"faces/{user_id}/"
    os.makedirs(user_folder, exist_ok=True)

    encodings = []
    
    for idx, image in enumerate(images):
        img_path = f"{user_folder}{idx}.jpg"
        cv2.imwrite(img_path, image)

        # Encode face
        face = face_recognition.load_image_file(img_path)
        encoding = face_recognition.face_encodings(face)
        if encoding:
            encodings.append(encoding[0])

        # Upload to Firebase Storage
        blob = bucket.blob(f"{user_folder}{idx}.jpg")
        blob.upload_from_filename(img_path)

    # Store encodings in Firestore
    db.collection("face_encodings").document(user_id).set({"encodings": np.array(encodings).tolist()})
    return {"message": "Face data stored successfully!"}
```

- Saves **face encodings in Firestore** (easier for ML matching).
    
- Uploads images to **Firebase Storage** for future retraining.
    

---

## **🔹 Flask Process for Identifying a Face**

### **1️⃣ A Camera Captures an Image and Sends It to Flask**

- The image is processed by Flask, and the face is compared with stored encodings.
    

### **2️⃣ Flask Compares with Stored Encodings**

```python
def identify_face(image):
    known_encodings = db.collection("face_encodings").stream()

    # Load and encode the new face
    unknown_face = face_recognition.load_image_file(image)
    unknown_encoding = face_recognition.face_encodings(unknown_face)

    if not unknown_encoding:
        return {"message": "No face detected"}

    unknown_encoding = unknown_encoding[0]

    for doc in known_encodings:
        stored_encodings = np.array(doc.to_dict()["encodings"])

        # Compare with stored faces
        matches = face_recognition.compare_faces(stored_encodings, unknown_encoding)
        if True in matches:
            return {"message": f"Face recognized as {doc.id}"}

    return {"message": "Unknown person detected!"}
```

- This checks **if the captured face exists in the database**.
    
- If a **match is found**, it **logs the person’s entry**.
    
- If **not found**, it **triggers an alert for an imposter**.
    

---

## **📌 Key Takeaways**

✅ **React handles image submission** (user cannot proceed without submitting 15+ images).  
✅ **Flask stores images in Firebase Storage & extracts face encodings**.  
✅ **Face recognition happens in Flask** using stored encodings from Firestore.  
✅ **If a match is found → log the entry. If not → trigger an alert.**

This approach **ensures security** while keeping React responsible for **image collection** and Flask responsible for **face recognition & logging**.

Do you want to **fine-tune alert conditions** (e.g., notify admins if an unregistered person enters the building)? 🚀