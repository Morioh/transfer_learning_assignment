# Signature Detection from Alusive Contract Images

## Problem Statement
At Alusive Africa, the agreement verification process relies heavily on confirming whether documents have been signed by individuals such as grant recipients, university officials, or Alusive leadership. 

The objective of this model is to automate the verification of signed documents, especially in scenarios where multiple documents need to be validated simultaneously.

## Dataset
The [dataset](https://drive.google.com/file/d/1Fnjy9BTNIW9Pgwt-stZbq6_VvZtXe_yH/view?usp=sharing) consists of 75 images of Alusive Africa agreements and documents, with a significant class imbalance: 68 signed document images and only 7 unsigned document images. Images were chosen instead of PDFs to better suit the model selected for this task.

## Pretrained Model Selection & Justification
The choice of pretrained models was influenced by their architecture and suitability for image classification tasks that require attention to fine-grained textures, such as distinguishing between signed and unsigned documents:

- **VGG16**: This model captures fine details due to its deep convolutional layers, making it suitable for recognizing texture details that differentiate signed from unsigned documents.
- **InceptionV3**: Known for efficient feature extraction and the ability to handle complex classification tasks, InceptionV3 uses modular scaling to capture patterns at different scales, enhancing classification capabilities.
- **EfficientNetB0**: With state-of-the-art accuracy and fewer parameters, this model optimizes depth, width, and resolution, offering computational efficiency without sacrificing performance.

## Fine-Tuning Process
Each model was initialized with pretrained ImageNet weights and fine-tuned with a custom top layer for binary classification:

- **Global Average Pooling Layer**: Reduces dimensions to mitigate overfitting.
- **Dense Layers**: Adds task-specific complexity.
- **Dropout Layers**: Prevents overfitting by randomly disabling neurons during training.

The models were trained with a learning rate tailored to each architecture, and the final few layers were unfrozen to enhance feature extraction specific to the dataset.

## Evaluation Metrics
The models were evaluated using the following metrics:

- **Accuracy**: Overall correctness of predictions.
- **Loss**: Binary cross-entropy loss for training and evaluation.
- **Precision**: Proportion of true positive predictions among all positive predictions.
- **Recall**: Ability of the model to detect all relevant cases.
- **F1 Score**: Harmonic mean of precision and recall for balanced performance assessment.

## Results

| Model        | Accuracy | Loss | Precision | Recall | F1 Score |
|--------------|----------|------|-----------|--------|----------|
| VGG16        | 100%     | Low  | High      | High   | High     |
| InceptionV3  | 100%     | Low  | High      | High   | High     |
| EfficientNetB0 | 100%  | Slightly Higher | High | High | High |

Both VGG16 and InceptionV3 achieved perfect accuracy and minimal loss, though the dataset's imbalance impacted training effectiveness. EfficientNetB0, while achieving perfect accuracy, showed a slight increase in test loss.

## Findings
The insufficient balance in the dataset significantly impacted evaluation metrics for all models. Fine-tuning with a lower learning rate did not affect results for VGG16 or InceptionV3. 

However, unfreezing layers in EfficientNetB0 led to a minor increase in loss, suggesting sensitivity to dataset variations.

Despite attempts at data augmentation, the models showed minimal improvement. Due to class imbalance, confusion matrix and classification report analysis indicated that the models were biased toward predicting a single class. 

Future iterations may benefit from collecting more unsigned document samples or applying other class-balancing techniques.

## Conclusion
The current model demonstrates the potential for signature detection but is limited by dataset imbalance hence the overfitting. 

Future efforts will focus on increasing unsigned document samples and exploring alternative model architectures and balancing techniques to improve robustness and generalizability.