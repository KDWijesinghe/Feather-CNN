# Resource-Constrained CNN for Edge Image Classification

This project was developed for the **EN3150 – Pattern Recognition** module at the University of Moratuwa.

The main objective of the project is to design and evaluate a lightweight Convolutional Neural Network (CNN) suitable for image classification on resource-constrained edge devices.

The **Fashion-MNIST** dataset was used for the experiments. Two custom CNN architectures were developed and compared with two lightweight pre-trained deep learning models:

- **Model A** – Standard CNN using normal 2D convolutions
- **Model B** – Lightweight CNN using depthwise separable convolutions
- **MobileNetV2**
- **EfficientNetB0**

The models were trained using **TensorFlow/Keras in Google Colab** and later converted to **TensorFlow Lite (TFLite)** for deployment and evaluation on a **Raspberry Pi**.

---

## Dataset

The project uses the **Fashion-MNIST** dataset.

Each image is a grayscale clothing image with a resolution of:

```text
28 × 28 × 1
```

The dataset contains 10 classes.

For the custom models, the original `28 × 28 × 1` images were used.

For MobileNetV2 and EfficientNetB0, the images were resized to:

```text
64 × 64 × 3
```

The grayscale images were converted to three channels in order to match the input format expected by the pre-trained models.

The dataset was divided into:

```text
Training    : 70%
Validation  : 15%
Testing     : 15%
```

---

## Model A – Standard CNN

Model A was designed using standard convolution layers.

Main structure:

```text
Input
 ↓
Conv2D
 ↓
MaxPooling
 ↓
Conv2D
 ↓
MaxPooling
 ↓
Flatten / Feature Processing
 ↓
Dense Layers
 ↓
10-Class Output
```

Model A provides a baseline for comparison with the lightweight Model B.

---

## Model B – Lightweight CNN

Model B was designed specifically for resource-constrained edge devices.

Instead of normal convolution operations, it mainly uses **Depthwise Separable Convolutions**.

These separate a standard convolution into:

```text
Depthwise Convolution
        ↓
Pointwise 1 × 1 Convolution
```

This significantly reduces the number of trainable parameters and computational requirements.

Model B contains approximately:

```text
5,539 trainable parameters
```

which is well below the project requirement of:

```text
100,000 parameters
```

---

## Pre-trained Lightweight Models

Two existing lightweight architectures were also evaluated.

### MobileNetV2

MobileNetV2 is designed for mobile and edge applications. It uses efficient convolution operations and inverted residual blocks to reduce computational cost while maintaining good classification performance.

### EfficientNetB0

EfficientNetB0 is a compact architecture that balances network depth, width and input resolution to obtain high classification accuracy with relatively efficient computation.

---

# Training and Deployment Workflow

```text
Fashion-MNIST
      ↓
Data Preprocessing
      ↓
Train Models in Google Colab
      ↓
Evaluate Models
      ↓
Convert Models to TensorFlow Lite
      ↓
Transfer .tflite Models to Raspberry Pi
      ↓
Run Inference on Raspberry Pi CPU
      ↓
Measure
 ├── Accuracy
 ├── Inference Latency
 ├── Peak RAM
 └── Model Size
```

---

# Raspberry Pi Deployment

All four trained models were converted to the `.tflite` format and executed on the Raspberry Pi using TensorFlow Lite.

The Raspberry Pi experiment used:

```text
100 test images
Batch size = 1
CPU inference
TensorFlow Lite XNNPACK delegate
```

For Model A and Model B, the input shape was:

```text
1 × 28 × 28 × 1
```

For MobileNetV2 and EfficientNetB0, the input shape was:

```text
1 × 64 × 64 × 3
```

---

# Raspberry Pi Results

<table>
<tr>
<td align="center">
<img src="images/modelA.jpg" width="230">
</td>

<td align="center">
<img src="img/modelB.png" width="230">
</td>

<td align="center">
<img src="img/mobv2.png" width="230">
</td>

<td align="center">
<img src="img/effb0.png" width="230">
</td>
</tr>

<tr>
<td align="center"><b>Model A</b></td>
<td align="center"><b>Model B</b></td>
<td align="center"><b>MobileNetV2</b></td>
<td align="center"><b>EfficientNetB0</b></td>
</tr>
</table>

---

## Target-Device Comparison

| Model | Accuracy* | Mean Inference Latency | Peak RAM | TFLite Model Size |
|---|---:|---:|---:|---:|
| **Model A** | 90% | **0.0575 ms/image** | 58.62 MiB | 0.408 MB |
| **Model B** | 85% | **0.0614 ms/image** | **40.61 MiB** | **0.027 MB** |
| **MobileNetV2** | 93% | 1.9515 ms/image | 63.09 MiB | 8.523 MB |
| **EfficientNetB0** | **94%** | 4.2518 ms/image | 77.59 MiB | 15.362 MB |

> **Note:** The accuracy values in this table correspond to the 100-image Raspberry Pi deployment subset. Full test-set accuracy is evaluated separately in the training notebook.

# Main Trade-Off

The experiment demonstrates that no single architecture is best for every requirement.

| Priority | Suitable Model |
|---|---|
| Minimum storage | **Model B** |
| Minimum RAM usage | **Model B** |
| Very low computational requirement | **Model B** |
| Higher custom-CNN accuracy | **Model A** |
| Good accuracy with moderate edge requirements | **MobileNetV2** |
| Highest accuracy among tested models | **EfficientNetB0** |

The custom lightweight Model B provides the strongest resource-efficiency characteristics, while the larger pre-trained models provide higher classification accuracy at the cost of greater memory, storage and inference time.


**EN3150 – Pattern Recognition**
