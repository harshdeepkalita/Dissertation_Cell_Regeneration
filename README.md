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

- Model shows high precision but relatively low recall, suggesting it misses many actual “Mutant” cases (i.e., high false negatives). This may be due to the use of frozen encoder weights.
- Accuracy remains consistent across folds, with stable performance on unseen data.
- ROC AUC is consistently high (~0.95 ± 0.01), indicating strong ability to distinguish between WT and MUT.
- Low standard deviations across metrics imply reliable generalization across all folds.

3. Plots:
- Receiver Operating Characteristic (ROC) Curves Across 5 Folds with Mean AUC: 

<img width="400" alt="image" src="https://github.com/user-attachments/assets/3ab9341c-7983-4a1c-a1bf-aafe155ed776" /> 

- Training and Validation loss curves across all 5 folds during cross-validation:

<img width="400" alt="image" src="https://github.com/user-attachments/assets/ccc3fdf1-c338-42c4-9c1f-25e93d699eba" />


<img width="400" alt="image" src="https://github.com/user-attachments/assets/ea0d653f-8514-4823-893a-f879377d67d9" />

- The model does not appear to be overfitting:
    - Validation loss is consistently below training loss, which may suggest good generalization.
    -  This pattern may result from the use of transfer learning with frozen encoder weights, along with regularization techniques (such as dropout or data augmentation) applied during training but not during validation — leading to artificially higher training loss despite good generalization.


 
**Note: During 5-fold cross-validation, Fold 3 stopped training at epoch 26 due to early stopping. All other folds continued training up to epoch 30. To ensure consistency in visualizing and averaging training and validation losses across folds, all loss curves were truncated at epoch 26 (the final epoch completed by every fold).**

4. Grad-CAM : The representations visualised are from the pretrained encoder's last layer and this wasn't trained in approach1, so the results are going to be roughly informative. We'll compare these results with the approach 2's encoder.
 <img width="400" alt="image" src="https://github.com/user-attachments/assets/9343e7b5-f955-4ab7-b90a-c184ceea59de" />
<img width="400" alt="image" src="https://github.com/user-attachments/assets/49ffbeef-86ec-47f7-a5b9-9d54df4cfdf2" />




## Approach 2 : Train the entire network without freezing encoder weights. Hoping for better results here!!


## To-Do List

- [x] Finalise the Network for training my 3D medical dataset.
- [x] Approach 1 - recall low is a problem here.
- [ ] Approach 2


