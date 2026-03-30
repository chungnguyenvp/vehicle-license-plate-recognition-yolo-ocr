# Vehicle License Plate Recognition using YOLO and PaddleOCR

A computer vision project for **vehicle detection**, **license plate detection**, and **license plate text recognition** from video using **YOLO**, **PaddleOCR**, and **OpenCV**.

## Project Overview

This project detects vehicles in video frames, locates their license plates, and recognizes plate text using OCR.  
The implementation is written in a Jupyter Notebook and uses two YOLO models:

- one model for **vehicle detection**
- one model for **license plate detection**

After detection, the plate region is preprocessed and passed into **PaddleOCR** for text extraction.

---

## Features

- Detect vehicles from video
- Detect license plates from vehicles
- Match each detected plate to the correct vehicle
- Recognize plate text using OCR
- Visualize results directly on video frames
- Simple notebook-based workflow for testing and demonstration

---

## Technologies Used

- Python
- OpenCV
- Ultralytics YOLO
- PaddleOCR
- Matplotlib
- Jupyter Notebook

---

## Project Structure

```bash
vehicle-license-plate-recognition-yolo-ocr/
│
├── .gitignore
├── README.md
├── requirements.txt
├── vehicle_plate_recognition.ipynb
│
├── models/
│   ├── vehicle.pt
│   └── bienso.pt
│
└── videos/
    └── sample.mp4
```

> Your notebook file is currently named `vehicle_plate_recognition.ipynb`.  
> If your sample video or model filenames are different, update the paths inside the notebook.

---

## How It Works

The pipeline in this project follows these steps:

### 1. Vehicle Detection
The system detects vehicles in each frame, including:
- bus
- car
- motor
- truck

### 2. License Plate Detection
A second YOLO model is used to detect the plate region.

### 3. Plate-to-Vehicle Matching
Each detected plate is assigned to the most suitable detected vehicle using geometric conditions such as:
- plate center inside the vehicle box
- plate appears in the lower region of the vehicle
- choosing the smallest valid containing vehicle

### 4. Plate Preprocessing
Before OCR, the cropped plate image is processed with:
- grayscale conversion
- image enlargement
- Gaussian blur
- Otsu thresholding

### 5. OCR Recognition
The processed plate image is passed to PaddleOCR to extract text.

### 6. Visualization
The final frame displays:
- vehicle bounding boxes
- plate bounding boxes
- vehicle class names
- recognized license plate text

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/vehicle-license-plate-recognition-yolo-ocr.git
cd vehicle-license-plate-recognition-yolo-ocr
```

### 2. Create a virtual environment

```bash
python -m venv venv
```

Activate it:

**Windows**
```bash
venv\Scripts\activate
```

**Linux / macOS**
```bash
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

## Requirements

A typical `requirements.txt` for this project:

```txt
opencv-python
matplotlib
ultralytics
paddleocr
paddlepaddle
jupyter
```

> Depending on your machine, you may need to install the correct version of `paddlepaddle` separately for CPU or GPU.

---

## Model Files

Place your model files inside the `models/` folder:

- `models/vehicle.pt` → vehicle detection model
- `models/bienso.pt` → license plate detection model

Make sure the model paths in the notebook match these filenames.

---

## Input Video

Place your test video in the `videos/` folder.

Example:

```bash
videos/sample.mp4
```

If your file has a different name, update the path inside the notebook.

Example:

```python
video_path = "videos/sample.mp4"
```

---

## Basic Configuration

The notebook uses settings similar to:

```python
vehicle_model_path = "models/vehicle.pt"
plate_model_path = "models/bienso.pt"
video_path = "videos/sample.mp4"

CLASS_NAMES = {
    0: "bus",
    1: "car",
    2: "motor",
    3: "truck"
}

VEHICLE_CONF = 0.30
PLATE_CONF = 0.15
PLATE_IMGSZ = 1280
```

### Parameter Notes

- `VEHICLE_CONF`: confidence threshold for vehicle detection
- `PLATE_CONF`: confidence threshold for plate detection
- `PLATE_IMGSZ`: image size used for plate detection

---

## How to Run

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open:

```bash
vehicle_plate_recognition.ipynb
```

Run all cells in order.

Typical usage in the notebook:

```python
vehicle_model, plate_model, ocr = load_models(
    vehicle_model_path,
    plate_model_path
)

run_video_live(
    video_path,
    vehicle_model,
    plate_model,
    ocr
)
```

---

## Main Functions

### `load_models(...)`
Loads the YOLO vehicle model, YOLO plate model, and PaddleOCR engine.

### `detect_vehicles(frame, vehicle_model)`
Detects vehicles in a frame.

### `detect_plates(frame, plate_model)`
Detects license plates in a frame.

### `assign_plate_to_vehicle(plate_box, vehicles)`
Assigns a plate to the correct vehicle using geometric rules.

### `match_plates_to_vehicles(vehicles, plates)`
Matches all detected plates with detected vehicles.

### `preprocess_plate(plate_img)`
Preprocesses the cropped plate region before OCR.

### `clean_plate_text(text)`
Cleans OCR results by:
- converting text to uppercase
- removing spaces
- keeping valid characters only

### `ocr_plate_from_box(frame, plate_box, ocr)`
Crops the plate image, preprocesses it, and applies OCR.

### `draw_results(frame, vehicles)`
Draws bounding boxes and recognized text on the frame.

### `process_frame(frame, vehicle_model, plate_model, ocr)`
Runs the complete detection and OCR pipeline for one frame.

### `run_video_live(...)`
Processes the entire video stream frame by frame.

---

## Output

The system shows:
- vehicle bounding boxes
- plate bounding boxes
- recognized plate text
- vehicle class labels

Example label format:

```txt
car | 51H12345
motor | 59X2-45678
truck | no_plate
```

---

## Limitations

- The project currently runs as a notebook, not a full packaged application
- OCR performance depends on image quality, blur, angle, and lighting
- No object tracking across frames
- Plate matching is rule-based

---

## Future Improvements

- Export results to CSV or JSON
- Add vehicle tracking across frames
- Improve OCR post-processing using plate format rules
- Support webcam or RTSP stream
- Refactor notebook into Python modules
- Add demo images to the repository

---

## Repository Description Suggestion

You can use this short description on GitHub:

**Vehicle and license plate recognition from video using YOLO, PaddleOCR, and OpenCV.**

---

## Suggested Topics

Suggested GitHub topics:
- computer-vision
- yolo
- ocr
- paddleocr
- opencv
- license-plate-recognition
- vehicle-detection
- python
- jupyter-notebook

---

## Author

Your Name

---

## License

This project is for educational and research purposes.
