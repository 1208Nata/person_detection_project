# Person Detection System (staff & student recognition)
## Introduction 
This project is an **AI-powered person detection system** that can identify whether a person in front of the camera is a **staff member** or a **student**.
It was built using NVIDIA Jetson Nano and the jetson-inference library, with a custom-trained SSD-Mobilenet model.

The idea came from the need to **improve safety and security** in schools.
With such a system, schools can:
* Automatically detect people entering the building.
* Differentiate between staff, students, and unknown individuals.
* React faster in case of unauthorized access.
___
## Project Goals 
* Train a **custom object detection model** to recognize two categories: staff and student.
* Deploy the model in real time using the Jetson Nano camera.
* Provide a step-by-step guide so others can reproduce the project.
___
## Technologies Used 
* **NVIDIA Jetson Nano**
* **Python**
* **Jetson-Inference** library
* **SSD-Mobilenet** architecture
* **ONNX** for model export
___
## Dataset Creation
1. Navigate to the dataset directory:
```
cd jetson-inference/python/training/detection/ssd/data
```
2. Create a new folder for your dataset:
```
mkdir person_detect_dataset
cd person_detect_dataset
```
3. Create a file called `labels.txt` with the following content:
```
staff
student
```
___
## Capturing Images 
![](https://ucarecdn.com/5d667588-9938-4e28-a3dc-142198fe589f/-/format/auto/-/preview/800x800/-/quality/lighter/figure%204.jpg)
1. Run the camera capture tool:
```
camera-capture /dev/video0
```
2. Make sure `Detection` mode is selected.
3. Under `Dataset path`, click the three dots and select your person_detect_dataset folder.
4. Under `Class Labels`, choose your `labels.txt` file.
5. Select the correct set you want to capture first (*staff* or *student*).
6. Draw a rectangle around the object in your scene and label it correctly.
7. Capture enough images for both classes.
___
## Training the Model
1. Open VS Code and enter the Jetson-Inference Docker container:
```
cd ~/jetson-inference/
./docker/run.sh
```
2. Go to the SSD training folder:
```
cd python/training/detection/ssd
```
3. Train the model:
```python
python3 train_ssd.py --dataset-type=voc \
                     --data=data/person_detect_dataset \
                     --model-dir=models/person_detect_model
```
4. Export the model to ONNX format:
```
python3 onnx_export.py --model-dir=models/person_detect_model
```
___
## Running Real-Time Detection 
Navigate to your model directory:
```
cd jetson-inference/python/training/detection/ssd/models/person_detect_model
```
and run:
```
detectnet 
  --model=ssd-mobilenet.onnx 
  --labels=labels.txt 
  --input-blob=input_0 
  --output-cvg=scores 
  --output-bbox=boxes 
  /dev/video0
```
___
## Demo Video
A short demo video is availible [here]()
___
**Project by**: Nata
**Platform**: NVIDIA Jetson Nano
**Date**: August 2025
