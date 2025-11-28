# 🚗 Traffic Analysis & Vehicle Speed Estimation  
### YOLOv8 + ByteTrack + Perspective Transform (Bird’s-Eye-View)

This project performs **vehicle detection, tracking, type labeling, and real-world speed estimation** from a monocular traffic camera. It uses:

- **YOLOv8** for object detection  
- **ByteTrack** for multi-object tracking  
- **Supervision** for annotation + utilities  
- **OpenCV Perspective Transform** to convert camera view → Bird’s-Eye-View (BEV)  
- **Homography-based speed estimation** for accurate km/h values  
- **Polygon ROI filtering** for lane-specific measurement

## 🖼️ Visual Outputs

![Preview](image.png)

-	Bounding boxes with vehicle types
-	Trace lines showing motion history
-	Speed labels (km/h)
-	Valid lane polygon overlay
-	Exported annotated video

---

## 📌 Key Features

- ✔️ Vehicle detection (car, truck, bus, bike…)  
- ✔️ Multi-object tracking with persistent IDs  
- ✔️ Real-world speed estimation (km/h)  
- ✔️ Perspective → Bird’s-Eye transformation  
- ✔️ Road-lane polygon filtering  
- ✔️ Class-label display (`#3 car 78 km/h`)  
- ✔️ Bounding boxes, traces, annotations  
- ✔️ Export annotated output video  
- ✔️ Ready for multi-video batch processing  

---

## 🎥 How Speed Estimation Works

The core idea comes from mapping the camera’s **distorted trapezoidal road area** to a **rectangular BEV space**, where every pixel equals a fixed real-world distance.

### 1. **Define SOURCE points (camera view)**  
Four points manually selected around the drivable area. These form a **trapezoid** because of perspective.  
(Source diagram on page 2 of the PDF  [oai_citation:1‡NOTEBOOK PDF - Traffic - Computer Vision.pdf](sediment://file_00000000687c720690963fddaaf39cb0))

### 2. **Define TARGET rectangle (flat BEV space)**  
A clean rectangle representing real-world dimensions.

These form a **trapezoid** because of perspective.  
(Source diagram on page 2 of the PDF  [oai_citation:1‡NOTEBOOK PDF - Traffic - Computer Vision.pdf](sediment://file_00000000687c720690963fddaaf39cb0))

### 2. **Define TARGET rectangle (flat BEV space)**  
A clean rectangle representing real-world dimensions:

### 3. Compute homography matrix
- This computes a 3×3 projective warp matrix mapping : SOURCE → TARGET.

### 4. Transform each tracked vehicle point
Now each vehicle’s movement occurs in uniform meter-like coordinates, enabling accurate km/h estimation.

### 5. Compute speed
```
distance = |y_start − y_end|
time = frames / FPS
speed(km/h) = (distance / time) * 3.6
```


