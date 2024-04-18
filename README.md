# Brand Logo Detection and Extraction

This project is a brand logo detection and brand name extraction pipeline in videos. This takes in an video and provides a csv with timestamps and brand names detected in the video. The pipeline is divided into 3 parts:

1. **Video Preprocessing**: Extracting frames from the video.
2. **Logo Detection**: Detecting the brand logos in the frames.
3. **Logo Name Extraction**: Extracting the brand names from the detected logos.

## Notebooks

This repo contains the following directories with notebooks for each part of the pipeline:
- [Video Preprocessing](notebooks/video_preprocessing)
- [Logo Detection](notebooks/logo_detection)
- [Logo Name Extraction](notebooks/logo_name_extraction)
- [Pipeline](notebooks/pipeline)

## Installation 

1. Clone the repository
```bash
git clone 
```

2. Install the required packages
```bash
pip install -r requirements.txt
```

3. Download the pre-trained models
```bash
sh download_models.sh
```

## Usage

1. Run the pipeline
```bash
python main.py --video_path <path_to_video> --output_path <output_path_csv>
```

## Approach

## Video Preprocessing

The video is split into frames using OpenCV. Sample 1 frame per second from the video.

## Logo Detection

### Dataset:  
Used [Logos in the Wild dataset](https://zenodo.org/records/5101018) for training the logo detection model. The dataset contains `110` brand logos and symbols with `8502` images. We split the data into `70-20-10` split for training, validation, and testing.

### Model:
Used [Yolov8 model](https://docs.ultralytics.com/) with `yolov8m` pre-trained weights. The model was trained for `15` epochs with a batch size of `16` on `RTX 3070 Mobile GPU`. Trained two versions of the model `with logos and symbol` and `with logos only`. The model `with logos only` was used for inference.

### Inference:
The model was exported to [onnx](https://onnx.ai/) format and used for inference. The model was used to detect the logos in the frames.

## Logo Name Extraction

This required OCR (Optical Character Recognition) to extract the brand names from the detected logos. We used [Tesseract OCR]() for this purpose. The detected logos were cropped and passed through the OCR to extract the brand names.
