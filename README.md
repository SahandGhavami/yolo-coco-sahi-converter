# YOLO ↔ COCO Annotation Converter with Image Slicing

A comprehensive Jupyter notebook tool for converting between YOLO and COCO annotation formats, with integrated image slicing capabilities using SAHI (Slicing Aided Hyper Inference).

## 🎯 Overview

This tool provides a complete pipeline for:
1. **YOLO → COCO Conversion**: Convert YOLO format annotations to COCO JSON format
2. **Image Slicing**: Slice large images and annotations into smaller tiles using SAHI
3. **COCO → YOLO Conversion**: Convert sliced COCO annotations back to YOLO format

Perfect for preparing datasets for object detection tasks, especially when working with large images that need to be split into smaller patches for training.

## ✨ Features

- ✅ **Bidirectional Conversion**: Seamlessly convert between YOLO and COCO formats
- ✅ **Smart Image Slicing**: Use SAHI to intelligently slice images with configurable overlap
- ✅ **Preserve Annotations**: Maintain bounding box accuracy during slicing and conversion
- ✅ **Batch Processing**: Handle entire datasets efficiently
- ✅ **Configurable Parameters**: Customize slice dimensions, overlap ratios, and more
- ✅ **Negative Sample Control**: Option to include or exclude slices without annotations

## 📋 Requirements

```bash
pip install sahi pillow matplotlib
```

### Dependencies
- Python 3.7+
- SAHI (Slicing Aided Hyper Inference)
- Pillow (PIL)
- Matplotlib

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/SahandGhavami/yolo-coco-sahi-converter.git
cd yolo-coco-sahi-converter
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Open the Notebook

```bash
jupyter notebook Converter.ipynb
```

## 📖 Usage Guide

### Step 1: YOLO to COCO Conversion

Configure your data directory and class mapping:

```python
# Set your dataset path
DATA_DIR = '/path/to/your/dataset'

# Define your class mapping
CLASS_MAPPING = {
    0: 'Small Craft', 
    1: 'Small Fishing Boat', 
    2: 'Small Passenger Ship',
    # ... add your classes
}
```

Run the conversion:

```python
coco_dataset = create_coco_dataset()
```

This generates a `boat_annotations_coco.json` file in COCO format.

### Step 2: Slice Images and Annotations

Use SAHI to slice your images and COCO annotations:

```python
coco_dict, coco_path = slice_coco(
    coco_annotation_file_path="boat_annotations_coco.json",
    image_dir="path/to/images",
    output_coco_annotation_file_name="sliced_coco.json",
    output_dir="path/to/output",
    slice_height=640,
    slice_width=640,
    overlap_height_ratio=0.2,
    overlap_width_ratio=0.2,
    ignore_negative_samples=False,
    verbose=True
)
```

**Parameters:**
- `slice_height/width`: Dimensions of each slice (pixels)
- `overlap_height/width_ratio`: Overlap between adjacent slices (0.0-1.0)
- `ignore_negative_samples`: Skip slices without annotations if `True`

### Step 3: Convert Sliced COCO back to YOLO

Convert the sliced COCO annotations back to YOLO format:

```python
from sahi.utils.coco import Coco

coco = Coco.from_coco_dict_or_path(
    "sliced_coco.json", 
    image_dir="path/to/sliced/images"
)

coco.export_as_yolo(
    output_dir="output/yolo/annotations",
    disable_symlink=True
)
```

## 📁 Dataset Structure

### Input (YOLO Format)
```
dataset/
├── image1.jpg
├── image1.txt
├── image2.jpg
├── image2.txt
└── ...
```

**YOLO Annotation Format** (`.txt` files):
```
class_id x_center y_center width height
```
All coordinates are normalized (0.0-1.0).

### Output (COCO Format)
```json
{
  "images": [...],
  "annotations": [...],
  "categories": [...]
}
```

## 🎨 Example Use Cases

### Maritime Vessel Detection
The default configuration includes 12 boat/ship classes:
- Small Craft, Small Fishing Boat, Small Passenger Ship
- Fishing Trawler, Large Passenger Ship, Sailing Boat
- Speed Craft, Motorboat, Pleasure Yacht
- Medium Ferry, Large Ferry, High Speed Craft

### Custom Object Detection
Simply modify the `CLASS_MAPPING` dictionary to match your dataset classes.

## 🔧 Configuration Options

### YOLO to COCO Conversion
- `DATA_DIR`: Path to your dataset directory
- `CLASS_MAPPING`: Dictionary mapping class IDs to class names
- `output_file`: Name of the output COCO JSON file

### Image Slicing (SAHI)
- `slice_height`: Height of each slice in pixels (default: 640)
- `slice_width`: Width of each slice in pixels (default: 640)
- `overlap_height_ratio`: Vertical overlap ratio (default: 0.2)
- `overlap_width_ratio`: Horizontal overlap ratio (default: 0.2)
- `ignore_negative_samples`: Whether to skip slices without objects (default: False)

## 📊 Output Information

The notebook provides detailed statistics:
- ✅ Number of images processed
- ✅ Total annotations created
- ✅ Categories defined
- ✅ Sample annotation preview
- ✅ Category summary with supercategories

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📜 Attribution

The YOLO to COCO conversion code (Step 1) is adapted from the Kaggle notebook ["YOLO to COCO" by Raghav Dharwal](https://www.kaggle.com/code/raghavdharwal/yolo-to-coco), licensed under Apache 2.0.

**Original Author:** Raghav Dharwal  
**Original Source:** https://www.kaggle.com/code/raghavdharwal/yolo-to-coco  
**License:** Apache License 2.0

Modifications and additions include:
- Enhanced documentation and user instructions
- Integration with SAHI for image slicing
- Complete bidirectional conversion pipeline (YOLO → COCO → Sliced → YOLO)
- Additional utility functions and error handling

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

**Note:** The YOLO to COCO conversion portion retains its original Apache 2.0 license from the source material.

## 🙏 Acknowledgments

- [Raghav Dharwal](https://www.kaggle.com/raghavdharwal) - Original YOLO to COCO conversion implementation
- [SAHI](https://github.com/obss/sahi) - Slicing Aided Hyper Inference library
- YOLO and COCO format specifications

## 📧 Contact

For questions or issues, please open an issue on GitHub.

## 🔗 Related Projects

- [SAHI](https://github.com/obss/sahi) - Slicing Aided Hyper Inference
- [Ultralytics YOLO](https://github.com/ultralytics/ultralytics) - YOLOv8 implementation
- [COCO API](https://github.com/cocodataset/cocoapi) - COCO dataset tools

---
