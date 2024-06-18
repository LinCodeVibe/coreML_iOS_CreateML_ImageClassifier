# Classifying Images with Vision and Core ML

# Core ML & Create ML Image Classifier

<img width="331" alt="Cat_Classification" src="https://github.com/LinCodeVibe/coreML_iOS_CreateML_ImageClassifier/assets/166785174/74622fed-cf52-45b5-83f1-951d1b683ed6">


Welcome to the Core ML & Create ML Image Classifier project! This project demonstrates how to build an image classification model using Core ML and Create ML, and how to integrate it into an iOS app with a view controller for selecting images and applying Vision + Core ML processing.

## Overview
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Project Setup](#project-setup)
- [Creating the Model](#creating-the-model)
- [Integrating the Model](#integrating-the-model)
  - [1. View Controller for Selecting Images and Applying Vision + Core ML Processing](#1-view-controller-for-selecting-images-and-applying-vision--core-ml-processing)
  - [2. Using the Swift Class `MobileNet` Generated by Core ML](#2-using-the-swift-class-mobilenet-generated-by-core-ml)
  - [3. Handling General Image Processing Errors](#3-handling-general-image-processing-errors)
  - [4. Updating the UI with Classification Results](#4-updating-the-ui-with-classification-results)
  - [5. Handling Classification Observations](#5-handling-classification-observations)
  - [6. Showing Options for the Source Picker](#6-showing-options-for-the-source-picker)
  - [7. Handling Image Picker Selection](#7-handling-image-picker-selection)
- [Contributing](#contributing)
- [License](#license)

## Getting Started

In this project, we will create an image classification model using Core ML and Create ML, and integrate it into an iOS app. The app will allow users to select images and apply the classification model to identify the contents of the images.

## Prerequisites

Before you begin, make sure you have the following prerequisites installed:
- code project runs on iOS 11.
- You can also use Vision and Core ML in your own apps on macOS 10.13, iOS 11, or tvOS 11.


## Project Setup

1. **Open Xcode**: Launch Xcode and create a new iOS project.

2. **Add the Core ML Model**: Download or create a Core ML model (e.g., `MobileNet.mlmodel`) and add it to your Xcode project.

3. **Import Required Frameworks**: Ensure you import the following frameworks in your project:
    ```swift
    import UIKit
    import CoreML
    import Vision
    import ImageIO
    ```

## Creating the Model

1. **Open Create ML**: Launch Create ML from Xcode by going to `Xcode > Open Developer Tool > Create ML`.

2. **Create a New Image Classifier**: Select `New Document` and choose `Image Classifier`.

3. **Import the Dataset**: Drag and drop your dataset into the training section and start the training process.

4. **Export the Model**: Once training is complete, export the model as a `.mlmodel` file and add it to your Xcode project.

## Integrating the Model

### 1. Set Up Vision with a Core ML Model + Core ML Processing

Use the Swift class `MobileNet` Core ML generates from the model.
             To use a different Core ML classifier model, add it to the project
             and replace `MobileNet` with that model's generated Swift class.

```swift
    lazy var classificationRequest: VNCoreMLRequest = {
        do {
           // let model = try VNCoreMLModel(for: MobileNet().model)
            let model = try VNCoreMLModel(for: MyImageClassifier_1_copy_3().model)
            
            let request = VNCoreMLRequest(model: model, completionHandler: { [weak self] request, error in
                self?.processClassifications(for: request, error: error)
            })
            request.imageCropAndScaleOption = .centerCrop
            return request
        } catch {
            fatalError("Failed to load Vision ML model: \(error)")
        }
    }()

```




## 2. Updates the UI with the results of the classification.
The `results` will always be `VNClassificationObservation`s, as specified by the Core ML model in this project.


```swift
 func processClassifications(for request: VNRequest, error: Error?) {
        DispatchQueue.main.async {
            guard let results = request.results else {
                self.classificationLabel.text = "Unable to classify image.\n\(error!.localizedDescription)"
                return
            }
            let classifications = results as! [VNClassificationObservation]

```







