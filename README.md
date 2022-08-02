# Pothole-and-Obstacle-Detector
This system identifies the pothole and obstacle on the road in real time system. Obstacles on the road includes trees, cars and people.

1. Run a docker container and cd to jetson-inference directory.
cd jetson-inference

2. Run docker/run.sh on docker to change to root user.

3. Use the docker run script and cd to python/training/detection/ssd. In this directory, all PyTorch scripts and utilities are located.
cd python/training/detection/ssd

4. Use camera-capture tool to capture the custom dataset. It has features to detect the datasets and bounding boxes as well.
camera-capture /dev/video0

5. Change the Dataset Type in Data Capture Control to detection. Fill up the Dataset Path and Class Labels to its respective browser. Capture pictures for train, validate and test by changing the Current Set dropdown arrow.

6. In the .txt file, label pothole and obstacle.

7. Start capturing the data by click on Freeze/Edit button. Create the bounding box on the picture once it is freeze. The data will be saved automatically on Unfreeze the camera.

8. Train the dataset with suitable epoch.
python3 train_ssd.py --dataset-type=voc --data=data/pothole --model-dir=models/pothole --batch-size=2 --workers=1 --epochs=30

9. Export the trained dataset from PyTorch to onnx
python3 onnx_export.py --model-dir=models/pothole

10. Test the data on USB Camera
detectnet --model=models/pothole/ssd-mobilenet.onnx --labels=models/pothole/labels.txt --input-blob=input_0 --output-cvg=scores --output-bbox=boxes /dev/video0
