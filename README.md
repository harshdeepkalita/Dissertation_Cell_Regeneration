# Dissertation: Applying Machine Learning Methods for Modeling Cell Behavior During Regeneration (Variant 2)

## Final Model

- [MedicalNet](https://github.com/Tencent/MedicalNet)
- MedicalNet is a model developed by Tencent’s research team to advance 3D medical image analysis using deep learning and transfer learning techniques which is based on [MED3D: TRANSFER LEARNING FOR 3D MEDICAL IMAGE ANALYSIS](https://arxiv.org/pdf/1904.00625).
    - It provides a family of 3D ResNet-based encoder networks that are pre-trained on a massive, diverse aggregated medical datasets called [3DSeg-8](https://paperswithcode.com/dataset/3dseg-8).
    - These pre trained 3D ResNet based encoders can be used for downstream tasks such as classification and segmentation:

<img width="731" alt="image" src="https://github.com/user-attachments/assets/d41e5832-69eb-4cbe-99f8-b8d27ab40251" />


- I used the 3D-ResNet10 variant from MedicalNet’s encoder family for my Binary Classification between WT (Wild Type) vs MUT (Mutant), where I added a fully connected layer followed by a sigmoid activation function to the pre-trained encoder :  [Drew this figure using Draw.io and Overleaf] 

<img width="786" alt="image" src="https://github.com/user-attachments/assets/3fe816e3-7fc4-4830-a782-04917548774f" />


## Approach 1 : Train the network with the encoder weights frozen

- Implemented 5 Fold Cross Validation.
- Ran the network for 30 epochs per fold.
- Total training time : 23 hours, 10 minutes, and 25 seconds.
- Encoder weights were frozen; only the output layer was trained.
- Loss function : Binary Cross Entropy 

1. K = 5 Fold Cross-Validation Summary (Training Phase): 
    
   <img width="484" alt="image" src="https://github.com/user-attachments/assets/6b992ebd-703e-4447-8d1c-215a8e264f43" />

    
2. Evaluation Metrics on Unseen Validation Data from 5-Fold Cross-Validation:

| **Trained Model (Fold)** | **Accuracy** | **Precision** | **Recall** | **F1 Score** | **ROC AUC** |
| --- | --- | --- | --- | --- | --- |
| Fold 1 | 0.7600 | 0.8889 | 0.6154 | 0.7273 | 0.9295 |
| Fold 2 | 0.8000 | 1.0000 | 0.6154 | 0.7619 | 0.9744 |
| Fold 3 | 0.7600 | 0.8889 | 0.6154 | 0.7273 | 0.9551 |
| Fold 4 | 0.8000 | 1.0000 | 0.6154 | 0.7619 | 0.9551 |
| Fold 5 | 0.8000 | 1.0000 | 0.6154 | 0.7619 | 0.9551 |
| **Mean ± Std** | **0.7840 ± 0.0196** | **0.9556 ± 0.0544** | **0.6154 ± 0.0000** | **0.7481 ± 0.0170** | **0.9538 ± 0.0143** |

3.  Receiver Operating Characteristic (ROC) Curves Across 5 Folds with Mean AUC: 
<img width="400" alt="image" src="https://github.com/user-attachments/assets/3ab9341c-7983-4a1c-a1bf-aafe155ed776" />

- The closer the AUC is to 1.0, the better the model distinguishes WT from MUT. In our case, the mean AUC across folds was 0.95 ± 0.01, indicating consistently high discriminative performance across all validation splits.

4. Training and Validation loss curves across all 5 folds during cross-validation:

<img width="400" alt="image" src="https://github.com/user-attachments/assets/ccc3fdf1-c338-42c4-9c1f-25e93d699eba" />


<img width="400" alt="image" src="https://github.com/user-attachments/assets/ea0d653f-8514-4823-893a-f879377d67d9" />

- The model does not appear to be overfitting, as:
  - A low average standard deviation of training loss (~0.0082) and validation loss (~0.0186) across the 5 folds is low, indicating low variance and stable generalization across folds.
  - Validation loss consistently remains below training loss, which may indicate good generalization.
  - However, this pattern may arise due to the use of transfer learning with a frozen encoder, with the encoder’s weights frozen during training. Additionally, strong regularization techniques—such as dropout or data augmentation—are applied during training but not during validation. These factors can make the model’s performance appear worse on the training data than on the validation set, leading to higher training loss despite the model actually generalizing well.

 
**Note: During 5-fold cross-validation, Fold 3 stopped training at epoch 26 due to early stopping. All other folds continued training up to epoch 30. To ensure consistency in visualizing and averaging training and validation losses across folds, all loss curves were truncated at epoch 26 (the final epoch completed by every fold).**

## Approach 2 : Train the entire network without freezing encoder weights [Hoping for better results here!!]


## To-Do List

- [x] Finalise the Network for training my 3D medical dataset.
- [x] Approach 1
- [ ] Approach 2


