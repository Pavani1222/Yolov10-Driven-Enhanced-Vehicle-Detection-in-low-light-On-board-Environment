# Vehicle Detection Project

A deep learning-based vehicle and object detection application using YOLOv10 with a Flask web interface. This project detects multiple classes including vehicles, pedestrians, animals, and road hazards.

## Features

- **Real-time Object Detection**: Uses YOLOv10 pre-trained model for fast and accurate detection
- **Web Interface**: Flask-based web application for easy interaction
- **Image Upload**: Upload and process images through the web interface
- **User Authentication**: Login and registration system for secure access
- **Multiple Detection Classes**: Detects 12 different object classes
- **Results Visualization**: View detection results with bounding boxes and confidence scores

## Supported Classes

The model detects the following 12 classes:
- Bicycle
- Bus
- Car
- Cat
- Dog
- Light
- Motorcycle
- Person
- Pothole
- Sign
- Tree
- Truck

## Project Structure

```
VehicleDetectionProject/
├── app.py                      # Flask application
├── best.pt                     # Trained YOLO model weights
├── yolov10n.pt                # Pre-trained YOLOv10 nano model
├── data.yaml                   # Dataset configuration
├── templates/                  # HTML templates
│   ├── home.html
│   ├── index.html
│   ├── login.html
│   ├── register.html
│   ├── result.html
│   ├── video_result.html
│   └── ...
├── static/                     # Static files (CSS, JS, images)
│   ├── css/
│   ├── js/
│   ├── img/
│   └── font-awesome-4.7.0/
├── train/                      # Training dataset
│   ├── images/
│   └── labels/
├── valid/                      # Validation dataset
│   ├── images/
│   └── labels/
├── test/                       # Test dataset
│   ├── images/
│   └── labels/
├── uploads/                    # User uploaded images
├── results/                    # Detection results
└── runs/                       # Training run logs
```

## Requirements

- Python 3.8+
- Flask
- Ultralytics YOLO
- OpenCV (cv2)
- NumPy
- Werkzeug

## Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/VehicleDetectionProject.git
   cd VehicleDetectionProject
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Download required model files**
   - Ensure `best.pt` (trained weights) is in the project root
   - Optionally download `yolov10n.pt` for inference

## Usage

### Run the Flask Application

```bash
python app.py
```

The application will start on `http://localhost:5000`

### Web Interface

1. **Register/Login**: Create an account or log in with existing credentials
2. **Upload Image**: Navigate to the detection page and upload an image
3. **View Results**: The detection results with bounding boxes will be displayed
4. **Download**: Save the annotated image with detections

### Command Line Detection

```python
from ultralytics import YOLO

# Load model
model = YOLO('best.pt')

# Predict on image
results = model.predict(source='path/to/image.jpg')

# Display results
for result in results:
    print(result.boxes)
```

## Model Information

- **Architecture**: YOLOv10
- **Framework**: Ultralytics
- **Input Size**: 640x640 pixels
- **Classes**: 12 object classes
- **Performance**: Optimized for speed and accuracy

## Dataset

The project uses a custom dataset containing:
- **Training Set**: Images with vehicle and object annotations
- **Validation Set**: Images for model validation
- **Test Set**: Images for testing model performance

Dataset format follows YOLO annotation standards with:
- Images in `images/` folders
- Labels in `labels/` folders (one `.txt` file per image)

## Training

To train the model on your dataset:

```python
from ultralytics import YOLO

# Load a pretrained model
model = YOLO('yolov10n.pt')

# Train the model
results = model.train(
    data='data.yaml',
    epochs=100,
    imgsz=640,
    batch=16,
    device=0  # GPU device
)
```

Training results and weights will be saved in the `runs/` directory.

## Results

- Training metrics: stored in `runs/detect/train/results.csv`
- Best model weights: `runs/detect/train/weights/best.pt`
- Training logs: TensorBoard events in `runs/detect/train/`

## API Routes

### Authentication
- `GET /` - Home page
- `POST /register` - User registration
- `POST /login` - User login
- `GET /logout` - User logout

### Detection
- `GET /index` - Detection page
- `POST /detect` - Process uploaded image for detection
- `GET /results/<filename>` - View detection results

### Other Pages
- `GET /products` - Products page
- `GET /about` - About page
- `GET /contact` - Contact page
- `GET /performance` - Model performance metrics

## Configuration

Edit `app.py` to modify:
- Upload folder path
- Results folder path
- Model weights file
- Flask secret key

## Performance Tips

1. Use GPU for faster inference: `model = YOLO('best.pt').cuda()`
2. Batch processing for multiple images
3. Adjust confidence threshold for filtering weak detections
4. Use model quantization for deployment

## Troubleshooting

**Issue**: Model weights file not found
- Solution: Ensure `best.pt` exists in the project root

**Issue**: CUDA not available
- Solution: Install PyTorch with CUDA support or use CPU mode

**Issue**: Port 5000 already in use
- Solution: Change Flask port in `app.py` or use `flask run --port 8000`

## Future Enhancements

- [ ] Real-time video detection via webcam
- [ ] Batch processing capabilities
- [ ] Model deployment on cloud services
- [ ] Mobile application support
- [ ] Advanced analytics dashboard
- [ ] Multiple model support

## License

This project is provided for educational and research purposes.

## Author

Your Name / Organization

## Acknowledgments

- [Ultralytics YOLO](https://github.com/ultralytics/ultralytics)
- [Roboflow Dataset](https://roboflow.com)
- Flask community

## Contact

For questions or support, please create an issue in the repository.

---

**Last Updated**: May 2026


To find over 100k other datasets and pre-trained models, visit https://universe.roboflow.com

The dataset includes 7342 images.
Obstacles are annotated in YOLOv8 format.

The following pre-processing was applied to each image:
* Auto-orientation of pixel data (with EXIF-orientation stripping)
* Resize to 640x640 (Stretch)

The following augmentation was applied to create 3 versions of each source image:
* 50% probability of horizontal flip
* Randomly crop between 0 and 20 percent of the image
* Random rotation of between -15 and +15 degrees
