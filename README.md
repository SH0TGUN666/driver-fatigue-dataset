# Driver Fatigue Detection Dataset (YOLOv11, Multi-feature Fusion)

This repository provides the metadata and usage instructions for the dataset used in our research:

> **A Comparative Study of Fatigue Detection Models Based on Temporal Feature Fusion**, submitted to IEEE Conference, 2025.

This dataset was collected to support real-time fatigue detection in drivers using multi-feature fusion methods and YOLOv11 object detection.

---

## 📦 Dataset Overview

- **Total Images**: 9751
- **Annotation Format**: YOLO format (class_id, x_center, y_center, width, height)
- **Image Format**: `.jpg`
- **Classes (7 total)**:
  - `open_eyes`, `close_eyes`
  - `yawn`, `mouth`
  - `head_low`, `head_normal`, `head_rise`
- **Complex Driving Scenarios Covered**:
  - Nighttime weak light (1133 images)
  - Strong daylight glare (2435 images)
  - Drivers wearing masks (663 images)
  - Drivers wearing sunglasses (661 images)
  - Passengers visible in cabin (3557 images)
  - Other occlusions: hair, hands, formal dress (334 images)

---

## 📁 Directory Structure

After extracting the ZIP archive, the dataset follows this structure:
driver - fatigue - dataset/  
├── train/  
│   ├── images/  
│   └── labels/  
├── val/  
│   ├── images/  
│   └── labels/  
└── test/  
    ├── images/  
    └── labels/  


- Each `.jpg` image has a corresponding `.txt` annotation file in YOLO format.
- Annotations follow the format:  
<class_id> <x_center> <y_center> <width> <height>



---

## 🔗 Download

The full dataset is available for download via Baidu Netdisk:

- **ZIP File**: `driver-fatigue-dataset.zip` (~1GB)
- **Download Link**: [https://pan.baidu.com/s/1_cXVKDS1Atz_vM73icm6Jw?pwd=6p17](https://pan.baidu.com/s/1_cXVKDS1Atz_vM73icm6Jw?pwd=6p17)
- **Access Code**: `6p17`

> Please unzip the file and preserve the directory structure before use.

---

## 📚 Citation

If you use this dataset in your work, please cite:

Yi Li, Changwen Fan, Le Yang, Yingqi Wang, Jung-Kuei Yang,
"A Comparative Study of Fatigue Detection Models Based on Temporal Feature Fusion",
IEEE Conference, 2025.


BibTeX:
```bibtex
@inproceedings{li2025fatigue,
  author={Yi Li and Changwen Fan and Le Yang and Yingqi Wang and Jung-Kuei Yang},
  title={A Comparative Study of Fatigue Detection Models Based on Temporal Feature Fusion},
  booktitle={Proc. IEEE Int. Conf. on Intelligent Transportation Systems (ITSC)},
  year={2025}
}
