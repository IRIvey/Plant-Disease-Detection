Training and Evaluation Report

Model: EfficientNet-B0
Training Type: From scratch
Input Image Size: 224 × 224

1. Overview

This experiment trained an EfficientNet-B0 model from scratch for image classification using input images resized to 224 × 224. The model was trained for 30 epochs and evaluated using loss, accuracy, precision, recall, and F1-score. In addition, inference speed was measured to assess prediction efficiency.

2. Final Test Performance

The final model achieved strong performance on the test set:

Test Loss: 0.1200

Test Accuracy: 0.9821 (98.21%)

Test Precision (Macro): 0.9817

Test Recall (Macro): 0.9817

Test F1-Score (Macro): 0.9817

Interpretation

These results indicate that the model generalizes well to unseen data. The macro-averaged precision, recall, and F1-score are all very close to one another, which suggests that the classifier performed consistently across classes rather than favoring only a few dominant categories.

3. Training Progress

The training history shows a steady and effective learning process.

Early Stage

At the beginning of training, the model started with moderate performance:

Epoch 1 Training Accuracy: 53.52%

Epoch 1 Validation Accuracy: 68.34%

Epoch 1 Validation Loss: 0.8806

This is expected when training from scratch, since the network begins with randomly initialized weights.

Mid Training

After only a few epochs, the model improved rapidly:

Epoch 4 Validation Accuracy: 96.76%

Epoch 8 Validation Accuracy: 97.85%

Epoch 11 Validation Accuracy: 98.53%

This shows that the model learned meaningful image features quite quickly.

Later Stage

Performance continued to improve in the later epochs:

Epoch 20 Validation Accuracy: 98.99%

Epoch 24 Validation Accuracy: 99.16%

Epoch 28 Validation Accuracy: 99.22%

Epoch 30 Validation Accuracy: 99.32%

By the final epoch:

Training Accuracy: 99.11%

Validation Accuracy: 99.32%

Training Loss: 0.0271

Validation Loss: 0.0417

These values indicate a very strong fit to the data.

4. Learning Rate Behavior

The learning rate was reduced during training:

Initial learning rate: 0.001

Reduced to 0.0005 at epoch 19

Reduced to 0.00025 at epoch 28

This learning rate schedule appears to have helped the model refine its performance in later epochs. After each reduction, validation performance remained stable or improved slightly, indicating that lower learning rates supported better convergence.

5. Overfitting Analysis

A comparison between training and validation metrics suggests that overfitting was minimal.

At the final epoch:

Training Accuracy: 99.11%

Validation Accuracy: 99.32%

The validation accuracy was slightly higher than the training accuracy, and validation loss remained low throughout the later epochs. This indicates that the model maintained good generalization rather than memorizing the training set.

Although there were some small fluctuations in validation loss across epochs, the overall trend remained stable and strong. Therefore, there is no major sign of harmful overfitting.

6. Best Validation Trend

The best validation performance was observed near the end of training:

Epoch 30 Validation Accuracy: 99.32%

Epoch 30 Validation F1-Score: 0.9930

This suggests that the model was still benefiting from training up to epoch 30, though improvements after around epoch 20 became smaller and more incremental.

7. Inference Performance

The model was also efficient during inference:

Number of batches measured: 20

Total images measured: 320

Total inference time: 0.4327 seconds

Average time per batch: 0.0216 seconds

Average time per image: 0.00135 seconds

Throughput: 739.51 images/second

Interpretation

These inference results show that the trained model is computationally efficient. On average, a single image required only about 1.35 milliseconds for prediction, while the model processed approximately 740 images per second under the measured setup. This makes the model suitable for fast classification tasks.

8. Overall Findings

The experiment demonstrates that EfficientNet-B0 trained from scratch can achieve excellent classification results. The model showed:

rapid improvement during early epochs,

stable optimization throughout training,

very high validation performance,

strong test accuracy of 98.21%,

balanced macro precision, recall, and F1-score,

and fast inference speed.

The close alignment of precision, recall, and F1-score further suggests balanced performance across classes.

9. Conclusion

In conclusion, the EfficientNet-B0 model trained from scratch performed very well on this classification task. After 30 epochs, it achieved:

98.21% test accuracy

0.9817 macro F1-score

739.51 images per second inference speed

The training history indicates strong convergence and good generalization, with minimal signs of overfitting. Overall, the model is both accurate and efficient, making it a strong choice for image classification in this setting.