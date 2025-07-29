# Missing‑Frame‑Detection  
Detecting **dropped frames** in traffic‑surveillance videos with **YOLOv11 + DeepSORT**

> The project leverages the **temporal consistency of object tracks** (vehicles, pedestrians, etc.) to identify missing frames—useful for forensic analysis, accident reconstruction, and other *offline* scenarios.

---

## Requirements

| Package         | Suggested Version | Notes                                            |
| --------------- | ----------------- | ------------------------------------------------ |
| Python          | ≥ 3.8             | Create a dedicated *conda* or *venv* if possible |
| PyTorch         | ≥ 2.0             | GPU support highly recommended                   |
| OpenCV‑Python   | ≥ 4.8             | Reading / writing video frames                   |
| NumPy           | ≥ 1.24            | Basic numeric ops                                |
| **ultralytics** | latest            | `pip install ultralytics` — bundles YOLOv11      |

*Other minor deps (e.g. torchvision) will be auto‑installed by `ultralytics`. Install any missing packages exactly as prompted.*

---

## Repository Layout

```text
.
├── deep_sort/                # ⭐ Tracking core adapted from nwojke/deep_sort (see Acknowledgement)
├── faster_rcnn_IoU.ipynb     # Baseline: Faster‑RCNN + IoU continuity
├── rename.py                 # Batch‑rename frames to 00000.jpg, 00001.jpg, …
├── yolo11_deepsort.ipynb     # Main notebook: detection + tracking + drop detection
└── videos/                   # You create this folder for frame sequences
    ├── 000/                  # Video #1 → 00000.jpg, 00001.jpg, …
    ├── 001/                  # Video #2
    └── ...
```

### deep_sort/

- **Source**: https://github.com/nwojke/deep_sort
- Slightly refactored (weight paths, class filtering, etc.). Many thanks to the original authors!

### faster_rcnn_IoU.ipynb

- Traditional two‑stage detector (Faster‑RCNN) combined with IoU‑based continuity—used as a **baseline** for comparison.

### rename.py

- Turn arbitrary filenames into the required five‑digit ascending pattern (`00000.jpg`, `00001.jpg`, …) so frame order is correct.

### yolo11_deepsort.ipynb

1. Frame extraction (with optional down‑sampling).
2. YOLOv11 detection.
3. DeepSORT multi‑object tracking.
4. Timestamp continuity analysis → dropped‑frame verdicts.

------

## Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/<YOUR_NAME>/Missing-frame-detection.git
cd Missing-frame-detection

# 2. Install dependencies (ideally in a virtual env)
pip install ultralytics opencv-python numpy torch

# 3. Prepare data
mkdir -p videos/000
#   Put extracted frames in videos/000/ as 00000.jpg, 00001.jpg, ...

# 4. Run the notebook
jupyter notebook yolo11_deepsort.ipynb
#   👉 Execute cells sequentially to obtain dropped‑frame reports
```

------

## Citation & Acknowledgement

```text
@inproceedings{Wojke2017simple,
  title={Simple Online and Realtime Tracking with a Deep Association Metric},
  author={Wojke, Nicolai and Bewley, Alex and Paulus, Dietrich},
  booktitle={2017 IEEE International Conference on Image Processing (ICIP)},
  year={2017},
  pages={3645--3649},
  organization={IEEE},
  doi={10.1109/ICIP.2017.8296962}
}

@inproceedings{Wojke2018deep,
  title={Deep Cosine Metric Learning for Person Re-identification},
  author={Wojke, Nicolai and Bewley, Alex},
  booktitle={2018 IEEE Winter Conference on Applications of Computer Vision (WACV)},
  year={2018},
  pages={748--756},
  organization={IEEE},
  doi={10.1109/WACV.2018.00087}
}
```

- **YOLOv11**: https://github.com/ultralytics/ultralytics
- **DeepSORT**: https://github.com/nwojke/deep_sort

> **Disclaimer**: For research and educational use only. Do not deploy on private or commercial surveillance footage without proper consent. Issues and pull requests are welcome!