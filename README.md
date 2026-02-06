# 🌸 Iris Flower Classification - ML App

A fast, lightweight machine learning web application that classifies iris flowers using a pre-trained RandomForest model with FastAPI and Docker.

## Features

- ⚡ **Fast Inference**: Optimized RandomForest model with parallel processing
- 🎨 **Modern UI**: Responsive web interface with gradient design
- 📊 **Real-time Predictions**: Get confidence scores for each iris species
- 🐳 **Docker Support**: Easy deployment with Docker and Docker Compose
- 🔧 **Production Ready**: Includes health checks and proper error handling

## Prerequisites

- Docker & Docker Compose (for containerized deployment)
- OR Python 3.10+ (for local development)

## Quick Start with Docker

### Option 1: Using Docker Compose (Easiest)

```bash
# Build and run the application
docker-compose up --build

# Application will be available at http://localhost:8000
```

### Option 2: Using Docker Directly

```bash
# Build the image
docker build -t iris-ml-app .

# Run the container
docker run -p 8000:8000 iris-ml-app
```

## Local Development

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run the Application

```bash
python main.py
```

Access the app at: **http://127.0.0.1:8000**

## Usage

1. Open your browser and navigate to the application URL
2. Enter iris flower measurements:
   - Sepal Length (cm)
   - Sepal Width (cm)
   - Petal Length (cm)
   - Petal Width (cm)
3. Click "Predict" to see the classification result with confidence scores

## Example Input Values

- **Setosa**: 5.0, 3.4, 1.5, 0.2
- **Versicolor**: 6.0, 2.7, 5.1, 1.6
- **Virginica**: 7.5, 4.0, 6.7, 2.0

## API Endpoints

### GET /
Returns the web interface (HTML)

### POST /predict
Predicts iris species from flower measurements

**Request Body:**
```json
{
  "sepal_length": 5.1,
  "sepal_width": 3.5,
  "petal_length": 1.4,
  "petal_width": 0.2
}
```

**Response:**
```json
{
  "class": "setosa",
  "class_index": 0,
  "probabilities": {
    "setosa": 0.95,
    "versicolor": 0.04,
    "virginica": 0.01
  }
}
```

## Project Structure

```
.
├── main.py                 # FastAPI application
├── requirements.txt        # Python dependencies
├── Dockerfile             # Docker container configuration
├── docker-compose.yml     # Docker Compose configuration
├── .dockerignore          # Files to exclude from Docker build
└── static/
    ├── index.html         # Web interface
    └── style.css          # Styling
```

## Docker Tips

### View Logs
```bash
docker-compose logs -f
```

### Stop the Container
```bash
docker-compose down
```

### Rebuild After Code Changes
```bash
docker-compose up --build
```

### Access Container Shell
```bash
docker-compose exec ml-app bash
```

## Performance Optimizations

- **RandomForest with 100 trees**: Balanced accuracy vs speed
- **Feature Scaling**: StandardScaler for consistent predictions
- **Parallel Processing**: Uses all available CPU cores
- **Lightweight Dependencies**: Minimal Python packages
- **Python:3.11-slim**: Smaller base image (~130MB)

## Machine Learning Model

- **Algorithm**: RandomForestClassifier (100 estimators)
- **Dataset**: Scikit-learn Iris Dataset (150 samples, 3 classes)
- **Features**: 4 (Sepal Length, Sepal Width, Petal Length, Petal Width)
- **Classes**: Setosa, Versicolor, Virginica
- **Preprocessing**: StandardScaler normalization

## System Requirements

- **Docker**: 4GB RAM recommended
- **CPU**: 1+ cores (more cores = faster inference)
- **Disk**: ~500MB for Docker image

## Troubleshooting

### Port Already in Use
If port 8000 is in use, modify `docker-compose.yml`:
```yaml
ports:
  - "8001:8000"  # Map to different port
```

### Permission Denied Error (Linux/Mac)
```bash
sudo docker-compose up --build
```

### Slow Predictions
- Check available CPU cores
- Increase Docker memory allocation
- Close other resource-intensive applications

## Development

### Make Changes to Static Files
Changes to `static/` are automatically reflected (volume mounted)

### Update Model
Edit `main.py` to change model parameters or training data

### Modify Dependencies
Update `requirements.txt` and rebuild:
```bash
docker-compose up --build
```

## License

Open source - Feel free to use and modify!

## Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest improvements
- Add new features
- Improve documentation

---

**Created with ❤️ for ML enthusiasts**
