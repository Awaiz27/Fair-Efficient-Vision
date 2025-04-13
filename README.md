# FAIR-EFFICIENT-VISION : Efficient and Fair Detection of Glaucoma & Diabetic Retinopathy

## Overview

This project focuses on leveraging Vision Transformer (ViT) models for the detection of retinal diseases, specifically glaucoma and diabetic retinopathy (DR). While achieving high diagnostic performance, the project also addresses fairness concerns to mitigate biases based on sensitive demographic factors (e.g., race, gender, ethnicity, language, marital status). The methodology integrates knowledge distillation and post-processing fairness techniques to ensure equitable outcomes across diverse populations.

---

## Objectives

1. **Model Performance**:
   - Train a RetFOund ViT-Large Encoder (teacher) model using a glaucoma dataset to establish a robust baseline.
   - Use knowledge distillation to train a compact ViT-Base (student) model on a combined glaucoma and DR dataset.
   - Achieve comparable or improved diagnostic performance with the student model (AUROC).

2. **Fairness**:
   - Apply post-processing fairness methods to reduce biases across demographic attributes.
   - Evaluate model fairness and performance trade-offs to ensure equitable and reliable predictions.

---

## Methodology

### 1. **Teacher Model Training (RETFound)**
- **Model**: RetFound MAE (Masked Autoencoders for retinal imaging).
- **Dataset**: Glaucoma dataset with 10,000 subjects.
- **Goal**: Achieve high AUROC by pretraining on meaningful visual representations and fine-tuning for disease prediction.

### 2. **Student Model Training (ViT-Base)**
- **Model**: ViT-Base trained via knowledge distillation.
- **Dataset**: Combined glaucoma and DR dataset (10,000 subjects each).
- **Goal**: Efficient and generalizable student model with balanced accuracy and scalability.

### 3. **Post-Processing Fairness Method**
- **Methodology**: Based on graph smoothing via Laplacian regularization.
- **Goal**: Treat similar individuals equitably and reduce disparities across demographic groups.
- **Fairness Metrics**: Address biases in Race, Gender, Language, Marital Status, and Ethnicity.

![Project Screenshot](https://github.com/Awaiz27/Fair-Efficient-Vision/blob/main/Project_pipeline.png)

---

## Dataset Description

We would like to express our gratitude to the **Harvard Ophthalmology AI Lab** for providing the dataset used in this project. Their invaluable contribution made it possible for us to work on these important medical image detection tasks.

### 1. **Diabetic Retinopathy (DR)**
- **Subjects**: 10,000
- **Case Distribution**: 90.9% Non-vision-threatening, 9.1% Vision-threatening.

### 2. **Glaucoma**
- **Subjects**: 10,000
- **Case Distribution**: 48.7% Glaucoma, 51.3% Normal.

---

## Hyperparameters

### Teacher and Student Models
<div align="center">

|        Hyperparameter           | Teacher Model (ViT-Large) | Student Model (ViT-Base) |
|---------------------------------|---------------------------|---------------------------|
| **Input Size**                  |        200×200            |           200×200         |
| **Batch Size**                  |          64               |             40            |
| **Epochs**                      |          100              |            100            |
| **Learning Rate**               |         5e-3              |            1e-3           |
| **Weight Decay**                |         0.05              |            0.02           |
| **Layer-wise LR Decay**         |         0.65              |            0.5            |
| **Drop Path Rate**              |         0.15              |            0.1            |

</div>

### Post-Processing Fairness
<div align="center">

| Hyperparameter              | Value                     |
|-----------------------------|---------------------------|
| **Graph Construction**          | Cosine Similarity         |
| **Laplacian Regularization (λ)**| 0.001                     |
| **Scaling Factor (θ)**          | No Disease: 0.001, Glaucoma: 0.002, DR: 0.015 |
| **Distance Threshold (τ)**      | No Disease: 0.8, Glaucoma: 1.1, DR: 1.4 |

</div>

---

## Results

### Performance Metrics
<div align="center">

| Method                      | AUROC  | Comments                              |
|-----------------------------|--------|---------------------------------------|
| **RETFound (Teacher)**          | 81.0%  | Strong baseline without fairness      |
| **ViT-Base (Student)**          | 85.5%  | Efficient student model               |
| **Fair Student Model**          | 87.6%  | Fair and efficient student model      |

</div>

### Attribute-Specific Results
<div align="center">


| **Attribute**        | **Value**        | **Student Model (Unfair)** | **Fair Student Model** |
|------------------|--------------|------------------------|---------------------|
| **Race**         | **Asian**    | 0.877                  | 0.894               |
|                  | **Black**    | 0.847                  | 0.867               |
|                  | **White**    | 0.855                  | 0.877               |
| **Gender**       | **Male**     | 0.858                  | 0.877               |
|                  | **Female**   | 0.855                  | 0.8744              |
| **Ethnicity**    | **Non-Hispanic** | 0.857              | 0.876               |
|                  | **Hispanic** | 0.832                  | 0.85                |
| **Marital Status**| **Unknown** | 0.846                  | 0.866               |
|                  | **Married**  | 0.865                  | 0.8826              |
|                  | **Single**   | 0.844                  | 0.865               |
|                  | **Divorced** | 0.8751                 | 0.894               |
|                  | **Widow**    | 0.848                  | 0.87                |
|                  | **Leg-Sep**  | 0.504                  | 0.504               |
| **Language**     | **English**  | 0.8571                 | 0.8759              |
|                  | **Spanish**  | 0.756                  | 0.771               |
|                  | **Other**    | 0.861                  | 0.879               |

</div>

---

## Models Weights

The Model Weights for RetFound Teacher Model trained on Glaucoma Dataset, and Vit-base student Model trained on both Glaucoma and Diabetic Retinopathy can be accessed via this [Link to Model Weights](https://drive.google.com/drive/folders/1DhXhEjqUsccYNVr-zET084l5V1kZyT4H?usp=sharing)

## Acknowledgements

We would like to express our sincere gratitude to the following papers and their authors for their valuable contributions that greatly influenced and helped make this project possible:

1. **Zhou, Yukun, Mark A. Chia, Siegfried K. Wagner, Murat S. Ayhan, Dominic J. Williamson, Robbert R. Struyven, Timing Liu, Moucheng Xu, Mateo G. Lozano, Peter Woodward-Court, et al.** (2023). *A Foundation Model for Generalizable Disease Detection from Retinal Images*. *Nature*, 622(7981), 156–163.  
   This paper provided foundational insights into generalizable disease detection from retinal images, which helped inform our approach to medical image analysis.  
   [Link to paper](https://github.com/rmaphoh/RETFound_MAE)

2. **Hinton, Geoffrey, Oriol Vinyals, and Jeff Dean** (2015). *Distilling the Knowledge in a Neural Network*. *NIPS 2014 Deep Learning Workshop*.  
   This work introduced the concept of knowledge distillation, which we leveraged to optimize our deep learning models.  
   [Link to paper](https://doi.org/10.48550/arXiv.1503.02531)

3. **Petersen, Felix, Debarghya Mukherjee, Yuekai Sun, and Mikhail Yurochkin** (2021). *Post-processing for Individual Fairness*. *NeurIPS 2021*.  
   The techniques proposed in this paper on ensuring individual fairness post-processing were instrumental in shaping our approach to fairness in the model.  
   [Link to paper](https://arxiv.org/pdf/2110.13796)

4. **Luo, Yan, Muhammad Osama Khan, Yu Tian, Min Shi, Zehao Dou, Tobias Elze, Yi Fang, and Mengyu Wang** (2024). *FairVision: Equitable Deep Learning for Eye Disease Screening via Fair Identity Scaling*.  
   This recent work in equitable deep learning for eye disease screening greatly influenced our efforts to incorporate fairness in the model training and evaluation processes.  
   [Link to paper](https://arxiv.org/abs/2310.02492)

5. **Lohia, Priyanka, et al.** (2020). *Bias Mitigation Post-processing for Individual and Group Fairness*. *NeurIPS 2020*.  
   The methodologies outlined in this paper were crucial for our approach to bias mitigation, ensuring that fairness was maintained across both individual and group levels.  
   [Link to paper](https://arxiv.org/abs/1812.06135)

These papers provided valuable techniques and methodologies that were essential in developing a fair and efficient model for detecting glaucoma and diabetic retinopathy. We are grateful for their contributions to the field of fair machine learning and medical image analysis.

---

## Citation

If you use this project, please cite as follows:

Noor, Awaiz, Hassan, Zohaib, & Kumar, Surendar . (2024). **FAIR-EFFICIENT-VISION: Efficient and Fair Detection of Glaucoma & Diabetic Retinopathy**.  
GitHub.https://github.com/Awaiz27/Fair-Efficient-Vision
---

## License

This project is licensed under the MIT License.


