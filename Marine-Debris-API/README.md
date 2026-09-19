# Marine Debris Detection API

Backend API for the **Underwater Marine Debris & Anomaly Detection using Side-Scan Sonar** system.

The API provides the backend layer for processing sonar images, running AI-based detection, validating detections, calculating confidence, and preparing results for visualization and reporting.

## Overview

The API is designed around the following detection pipeline:

```text
Side-Scan Sonar Image
        ↓
Image Preprocessing
        ↓
YOLO Object Detection
        ↓
Acoustic Shadow Verification
        ↓
Object Classification
        ↓
Confidence Scoring
        ↓
Geolocation
        ↓
GIS / Report Output
```

## Project Architecture

```text
api/
├── ...
├── ...
└── ...
```

> The exact file structure and endpoints should be updated based on the current implementation.

## Technology Stack

* **Python** — Backend development
* **FastAPI** — REST API framework
* **YOLOv8** — Object detection
* **OpenCV** — Image preprocessing
* **PostgreSQL** — Detection/result storage
* **Pydantic** — Request and response validation

## API Responsibilities

The API acts as the bridge between the AI detection pipeline and the frontend application.

### 1. Image Upload

Accepts side-scan sonar images for analysis.

### 2. Image Processing

Preprocesses sonar imagery before passing it to the detection model.

Typical processing may include:

* Noise reduction
* Image normalization
* Resizing
* Contrast enhancement

### 3. Object Detection

The detection model identifies potential underwater objects such as:

* Ghost nets
* Pipes
* Shipwrecks
* Other marine debris
* Anomalous objects

Each detection should contain information such as:

```json
{
  "class": "ghost_net",
  "confidence": 0.91,
  "bbox": [120, 85, 340, 260]
}
```

### 4. Shadow-Based Verification

The system can use acoustic shadow characteristics as an additional verification layer.

This helps distinguish potential man-made objects from:

* Natural rocks
* Seabed structures
* Sonar noise
* Other false positives

Low-confidence or ambiguous detections can be flagged for human review.

### 5. Geolocation

Where GPS/sonar metadata is available, detections can be associated with geographic coordinates.

Example:

```json
{
  "latitude": 12.9716,
  "longitude": 77.5946
}
```

### 6. GIS Integration

Detection results can be consumed by the frontend GIS interface to display detected objects on a map.

### 7. Report Generation

The backend can generate a structured summary containing:

* Detection ID
* Object type
* Confidence score
* Verification status
* Geographic coordinates
* Image reference
* Detection timestamp

## Running the API

Create and activate a Python virtual environment:

```bash
python -m venv venv
```

### Windows

```bash
venv\Scripts\activate
```

### Linux/macOS

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the FastAPI server:

```bash
uvicorn main:app --reload
```

The API should then be available at:

```text
http://127.0.0.1:8000
```

## API Documentation

FastAPI automatically provides interactive API documentation.

### Swagger UI

```text
http://127.0.0.1:8000/docs
```

### ReDoc

```text
http://127.0.0.1:8000/redoc
```

## Example Workflow

A typical client request follows this sequence:

```text
Frontend
   │
   │ Upload sonar image
   ▼
FastAPI
   │
   ▼
Preprocessing
   │
   ▼
YOLO Detection
   │
   ▼
Shadow Verification
   │
   ▼
Confidence Scoring
   │
   ▼
Geolocation
   │
   ├──────────────► PostgreSQL
   │
   ▼
Detection Response
   │
   ▼
React + Leaflet GIS Map
```

## Example Response

```json
{
  "status": "success",
  "detections": [
    {
      "class": "ghost_net",
      "confidence": 0.91,
      "verified": true,
      "latitude": 12.9716,
      "longitude": 77.5946
    }
  ]
}
```

## Error Handling

The API should return appropriate HTTP status codes.

| Status | Meaning               |
| ------ | --------------------- |
| `200`  | Request successful    |
| `201`  | Resource created      |
| `400`  | Invalid request       |
| `404`  | Resource not found    |
| `422`  | Validation error      |
| `500`  | Internal server error |

## Security Considerations

For deployment, consider:

* Input file validation
* File-size limits
* Authentication and authorization
* API rate limiting
* Secure database credentials
* Environment variables for secrets
* Validation of uploaded image formats

## Development

Run the API in development mode:

```bash
uvicorn main:app --reload
```

For production, configure an appropriate ASGI deployment setup and disable development-only settings.

## Integration

The API is intended to integrate with:

* **React frontend**
* **Leaflet.js GIS visualization**
* **YOLOv8 detection model**
* **OpenCV preprocessing pipeline**
* **PostgreSQL database**
* **Automated report generation**

## Project Goal

The API supports the overall goal of reducing the manual effort required to inspect large volumes of side-scan sonar imagery.

Instead of requiring survey teams to manually inspect every sonar frame, the system provides automated candidate detection, confidence scoring, physics-based verification, geolocation, and structured results for human review.

---

**Project:** Underwater Marine Debris & Anomaly Detection using Side-Scan Sonar
**Backend:** FastAPI + Python
**AI:** YOLOv8 + Computer Vision
**GIS:** React + Leaflet.js
**Database:** PostgreSQL