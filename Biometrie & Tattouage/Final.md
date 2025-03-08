### 📌 **Detailed Explanation of the Code**

This Python script is a **real-time facial recognition system** using **OpenCV, HOG (Histogram of Oriented Gradients), and a Support Vector Machine (SVM) classifier**. The goal is to detect faces, extract their features, classify them using a trained SVM model, and recognize the individual.

---

## **🔹 Imports and Their Roles**

```python
import os
import cv2
import numpy as np
import winsound
from skimage.feature import hog
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
```

1. **os**: Used for navigating directories to load images from the dataset.
2. **cv2 (OpenCV)**: Used for image processing and face detection.
3. **numpy**: Used for handling arrays and numerical data.
4. **winsound**: Used to generate a beep sound if an unrecognized person is detected.
5. **hog (from skimage.feature)**: Extracts Histogram of Oriented Gradients (HOG) features, which help identify facial structures.
6. **SVC (from sklearn.svm)**: A Support Vector Machine classifier used for face recognition.
7. **accuracy_score (from sklearn.metrics)**: Evaluates the performance of the classifier.
8. **train_test_split (from sklearn.model_selection)**: Splits data into training and testing sets.

---

## **🔹 Step 1: Loading Images and Labels**

```python
def load_images_and_labels(data_dir):
    labels = []
    features = []
    label_dict = {}
    label_count = 0

    for subject in os.listdir(data_dir):
        subject_path = os.path.join(data_dir, subject)
        if os.path.isdir(subject_path):
            label_dict[label_count] = subject  # Associate label ID with the person's name
            for image_name in os.listdir(subject_path):
                image_path = os.path.join(subject_path, image_name)
                image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)  # Convert to grayscale
                face, *_ = detect_face(image)
                if face is not None:
                    face = preprocess(face)  # Preprocess the face (resize, normalize)
                    features.append(extract_hog_features(face))  # Extract HOG features
                    labels.append(label_count)
            label_count += 1  # Increment the label ID after processing all images in a folder
    return np.array(features), np.array(labels), label_dict
```

### **💡 What This Function Does**

1. Loops through each **subject (person's name)** in the dataset directory.
2. For each subject, loads all images and converts them to grayscale.
3. Uses **face detection** to find the face in the image.
4. If a face is found, it is **preprocessed** (resized and normalized).
5. **HOG features** are extracted and stored.
6. The **labels and feature vectors** are returned along with a dictionary mapping label IDs to names.

---

## **🔹 Step 2: Face Detection**

```python
def detect_face(image):
    face_cascade = cv2.CascadeClassifier(
        cv2.data.haarcascades + "haarcascade_frontalface_default.xml"
    )
    faces = face_cascade.detectMultiScale(
        image, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30)
    )
    if len(faces) > 0:
        x, y, w, h = faces[0]
        return image[y : y + h, x : x + w], x, y, w, h
    return None, None, None, None, None
```

### **💡 What This Function Does**

1. **Loads** OpenCV's **pre-trained Haar cascade** face detector.
2. Applies the face detection algorithm on the **grayscale image**.
3. Returns the **cropped face** along with its position `(x, y, w, h)`.

---

## **🔹 Step 3: Preprocessing the Face**

```python
def preprocess(face):
    face = cv2.resize(face, (64, 64))  # Resize to 64x64 pixels
    face = cv2.equalizeHist(face)  # Enhance contrast using histogram equalization
    return face / 255.0  # Normalize pixel values between 0 and 1
```

### **💡 What This Function Does**

1. **Resizes** the face to a fixed **64x64 pixel** size.
2. **Applies histogram equalization** to enhance contrast and improve recognition.
3. **Normalizes pixel values** to be between 0 and 1.

---

## **🔹 Step 4: Extracting Features using HOG**

```python
def extract_hog_features(face):
    return hog(
        face,
        orientations=9,
        pixels_per_cell=(8, 8),
        cells_per_block=(2, 2),
        block_norm="L2-Hys",
        transform_sqrt=True,
        feature_vector=True,
    )
```

### **💡 What This Function Does**

- Extracts **HOG (Histogram of Oriented Gradients)** features from the preprocessed face.
- **HOG is useful for pattern recognition**, capturing edges and gradients.

---

## **🔹 Step 5: Training the SVM Model**

```python
data_dir = r"C:\Users\Hafedh GUENICHI\Desktop\Semester II\Biometrie & Tattouage\II\dataset\dataset"
data, labels, label_dict = load_images_and_labels(data_dir)

X_train, X_test, y_train, y_test = train_test_split(
    data, labels, test_size=0.2, random_state=42
)

model = SVC(kernel="linear", probability=True)
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print(f"Précision: {accuracy_score(y_test, y_pred) * 100:.2f}%")
```

### **💡 What This Section Does**

1. **Loads the dataset** and extracts features + labels.
2. **Splits data** into **80% training** and **20% testing**.
3. **Trains an SVM model** with a **linear kernel**.
4. **Predicts and evaluates** accuracy.

---

## **🔹 Step 6: Real-Time Recognition**

```python
def real_time_recognition():
    cap = cv2.VideoCapture(0)  # Open webcam

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        face, x, y, w, h = detect_face(gray)

        if face is not None:
            face = preprocess(face)
            features = extract_hog_features(face).reshape(1, -1)
            prediction = model.predict(features)[0]
            confidence = model.predict_proba(features).max()

            if confidence < 0.5:
                name = "Inconnu"
                color = (0, 0, 255)  # Red for unknown
                winsound.Beep(1000, 300)  # Sound alert
            else:
                name = label_dict.get(prediction, "Inconnu")
                color = (0, 255, 0)  # Green for recognized person

            # Draw rectangle and label
            cv2.rectangle(frame, (x, y), (x + w, y + h), color, 2)
            cv2.putText(
                frame, name, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.8, color, 2
            )

        cv2.imshow("Reconnaissance Faciale", frame)

        if cv2.waitKey(1) & 0xFF == ord("q"):
            break

    cap.release()
    cv2.destroyAllWindows()
```

### **💡 What This Function Does**

1. **Activates the webcam** and starts reading frames.
2. **Detects a face** in each frame.
3. **Extracts features** and predicts identity using SVM.
4. **Compares confidence**:
    - If **unknown**, a **red box + beep sound** is triggered.
    - If **recognized**, a **green box + name is displayed**.
5. **Displays the live video feed with recognition results**.
6. **Press "q" to exit.**

---

## **🔹 Conclusion**

This script: ✅ **Loads a dataset of faces**  
✅ **Extracts facial features using HOG**  
✅ **Trains an SVM model for classification**  
✅ **Detects faces in real-time and recognizes them**  
✅ **Alerts when an unknown person is detected**

It is a **basic facial recognition system** that can be improved by:

- **Using a deep learning model (CNNs) for better accuracy**
- **Improving dataset quality and size**
- **Handling multiple faces in real-time**