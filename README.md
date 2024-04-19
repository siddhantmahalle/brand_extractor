# Brand Logo Detection and Extraction

This project is a brand logo detection and brand name extraction pipeline in videos. The pipeline takes in a video and provides a CSV with timestamps and brand names detected in the video. The pipeline is divided into 3 parts:

1. **Video Preprocessing**: Extracting frames from the video.
2. **Logo Detection**: Detecting the brand logos in the frames.
3. **Logo Name Extraction**: Extracting the brand names from the detected logos.

## Notebooks

This repo contains the following directories with notebooks for each part of the pipeline:
- [Video Preprocessing](notebooks/video_preprocessing)
- [Logo Detection](notebooks/logo_detection)
- [Logo Name Extraction](notebooks/logo_name_extraction)
- [Pipeline](notebooks/pipeline)


## Approach

## Video Preprocessing

The video is split into frames using OpenCV. Sample 5 frames per second from the video.

## Logo Detection

### Dataset:
Used [Logos in the Wild dataset](https://zenodo.org/records/5101018) for training the logo detection model. The dataset contains `110` brand logos and symbols with `8502` images. We split the data into `70-20-10` splits for training, validation, and testing.

### Model:
Used [Yolov8 model](https://docs.ultralytics.com/) with `yolov8m` pre-trained weights. The model was trained for `15` epochs with a batch size of `16` on `RTX 3070 Mobile GPU`. Trained two versions of the model `with logos and symbol` and `with logos only`. The model `with logos only` was used for inference.

### Inference:
The model was exported to [onnx](https://onnx.ai/) format and used for inference. The model was used to detect the logos in the frames.

## Logo Name Extraction
This required OCR (Optical Character Recognition) to extract the brand names from the detected logos.
### Model:
Used [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) for OCR.

### Post Processing:

- The cropped logo images were passed through the OCR model to extract the brand names. We added padding of `10` pixels to the cropped logo images to improve the OCR results.
- We eliminated the brand names with less than `3` characters. We used a clustering algorithm to group the brand names that had a similarity score of more than `0.8`. We took the name that had the most no of non punctuation characters in the cluster as the final brand name. 
- We removed the brand names that occurred more than 10% of the total brand name containing frames.


## Further Improvements:
- Improve data preprocessing to improve the training set for logo detection.
- Use a better clustering algorithm to group the brand names.
- Use a better OCR model to improve the brand name extraction.