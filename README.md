# Resource-Constrained CNN for Edge Image Classification

This project was developed for the **EN3150 - Pattern Recognition** module.

The objective is to design and compare lightweight CNN models for image classification on resource-constrained edge devices.

The **Fashion-MNIST** dataset was used for classification.

## Models

Four models were evaluated:

- **Model A** - Standard CNN
- **Model B** - Lightweight CNN using Depthwise Separable Convolutions
- **MobileNetV2** - Pre-trained lightweight model
- **EfficientNetB0** - Pre-trained lightweight model

Model B was designed specifically for low-resource edge devices and contains only **5,539 trainable parameters**.

## Dataset

Fashion-MNIST contains 10 clothing classes.

Custom CNN input:

```text
28 × 28 × 1
