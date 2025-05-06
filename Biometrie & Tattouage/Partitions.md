### SecureNight API Documentation and Task Assignment

## Project Overview

SecureNight is a secure file system with fingerprint authentication, built with Flask and MongoDB. This document outlines the API routes that need to be implemented by each team member, along with example JSON responses.

---

## 1. Sarrah Khadhraoui - Authentication Routes

### 1.1. User Registration

- **Route**: `POST /api/auth/register`
- **Function**: `register()`
- **Description**: Register a new user
- **Example Request Body**:

```json
{
  "username": "testuser",
  "email": "test@example.com",
  "password": "Test@123",
  "phone_number": "+1234567890"
}
```

- **Example Response**:

```json
{
  "message": "User registered successfully",
  "user": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e1",
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "username": "testuser",
    "email": "test@example.com",
    "phone_number": "+1234567890",
    "fingerprint_hashes": [],
    "fingerprint_pictures": [],
    "last_login": null,
    "last_logout": null,
    "account_status": "active",
    "role": "client",
    "created_at": "2023-11-15T14:30:45.123Z"
  }
}
```

### 1.2. User Login

- **Route**: `POST /api/auth/login`
- **Function**: `login()`
- **Description**: Login with email/password or fingerprint
- **Example Request Body (Email/Password)**:

```json
{
  "email": "test@example.com",
  "password": "Test@123"
}
```

- **Example Response**:

```json
{
  "message": "Login successful",
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "username": "testuser",
    "email": "test@example.com",
    "role": "client",
    "account_status": "active"
  }
}
```
### 1.3. Refresh Token

- **Route**: `POST /api/auth/refresh`
- **Function**: `refresh()`
- **Description**: Get a new access token using refresh token
- **Example Response**:


```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```
### 1.4. Logout

- **Route**: `POST /api/auth/logout`
- **Function**: `logout()`
- **Description**: Logout the current user
- **Example Response**:

```json
{
  "message": "Logout successful"
}
```
### 1.5. Update Fingerprint

- **Route**: `POST /api/auth/update-fingerprint`
- **Function**: `update_fingerprint()`
- **Description**: Add or update user's fingerprint
- **Example Request Body**:

```json
{
  "fingerprint": "base64_encoded_fingerprint_data",
  "fingerprint_picture": "base64_encoded_image"
}
```

- **Example Response**:

```json
{
  "message": "Fingerprint updated successfully"
}
```

---
## 2. Amel Mabrouki - User Management Routes

### 2.1. Get All Users (Admin)

- **Route**: `GET /api/users/`
- **Function**: `get_users()`
- **Description**: Get a list of all users (admin only)
- **Example Response**:

```json
{
  "users": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e1",
      "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
      "username": "testuser",
      "email": "test@example.com",
      "phone_number": "+1234567890",
      "account_status": "active",
      "role": "client",
      "created_at": "2023-11-15T14:30:45.123Z"
    }
  ],
  "total": 1,
  "page": 1,
  "per_page": 10,
  "pages": 1
}
```

### 2.2. Get User by ID

- **Route**: `GET /api/users/<user_id>`
- **Function**: `get_user(user_id)`
- **Description**: Get a specific user by ID
- **Example Response**:

```json
{
  "user": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e1",
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "username": "testuser",
    "email": "test@example.com",
    "phone_number": "+1234567890",
    "fingerprint_pictures": [],
    "last_login": "2023-11-15T15:30:45.123Z",
    "last_logout": "2023-11-15T16:30:45.123Z",
    "account_status": "active",
    "role": "client",
    "created_at": "2023-11-15T14:30:45.123Z"
  }
}
```
### 2.3. Create User (Admin)

- **Route**: `POST /api/users/`
- **Function**: `create_user()`
- **Description**: Create a new user (admin only)
- **Example Request Body**:

```json
{
  "username": "newuser",
  "email": "new@example.com",
  "password": "New@123",
  "phone_number": "+9876543210",
  "role": "client",
  "account_status": "active"
}
```

- **Example Response**:

```json
{
  "message": "User created successfully",
  "user": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e2",
    "user_id": "b2c3d4e5-f6a7-8b9c-0d1e-2f3a4b5c6d7e",
    "username": "newuser",
    "email": "new@example.com",
    "phone_number": "+9876543210",
    "fingerprint_hashes": [],
    "fingerprint_pictures": [],
    "last_login": null,
    "last_logout": null,
    "account_status": "active",
    "role": "client",
    "created_at": "2023-11-15T17:30:45.123Z"
  }
}
```
### 2.4. Update User

- **Route**: `PUT /api/users/<user_id>`
- **Function**: `update_user(user_id)`
- **Description**: Update user information
- **Example Request Body**:


```json
{
  "username": "updateduser",
  "email": "updated@example.com",
  "phone_number": "+1122334455"
}
```

- **Example Response**:

```json
{
  "message": "User updated successfully",
  "user": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e1",
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "username": "updateduser",
    "email": "updated@example.com",
    "phone_number": "+1122334455",
    "fingerprint_pictures": [],
    "last_login": "2023-11-15T15:30:45.123Z",
    "last_logout": "2023-11-15T16:30:45.123Z",
    "account_status": "active",
    "role": "client",
    "created_at": "2023-11-15T14:30:45.123Z"
  }
}
```
### 2.5. Delete User (Admin)

- **Route**: `DELETE /api/users/<user_id>`
- **Function**: `delete_user(user_id)`
- **Description**: Delete a user and all associated data
- **Example Response**:


```json
{
  "message": "User and associated data deleted successfully"
}
```

---

## 3. Nesrine Gannouni - Device Management Routes

### 3.1. Get All Devices

- **Route**: `GET /api/devices/`
- **Function**: `get_devices()`
- **Description**: Get a list of all devices
- **Example Response**:

```json
{
  "devices": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e3",
      "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
      "device_name": "External SSD",
      "device_description": "1TB External SSD",
      "device_type": "SSD",
      "capacity": "1TB",
      "partitions": ["d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9"],
      "status": "active",
      "added_date": "2023-11-15T18:30:45.123Z"
    }
  ],
  "total": 1,
  "page": 1,
  "per_page": 10,
  "pages": 1
}
```
### 3.2. Get Device by ID

- **Route**: `GET /api/devices/<device_id>`
- **Function**: `get_device(device_id)`
- **Description**: Get a specific device by ID
- **Example Response**:

```json
{
  "device": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e3",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "device_name": "External SSD",
    "device_description": "1TB External SSD",
    "device_type": "SSD",
    "capacity": "1TB",
    "partitions": ["d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9"],
    "status": "active",
    "added_date": "2023-11-15T18:30:45.123Z"
  },
  "partitions": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e4",
      "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
      "partition_name": "Data",
      "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
      "format": "NTFS",
      "size": "500GB",
      "files": 5,
      "status": "active",
      "created_at": "2023-11-15T19:30:45.123Z"
    }
  ]
}
```
### 3.3. Create Device (Admin)

- **Route**: `POST /api/devices/`
- **Function**: `create_device()`
- **Description**: Create a new device
- **Example Request Body**:

```json
{
  "device_name": "New HDD",
  "device_description": "2TB External HDD",
  "device_type": "HDD",
  "capacity": "2TB",
  "status": "active"
}
```

- **Example Response**:

```json
{
  "message": "Device created successfully",
  "device": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e5",
    "device_id": "e5f6a7b8-c9d0-e1f2-a3b4-c5d6e7f8a9b0",
    "device_name": "New HDD",
    "device_description": "2TB External HDD",
    "device_type": "HDD",
    "capacity": "2TB",
    "partitions": [],
    "status": "active",
    "added_date": "2023-11-15T20:30:45.123Z"
  }
}
```

### 3.4. Update Device (Admin)

- **Route**: `PUT /api/devices/<device_id>`
- **Function**: `update_device(device_id)`
- **Description**: Update device information
- **Example Request Body**:


```json
{
  "device_name": "Updated HDD",
  "device_description": "Updated 2TB External HDD",
  "status": "inactive"
}
```

- **Example Response**:

```json
{
  "message": "Device updated successfully",
  "device": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e5",
    "device_id": "e5f6a7b8-c9d0-e1f2-a3b4-c5d6e7f8a9b0",
    "device_name": "Updated HDD",
    "device_description": "Updated 2TB External HDD",
    "device_type": "HDD",
    "capacity": "2TB",
    "partitions": [],
    "status": "inactive",
    "added_date": "2023-11-15T20:30:45.123Z"
  }
}
```

### 3.5. Delete Device (Admin)

- **Route**: `DELETE /api/devices/<device_id>`
- **Function**: `delete_device(device_id)`
- **Description**: Delete a device and associated partitions
- **Example Response**:


```json
{
  "message": "Device and associated partitions deleted successfully"
}
```

---
## 4. Ibtihel Salhi - Partition Management Routes

### 4.1. Get All Partitions

- **Route**: `GET /api/partitions/`
- **Function**: `get_partitions()`
- **Description**: Get a list of all partitions
- **Example Response**:

```json
{
  "partitions": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e4",
      "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
      "partition_name": "Data",
      "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
      "format": "NTFS",
      "size": "500GB",
      "files": 5,
      "status": "active",
      "created_at": "2023-11-15T19:30:45.123Z"
    }
  ],
  "total": 1,
  "page": 1,
  "per_page": 10,
  "pages": 1
}
```
### 4.2. Get Partition by ID

- **Route**: `GET /api/partitions/<partition_id>`
- **Function**: `get_partition(partition_id)`
- **Description**: Get a specific partition by ID
- **Example Response**:

```json
{
  "partition": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e4",
    "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
    "partition_name": "Data",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "format": "NTFS",
    "size": "500GB",
    "files": 5,
    "status": "active",
    "created_at": "2023-11-15T19:30:45.123Z"
  },
  "device": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e3",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "device_name": "External SSD",
    "device_description": "1TB External SSD",
    "device_type": "SSD",
    "capacity": "1TB",
    "status": "active",
    "added_date": "2023-11-15T18:30:45.123Z"
  },
  "files": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e6",
      "file_id": "f6a7b8c9-d0e1-f2a3-b4c5-d6e7f8a9b0c1",
      "file_name": "document.pdf",
      "file_size": 1024000,
      "file_type": "PDF",
      "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
      "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
      "encrypted": true,
      "upload_date": "2023-11-15T21:30:45.123Z",
      "last_modified_date": "2023-11-15T21:30:45.123Z",
      "file_path": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9/document.pdf"
    }
  ]
}
```
### 4.3. Create Partition (Admin)

- **Route**: `POST /api/partitions/`
- **Function**: `create_partition()`
- **Description**: Create a new partition
- **Example Request Body**:

```json
{
  "partition_name": "Backup",
  "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
  "format": "exFAT",
  "size": "500GB",
  "status": "active"
}
```

- **Example Response**:

```json
{
  "message": "Partition created successfully",
  "partition": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e7",
    "partition_id": "a7b8c9d0-e1f2-a3b4-c5d6-e7f8a9b0c1d2",
    "partition_name": "Backup",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "format": "exFAT",
    "size": "500GB",
    "files": 0,
    "status": "active",
    "created_at": "2023-11-15T22:30:45.123Z"
  }
}
```
### 4.4. Update Partition (Admin)

- **Route**: `PUT /api/partitions/<partition_id>`
- **Function**: `update_partition(partition_id)`
- **Description**: Update partition information
- **Example Request Body**:


```json
{
  "partition_name": "Updated Backup",
  "format": "NTFS",
  "status": "inactive"
}
```

- **Example Response**:

```json
{
  "message": "Partition updated successfully",
  "partition": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e7",
    "partition_id": "a7b8c9d0-e1f2-a3b4-c5d6-e7f8a9b0c1d2",
    "partition_name": "Updated Backup",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "format": "NTFS",
    "size": "500GB",
    "files": 0,
    "status": "inactive",
    "created_at": "2023-11-15T22:30:45.123Z"
  }
}
```
### 4.5. Delete Partition (Admin)

- **Route**: `DELETE /api/partitions/<partition_id>`
- **Function**: `delete_partition(partition_id)`
- **Description**: Delete a partition
- **Example Response**:


```json
{
  "message": "Partition deleted successfully"
}
```

---
## 5. Hafedh GUENICHI - File Management Routes

### 5.1. Get All Files

- **Route**: `GET /api/files/`
- **Function**: `get_files()`
- **Description**: Get a list of all files (admin sees all, users see only their own)
- **Example Response**:


```json
{
  "files": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e6",
      "file_id": "f6a7b8c9-d0e1-f2a3-b4c5-d6e7f8a9b0c1",
      "file_name": "document.pdf",
      "file_size": 1024000,
      "file_type": "PDF",
      "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
      "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
      "encrypted": true,
      "upload_date": "2023-11-15T21:30:45.123Z",
      "last_modified_date": "2023-11-15T21:30:45.123Z",
      "file_path": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9/document.pdf"
    }
  ],
  "total": 1,
  "page": 1,
  "per_page": 10,
  "pages": 1
}
```
### 5.2. Get File by ID

- **Route**: `GET /api/files/<file_id>`
- **Function**: `get_file(file_id)`
- **Description**: Get a specific file by ID
- **Example Response**:


```json
{
  "file": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e6",
    "file_id": "f6a7b8c9-d0e1-f2a3-b4c5-d6e7f8a9b0c1",
    "file_name": "document.pdf",
    "file_size": 1024000,
    "file_type": "PDF",
    "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "encrypted": true,
    "upload_date": "2023-11-15T21:30:45.123Z",
    "last_modified_date": "2023-11-15T21:30:45.123Z",
    "file_path": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9/document.pdf"
  },
  "partition": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e4",
    "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
    "partition_name": "Data",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "format": "NTFS",
    "size": "500GB",
    "files": 5,
    "status": "active",
    "created_at": "2023-11-15T19:30:45.123Z"
  },
  "device": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e3",
    "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
    "device_name": "External SSD",
    "device_description": "1TB External SSD",
    "device_type": "SSD",
    "capacity": "1TB",
    "status": "active",
    "added_date": "2023-11-15T18:30:45.123Z"
  }
}
```
### 5.3. Upload File

- **Route**: `POST /api/files/upload`
- **Function**: `upload_file()`
- **Description**: Upload a new file
- **Example Form Data**:


```json
file: [binary file data]
partition_id: d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9
encrypt: true
fingerprint: [base64_encoded_fingerprint_data]
```

- **Example Response**:


```json
{
  "message": "File uploaded successfully",
  "file": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e8",
    "file_id": "b8c9d0e1-f2a3-b4c5-d6e7-f8a9b0c1d2e3",
    "file_name": "confidential.docx",
    "file_size": 512000,
    "file_type": "DOCX",
    "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "encrypted": true,
    "upload_date": "2023-11-15T23:30:45.123Z",
    "last_modified_date": "2023-11-15T23:30:45.123Z",
    "file_path": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9/confidential.docx"
  }
}
```
### 5.4. Download File

- **Route**: `GET /api/files/<file_id>/download`
- **Function**: `download_file(file_id)`
- **Description**: Download a file (with fingerprint for encrypted files)
- **Example Query Parameters (for encrypted files)**:


```json
fingerprint=[base64_encoded_fingerprint_data]
```

- **Example Response**: Binary file data with appropriate headers

### 5.5. Update File

- **Route**: `PUT /api/files/<file_id>`
- **Function**: `update_file(file_id)`
- **Description**: Update file information
- **Example Request Body**:

```json
{
  "file_name": "updated_document.pdf"
}
```

- **Example Response**:

```json
{
  "message": "File updated successfully",
  "file": {
    "_id": "65a7b2c3d4e5f6a7b8c9d0e6",
    "file_id": "f6a7b8c9-d0e1-f2a3-b4c5-d6e7f8a9b0c1",
    "file_name": "updated_document.pdf",
    "file_size": 1024000,
    "file_type": "PDF",
    "partition_id": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9",
    "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
    "encrypted": true,
    "upload_date": "2023-11-15T21:30:45.123Z",
    "last_modified_date": "2023-11-16T00:30:45.123Z",
    "file_path": "d4e5f6a7-b8c9-d0e1-f2a3-b4c5d6e7f8a9/updated_document.pdf"
  }
}
```
### 5.6. Delete File

- **Route**: `DELETE /api/files/<file_id>`
- **Function**: `delete_file(file_id)`
- **Description**: Delete a file
- **Example Response**:


```json
{
  "message": "File deleted successfully"
}
```
### 5.7. Dashboard Statistics (Admin)

- **Route**: `GET /api/users/dashboard/stats`
- **Function**: `get_dashboard_stats()`
- **Description**: Get system statistics for admin dashboard
- **Example Response**:


```json
{
  "stats": {
    "users": 10,
    "devices": 5,
    "partitions": 12,
    "files": 50
  },
  "recent_activity": {
    "users": [
      {
        "_id": "65a7b2c3d4e5f6a7b8c9d0e1",
        "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
        "username": "testuser",
        "email": "test@example.com",
        "account_status": "active",
        "role": "client",
        "created_at": "2023-11-15T14:30:45.123Z"
      }
    ],
    "files": [
      {
        "_id": "65a7b2c3d4e5f6a7b8c9d0e6",
        "file_id": "f6a7b8c9-d0e1-f2a3-b4c5-d6e7f8a9b0c1",
        "file_name": "document.pdf",
        "file_type": "PDF",
        "upload_date": "2023-11-15T21:30:45.123Z"
      }
    ],
    "devices": [
      {
        "_id": "65a7b2c3d4e5f6a7b8c9d0e3",
        "device_id": "c3d4e5f6-a7b8-c9d0-e1f2-a3b4c5d6e7f8",
        "device_name": "External SSD",
        "device_type": "SSD",
        "added_date": "2023-11-15T18:30:45.123Z"
      }
    ]
  }
}
```
## 6. Shared Responsibility - Logging Routes

All team members should implement logging in their respective routes using the `save_log()` function. Additionally, the following log-specific routes should be implemented collaboratively:

### 6.1. Get Logs (Admin)

- **Route**: `GET /api/logs/`
- **Function**: `get_log_entries()`
- **Description**: Get system logs with filtering options
- **Example Response**:


```json
{
  "logs": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0e9",
      "log_type": "auth",
      "message": "User logged in successfully: testuser",
      "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
      "timestamp": "2023-11-16T01:30:45.123Z",
      "status": "info",
      "source": "auth_routes.login",
      "ip_address": "192.168.1.100"
    }
  ],
  "total": 1,
  "page": 1,
  "per_page": 50,
  "pages": 1
}
```
### 6.2. Clear Old Logs (Admin)

- **Route**: `POST /api/logs/clear`
- **Function**: `clear_logs()`
- **Description**: Clear logs older than specified days
- **Example Request Body**:


```json
{
  "days_to_keep": 30
}
```

- **Example Response**:


```json
{
  "message": "Successfully cleared logs older than 30 days",
  "deleted_count": 150
}
```
### 6.3. Get Log Statistics (Admin)

- **Route**: `GET /api/logs/stats`
- **Function**: `get_log_stats()`
- **Description**: Get statistics about system logs
- **Example Response**:


```json
{
  "log_types": [
    {
      "_id": "auth",
      "count": 250
    },
    {
      "_id": "file",
      "count": 180
    },
    {
      "_id": "device",
      "count": 75
    }
  ],
  "statuses": [
    {
      "_id": "info",
      "count": 450
    },
    {
      "_id": "warning",
      "count": 45
    },
    {
      "_id": "error",
      "count": 10
    }
  ],
  "recent_errors": [
    {
      "_id": "65a7b2c3d4e5f6a7b8c9d0ea",
      "log_type": "file",
      "message": "File upload failed - Fingerprint verification failed",
      "user_id": "a1b2c3d4-e5f6-7a8b-9c0d-e1f2a3b4c5d6",
      "timestamp": "2023-11-16T02:30:45.123Z",
      "status": "error",
      "source": "file_routes.upload_file",
      "ip_address": "192.168.1.100"
    }
  ],
  "total_logs": 505
}
```

---
## Implementation Guidelines

1. **Authentication**: Use JWT for authentication. All routes except registration and login require a valid JWT token.
2. **Authorization**: Implement role-based access control. Some routes are admin-only.
3. **Error Handling**: Provide clear error messages with appropriate HTTP status codes.
4. **Validation**: Validate all input data before processing.
5. **Logging**: Log all important operations using the `save_log()` function.
6. **Security**: Implement proper security measures, especially for fingerprint verification and file encryption.
7. **Testing**: Write tests for each route to ensure functionality.
## Database Schema

The application uses MongoDB with the following collections:

- users
- devices
- partitions
- files
- logs
- password_resets
## Getting Started

1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Set up environment variables in `.env` file
4. Run the application: `python app.py`
## API Authentication

Most endpoints require authentication. Include the JWT token in the Authorization header:

```json
Authorization: Bearer <your_jwt_token>
```

---
## Technologies and Libraries

### Core Technologies

1. **Python 3.8+**: The primary programming language used for backend development.
2. **Flask 2.0.3**: A lightweight WSGI web application framework for building the API.
3. **MongoDB 4.x+**: A NoSQL document database used for storing all application data.
### Python Libraries

#### Web Framework and Extensions

- **Flask-Cors 4.0.0**: Extension for handling Cross-Origin Resource Sharing (CORS).
- **Flask-JWT-Extended 4.5.3**: Extension for JSON Web Token (JWT) authentication.
- **Werkzeug 2.0.3**: WSGI utility library used by Flask for request/response handling.
#### Database

- **PyMongo 4.5.0**: MongoDB driver for Python, used for database operations.
- **python-dotenv 1.0.0**: Library for loading environment variables from .env files.
#### Security and Encryption

- **cryptography 41.0.4**: Library for secure encryption and decryption of files.
- **Werkzeug.security**: Used for password hashing and verification.
#### Image Processing (for Fingerprint)

- **Pillow 10.0.1**: Python Imaging Library fork, used for image processing.
- **NumPy 1.25.2**: Library for numerical computations, used for fingerprint data processing.
- **OpenCV-Python 4.8.0.76**: Computer vision library used for fingerprint image processing.
#### Utilities

- **uuid**: For generating unique identifiers.
- **datetime**: For timestamp handling.
- **json**: For JSON serialization/deserialization.
- **os**: For file system operations.
- **tempfile**: For creating temporary files during file operations.
- **re**: For regular expression operations in validation.
### Development Tools

- **Git**: Version control system.
- **pip**: Package installer for Python.
- **Virtual Environment**: For isolating project dependencies.
### Security Features

- **JWT Authentication**: For secure user authentication.
- **Password Hashing**: Using Werkzeug's security module.
- **Fingerprint Authentication**: Custom implementation using OpenCV and NumPy.
- **File Encryption**: Using AES encryption from the cryptography library.
- **Role-Based Access Control**: Admin vs. regular user permissions.
- **Comprehensive Logging**: For security auditing and monitoring.
### API Design

- **RESTful Architecture**: Following REST principles for API design.
- **Blueprint Pattern**: Using Flask blueprints for modular route organization.
- **JSON Responses**: Standardized JSON response format.
- **HTTP Status Codes**: Proper use of HTTP status codes for different scenarios.