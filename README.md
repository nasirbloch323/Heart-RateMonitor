# Heart-Rate-Monitor
monitoring heart rate in real-time using a webcam.This is based on an algorithm called Eulerian Video Magnification which makes it possible to see the colours of the face change as blood rushes in and out of the head. It is able to detect the pulses and calculates the heart rate in beats per minute (BPM).This method performs well in real-time.
STEPS

Input: Webcam video feed as the input for heart rate measurement.

Preprocessing: Use MediaPipe (CVZone) to detect and localize the face region in the video frames.

Spatial Decomposition: Decompose the video frames into multiple spatial frequency bands using a pyramid-based approach.

Temporal Filtering: Apply band-pass filtering techniques to isolate the desired frequency range associated with the heartbeat.

Magnification: Amplify the subtle temporal variations related to the heartbeat for better visibility.

Measurement: Extract the amplified signal and estimate the heart rate in beats per minute (bpm) using appropriate signal processing techniques.

Visualize Results: Use CVZone LivePlot to visualize the heart rate estimation results.

                                       ┌───────────────────┐
                                       │       Input       │
                                       │   Webcam Video    │
                                       └───────────────────┘
                                                │
                                                v
                                       ┌───────────────────┐
                                       │   Preprocessing   │
                                       │   Face Region     │
                                       │    Detection      │
                                       └───────────────────┘
                                                │
                                                v
                                       ┌───────────────────┐
                                       │    Spatial        │
                                       │  Decomposition    │
                                       └───────────────────┘
                                                │
                                                v
                                       ┌───────────────────┐
                                       │    Temporal       │
                                       │   Filtering       │
                                       └───────────────────┘
                                                │
                                                v
                                       ┌───────────────────┐
                                       │   Magnification   │
                                       └───────────────────┘
                                                │
                                                v
                                       ┌───────────────────┐
                                       │    Measurement    │
                                       │   Heart Rate      │
                                       │   Estimation      │
                                       └───────────────────┘
                                                │
                                                v
                                       ┌───────────────────┐
                                       │    Visualize      │
                                       │    Results        │
                                       │   CVZone LivePlot │
                                       └───────────────────┘



                                       develop by 3 star 
sudo apt update
sudo apt install curl unzip -y

# Download and install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version






# Check version
python3 --version
pip3 --version

# Virtual Environment
python3 -m venv venv
source venv/bin/activate       # Linux/macOS
venv\Scripts\activate          # Windows

# Dependencies
pip install -r requirements.txt
pip freeze > requirements.txt





# Build & Run
docker build -t image_name:tag .
docker run -d -p 8080:8080 --name container_name image_name:tag

# Management
docker ps
docker ps -a
docker images
docker logs -f container_name
docker exec -it container_name sh

# Cleanup
docker stop container_name
docker rm container_name
docker rmi image_name:tag
docker system prune -a --volumes




