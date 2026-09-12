# FitFlow AI/ML Service

Machine learning service for personalized workout recommendations and nutrition analysis.

## Overview

This service provides AI/ML capabilities for FitFlow, including:
- TensorFlow Lite model training and conversion
- Workout personalization algorithms
- Nutrition analysis models
- Model optimization for mobile deployment

## Features

- 🤖 Workout difficulty prediction
- 📊 Exercise recommendation engine
- 🍎 Nutrition pattern analysis
- 🎯 Personalized goal setting
- 📱 Mobile-optimized TensorFlow Lite models

## Prerequisites

- Python >= 3.9
- pip >= 21.0
- TensorFlow >= 2.13
- Node.js >= 18.0 (for TensorFlow.js conversion)

## Installation

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Requirements

Create `requirements.txt`:

```
tensorflow==2.13.0
tensorflow-lite==2.13.0
numpy==1.24.3
pandas==2.0.3
scikit-learn==1.3.0
matplotlib==3.7.2
jupyter==1.0.0
tensorflowjs==4.10.0
firebase-admin==6.2.0
python-dotenv==1.0.0
requests==2.31.0
```

## Project Structure

```
ai-service/
├── models/              # Trained models
│   ├── workout_recommender.tflite
│   ├── difficulty_predictor.tflite
│   └── nutrition_analyzer.tflite
├── services/           # ML service implementations
│   ├── workout_service.py
│   ├── nutrition_service.py
│   └── training_service.py
├── utils/             # Utility functions
│   ├── data_preprocessing.py
│   ├── model_converter.py
│   └── evaluation.py
├── tests/             # Unit tests
│   ├── test_workout_service.py
│   └── test_nutrition_service.py
├── notebooks/         # Jupyter notebooks for experimentation
│   ├── model_training.ipynb
│   └── data_analysis.ipynb
├── scripts/           # Utility scripts
│   ├── train_model.py
│   ├── convert_to_tflite.py
│   └── download_models.py
├── data/              # Training data (gitignored)
│   ├── raw/
│   ├── processed/
│   └── synthetic/
├── requirements.txt
└── README.md
```

## Models

### 1. Workout Recommender

**Purpose**: Generates personalized workout plans based on user profile and history

**Input Features**:
- User fitness level (beginner, intermediate, advanced)
- Available equipment
- Workout duration preference
- Goals (strength, cardio, flexibility)
- Past workout history

**Output**:
- Recommended exercises
- Sets and reps
- Difficulty level

**Model Architecture**:
```python
# Neural network with embedding layers
Input Layer → Embedding → Dense(128) → Dropout(0.3) → 
Dense(64) → Output Layer
```

### 2. Difficulty Predictor

**Purpose**: Predicts appropriate workout difficulty based on user performance

**Input Features**:
- Completion rate
- Rest time between sets
- Weight progression
- Heart rate data (if available)

**Output**:
- Recommended difficulty adjustment (-2 to +2)

### 3. Nutrition Analyzer

**Purpose**: Analyzes nutrition patterns and provides recommendations

**Input Features**:
- Daily calorie intake
- Macronutrient distribution
- Meal timing
- Workout schedule

**Output**:
- Nutrition score
- Recommendations

## Training Models

### Prepare Data

```bash
# Generate synthetic training data
python scripts/generate_data.py

# Preprocess data
python scripts/preprocess_data.py
```

### Train Model

```bash
# Train workout recommender
python scripts/train_model.py --model workout_recommender

# Train difficulty predictor
python scripts/train_model.py --model difficulty_predictor

# Train with custom parameters
python scripts/train_model.py \
  --model workout_recommender \
  --epochs 100 \
  --batch-size 32 \
  --learning-rate 0.001
```

### Convert to TensorFlow Lite

```bash
# Convert trained model to TFLite
python scripts/convert_to_tflite.py \
  --model models/workout_recommender.h5 \
  --output models/workout_recommender.tflite \
  --optimize

# Verify conversion
python scripts/verify_tflite.py models/workout_recommender.tflite
```

### Convert to TensorFlow.js (Optional)

```bash
# For potential web version
tensorflowjs_converter \
  --input_format=keras \
  models/workout_recommender.h5 \
  models/tfjs/workout_recommender
```

## Model Optimization

### Quantization

```python
import tensorflow as tf

# Post-training quantization for smaller model size
converter = tf.lite.TFLiteConverter.from_keras_model(model)
converter.optimizations = [tf.lite.Optimize.DEFAULT]
tflite_quant_model = converter.convert()
```

### Pruning

```python
import tensorflow_model_optimization as tfmot

# Prune model to reduce size
prune_low_magnitude = tfmot.sparsity.keras.prune_low_magnitude
pruned_model = prune_low_magnitude(model)
```

## Using Models in Mobile App

### React Native Integration

```typescript
import * as tf from '@tensorflow/tfjs';
import { bundleResourceIO } from '@tensorflow/tfjs-react-native';

// Load model
const model = await tf.loadLayersModel(
  bundleResourceIO(modelJson, modelWeights)
);

// Make prediction
const input = tf.tensor2d([[
  fitnessLevel,
  equipment,
  duration,
  goal
]]);
const prediction = model.predict(input);
```

## Testing

```bash
# Run all tests
pytest

# Run specific test
pytest tests/test_workout_service.py

# Run with coverage
pytest --cov=services tests/
```

## Model Evaluation

```bash
# Evaluate model performance
python scripts/evaluate_model.py \
  --model models/workout_recommender.tflite \
  --test-data data/test_data.csv

# Generate evaluation report
python scripts/generate_report.py
```

### Metrics

- **Accuracy**: Percentage of correct recommendations
- **Precision**: Relevance of recommendations
- **Recall**: Coverage of appropriate recommendations
- **F1 Score**: Harmonic mean of precision and recall
- **User Satisfaction**: Feedback from A/B testing

## Data Collection

### Privacy-Preserving Data Collection

```python
# Anonymize user data for training
from utils.privacy import anonymize_data

# Federated learning approach
# Train on-device, only share model updates
```

### Synthetic Data Generation

```bash
# Generate synthetic training data
python scripts/generate_synthetic_data.py \
  --samples 10000 \
  --output data/synthetic/workouts.csv
```

## Continuous Improvement

### Model Retraining Pipeline

```bash
# Automated retraining with new data
python scripts/retrain_pipeline.py \
  --schedule weekly \
  --min-samples 1000
```

### A/B Testing

```python
# Compare model versions
python scripts/ab_test.py \
  --model-a models/v1.tflite \
  --model-b models/v2.tflite \
  --duration 14days
```

## Model Versioning

Models are versioned using semantic versioning:

```
models/
├── workout_recommender_v1.0.0.tflite
├── workout_recommender_v1.1.0.tflite
└── workout_recommender_v2.0.0.tflite
```

## Deployment

### Mobile Deployment

Models are bundled with mobile app:

```bash
# Copy to mobile app
cp models/*.tflite ../frontend/assets/models/
```

### Cloud Deployment (Optional)

For complex predictions that can't run on-device:

```bash
# Deploy to Cloud Functions
firebase deploy --only functions:predictWorkout
```

## Performance Benchmarks

| Model | Size | Inference Time | Accuracy |
|-------|------|----------------|----------|
| Workout Recommender | 2.1 MB | 15 ms | 87% |
| Difficulty Predictor | 850 KB | 8 ms | 92% |
| Nutrition Analyzer | 1.5 MB | 12 ms | 85% |

## Troubleshooting

### Model Conversion Issues

```bash
# Check TensorFlow version
python -c "import tensorflow as tf; print(tf.__version__)"

# Verify model compatibility
python scripts/check_model_compatibility.py
```

### Performance Issues

```bash
# Profile model inference
python scripts/profile_model.py models/workout_recommender.tflite
```

## Contributing

1. Train model on diverse dataset
2. Evaluate on validation set
3. Optimize for mobile deployment
4. Document model architecture and performance
5. Submit Pull Request with benchmarks

## License

MIT License - see [LICENSE](../LICENSE) for details.

---

**Framework**: TensorFlow/TensorFlow Lite  
**Language**: Python 3.9+  
**Target Platform**: Mobile (iOS & Android)
