# Driver Fatigue Detection Dataset

驾驶员疲劳检测数据集：9,751 张模拟车内视角图像，7 类 YOLO 标注（眼部、嘴部、头部姿态），用于 YOLO11 多特征疲劳检测研究（IEEE ICEICT 2025）。

A public set of **9,751** JPEG images for vision-based driver fatigue detection. Each image is a simulated in-vehicle view with YOLO bounding boxes for **seven** facial and head-pose classes. The set was built for a comparative study of multi-feature fusion on top of an Ultralytics YOLO11 detector, published at IEEE ICEICT 2025.

The images and labels are distributed as `driver-fatigue-dataset.zip` (about 1 GB) on Baidu Netdisk. This repository documents the release: classes, splits, directory layout, label format, and citation.

| | |
| --- | --- |
| Images | 9,751 `.jpg` |
| Resolution | 640 × 640, as reported for the dataset |
| Labels | YOLO `.txt` (one object per line) |
| Classes | 7: eyes, mouth, head posture |
| Split | train 7,800 / val 976 / test 975 (described as 8:1:1) |
| Annotation | CVAT |
| Paper | [ICEICT 2025](https://doi.org/10.1109/iceict66683.2025.11160205), pp. 439–444 |
| Archive | `driver-fatigue-dataset.zip`, about 1 GB |
| License | [CC BY 4.0](LICENSE) |

## Paper

Yi Li, Changwen Fan, Le Yang, Yingqi Wang, Pao-Min Tu, and Jung-Kuei Yang (corresponding author).  
GuangDong-TaiWan College of Industrial Science & Technology, Dongguan University of Technology, GuangDong, China.

> **A Comparative Study on Fatigue Detection Models With Temporal Feature Fusion.**  
> 2025 IEEE 8th International Conference on Electronic Information and Communication Technology (ICEICT), Weihai, China, 26–28 July 2025, pp. 439–444.  
> DOI: [10.1109/iceict66683.2025.11160205](https://doi.org/10.1109/iceict66683.2025.11160205) · IEEE Xplore: [11160205](https://ieeexplore.ieee.org/document/11160205/)

The study trains YOLO11 to detect eye state, mouth state, and head posture, then compares three ways of turning those detections into a fatigue decision:

1. **Rule-based threshold fusion.** The paper marks fatigue when any two of the three feature groups exceed their thresholds (Table I). Eye indicators are PERCLOS above 30% and a blink rate under 15 per minute. Mouth indicators are a yawning frequency above 3 per minute, or a single yawn longer than 0.8 seconds. A downward head pose counts as fatigue when the deviation exceeds 15° and that pose occurs more than twice per minute.
2. **Weighted sliding-window scoring.** At 30 FPS, statistics are taken over a window of 30–60 frames (about 1–2 seconds). The paper uses the score S = 0.5 × PERCLOS + 0.3 × YFR + 0.2 × HFR, and raises an alert when S exceeds 0.6.
3. **YOLO11-LSTM temporal fusion.** Each frame is a 10-dimensional vector: the seven detection confidences, plus three binary visibility flags (eye occlusion, mouth occlusion, and combined occlusion). A single LSTM layer with hidden size 64 reads a 60-frame window. A fully connected layer and a sigmoid produce a fatigue probability; the paper triggers an alert above 0.6.

The paper trains and tests Ultralytics YOLO11 with PyTorch 2.0.1 and OpenCV, on Windows 11 with an Intel Core i9-12900K, an NVIDIA RTX 3090 (24 GB), and 64 GB DDR5 RAM. Images were resized to 640 × 640. The experimental section of the paper records the optimizer, learning rate, batch size, and epoch schedule.

## Classes

Label tokens follow Table I of the paper. Counts are from Table II and record how many images include that behavior. An image can carry more than one class, so the category rows can overlap.

Table II calls the `mouth` class **Mouth Closed**, `head_low` **Head Down**, `head_normal` **Normal Head Posture**, and `head_rise` **Head Up**.

| Class | Group | Train | Val | Test | Total |
| --- | --- | ---: | ---: | ---: | ---: |
| `open_eyes` | Eye | 5,004 | 639 | 614 | 6,257 |
| `close_eyes` | Eye | 2,023 | 235 | 237 | 2,495 |
| `yawn` | Mouth | 497 | 58 | 76 | 631 |
| `mouth` | Mouth | 6,601 | 819 | 812 | 8,232 |
| `head_low` | Head | 858 | 93 | 93 | 1,044 |
| `head_normal` | Head | 6,617 | 841 | 827 | 8,285 |
| `head_rise` | Head | 319 | 41 | 55 | 415 |
| **Images in the split** | | **7,800** | **976** | **975** | **9,751** |

Head-pose labels were assigned by visual inspection. YOLO11 detects all seven classes from image appearance.

## Driving conditions

The paper includes the following conditions so the detector sees common in-cabin interference. Figures are image counts for each condition.

| Condition | Images |
| --- | ---: |
| Low light | 1,133 |
| Strong light | 2,435 |
| Passengers in the cabin | 3,557 |
| Face mask | 663 |
| Sunglasses | 661 |
| Other occlusions (hair, hands, or clothing) | 334 |

The paper also names cabin distractions and varied camera angles. Table III compares this coverage with NTHU-DDD and DR(eye)VE on behavior categories, lighting, passengers, masks, sunglasses, and other occlusions.

## Files and label format

After unzipping, keep this layout:

```text
driver-fatigue-dataset/
├── train/
│   ├── images/
│   └── labels/
├── val/
│   ├── images/
│   └── labels/
└── test/
    ├── images/
    └── labels/
```

Each `.jpg` has a `.txt` file with the same stem in the matching `labels/` folder. The text files use YOLO detection format, one object per line:

```text
<class_id> <x_center> <y_center> <width> <height>
```

`class_id` is the integer index of one of the seven classes. The four box values are the center and the size of the box. In the YOLO convention used for these labels, they are normalized by image width and height and fall between 0 and 1.

To train, point a YOLO data config at `train/images`, `val/images`, and `test/images`, and list the seven class names in the same index order used in the label files. The paper’s detector is Ultralytics YOLO11, with input size 640 × 640.

## Download

The archive is on Baidu Netdisk:

- **File:** `driver-fatigue-dataset.zip` (about 1 GB)
- **Link:** https://pan.baidu.com/s/1_cXVKDS1Atz_vM73icm6Jw?pwd=6p17
- **Access code:** `6p17`

Unzip it and leave the `train`, `val`, and `test` folders intact.

## Results reported in the paper

The numbers in this section are the ones printed in the paper.

**YOLO11 detection.** On the validation set the paper reports mAP@0.5 of **0.9488**. For classification performance it reports precision **0.9245**, recall **0.9042**, and F1 **0.9142**. The confusion matrices show clear separation of open and closed eyes, some confusion between the normal head pose and the other head poses, and overlap between yawning and mouth closed.

**Fusion on a 3-minute video** (Table V). The clip runs at 30 FPS (about 5,400 frames) and was recorded by one subject with the camera angle aligned to the still-image set. The paper splits it into normal driving (0–60 s), mild fatigue (60–90 s), and severe fatigue (90–180 s), and computes the scores below from detections sampled at one frame per second. Delays are the values it states in seconds.

| Strategy | Accuracy | Precision | Recall | F1 | Delay |
| --- | ---: | ---: | ---: | ---: | ---: |
| Threshold fusion | 0.88 | 1.00 | 0.81 | 0.90 | ≈ 19.09 s |
| Sliding-window scoring | 0.52 | 1.00 | 0.23 | 0.37 | ≈ 16.36 s |
| YOLO11-LSTM | 0.94 | 0.96 | 0.94 | 0.95 | ≈ 0.23 s |

The abstract states the same YOLO11-LSTM headline: 94% accuracy, 96% precision, and a delay of 0.23 seconds.

## Citation

Please cite the paper if you use this dataset.

Yi Li, Changwen Fan, Le Yang, Yingqi Wang, Pao-Min Tu, and Jung-Kuei Yang, “A Comparative Study on Fatigue Detection Models With Temporal Feature Fusion,” in *2025 IEEE 8th International Conference on Electronic Information and Communication Technology (ICEICT)*, Weihai, China, 2025, pp. 439–444, doi: [10.1109/iceict66683.2025.11160205](https://doi.org/10.1109/iceict66683.2025.11160205).

```bibtex
@inproceedings{li2025fatigue,
  author    = {Yi Li and Changwen Fan and Le Yang and Yingqi Wang and Pao-Min Tu and Jung-Kuei Yang},
  title     = {A Comparative Study on Fatigue Detection Models With Temporal Feature Fusion},
  booktitle = {2025 IEEE 8th International Conference on Electronic Information and Communication Technology (ICEICT)},
  address   = {Weihai, China},
  pages     = {439--444},
  year      = {2025},
  doi       = {10.1109/iceict66683.2025.11160205},
  url       = {https://doi.org/10.1109/iceict66683.2025.11160205}
}
```

The paper’s own reference for this archive points at this repository: Y. Li, C. Fan, L. Yang, Y. Wang, and J.-K. Yang, “Driver fatigue detection dataset,” 2025. [Online]. Available: https://github.com/SH0TGUN666/driver-fatigue-dataset

## License

This repository is released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](LICENSE). You may share and adapt the material with appropriate credit. Cite the IEEE paper under the publisher’s terms.

## Sample images

Figure 1 of the paper shows example frames. A few representative samples can be added to this section later.

## Acknowledgment

The paper states that the study was supported by the Undergraduate Innovation and Entrepreneurship Training Program of Dongguan University of Technology (Grant No. S202311819121).
