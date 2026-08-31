## 1.	Executive Summary
   
Composed of trillions of microorganisms that reside in the digestive tract, the gut microbiome helps maintain immune balance, protect against pathogens, and support overall health. Chronic inflammation is a persistent, low-grade immune response that can gradually damage tissues and is linked to numerous serious health conditions, including autoimmune diseases, cardiovascular disease, type 2 diabetes, and neurodegenerative disorders such as Alzheimer’s disease (Melmedu 2024). 

Emerging research suggests that the gut microbiome plays an important role in regulating inflammation and influencing the development of these conditions. Dysregulation of the gut microbiome has been associated with a variety of diseases, including metabolic, gastrointestinal, neurological, and immune-related disorders (Shabani et al 2025).

<p align="center">
<img width="617" height="633" alt="image" src="https://github.com/user-attachments/assets/4af5ed0f-0aa0-4396-8a32-b87e4acd4585" /> 
</p>
Image: (Shabani et al 2025)


## 2.	Problem Statement/Research objective(s)

Using the Human Microbiome Project 2 (HMP2) dataset, I developed predictive models that classify individuals as having Crohn's disease, ulcerative colitis, or no inflammatory bowel disease (healthy controls) based on the relative abundance of bacterial species identified in stool samples. In addition to disease classification, I investigated whether bacterial relative abundance can be used to predict fecal calprotectin levels, a commonly used biomarker of intestinal inflammation (Hussain, 2026). This analysis may provide further insight into the relationship between gut microbial composition and inflammatory activity.

## 3.	Exploratory Data Analysis 

The dataset contains 3,387 observations and 571 features. Of these features, only three are categorical: ‘diagnosis’, External ID’, and ‘Participant Id’. The ‘External ID’ uniquely identifies each microbiome  sample. The ‘diagnosis’ variable includes three categories: Crohn's disease (1,608 observations), ulcerative colitis (913 observations), and healthy controls (866 observations).

The dataset includes samples from 116 unique participants, with some individuals contributing as many as 67 samples and others contributing only one. Of these participants, 55 have Crohn's disease, 33 have ulcerative colitis, and 28 are healthy controls.

The ‘fecalcal’ feature represents the fecal calprotectin score, a biomarker of intestinal inflammation. Scores below 100 generally indicate remission or a healthy state, whereas scores above 250 suggest active inflammation (Hussain, 2026). Although approximately 60% of observations are missing fecal calprotectin values, this variable remains an important measure of disease activity and inflammation within the dataset. 

The week_num feature represents the study week (0-52) during which each sample was collected (Hussain, 2026). All remaining features measure the abundance of specific bacterial species present in the stool samples.

*Note: The dataset was originally downloaded from Kaggle. Although it is no longer available from that source, there is a github repo containing information from the same author and project.*

## 4.	Data Preparation/Feature Engineering

In the first phase of this project, the top 20 bacterial species based on both mean and median abundance were identified for each diagnostic group. Additionally, the top 40 bacterial species by mean and median abundance were calculated for observations with fecal calprotectin (FecalCal) scores greater than 150 and less than 150, representing elevated and normal levels of inflammation, respectively. Although a FecalCal score greater than 250 is generally indicative of active inflammation, only approximately 5% of healthy individuals have scores exceeding 150.

<p align="center">
<img width="606" height="448" alt="image" src="https://github.com/user-attachments/assets/40ec4d5e-2c5c-476c-a816-1fc075616234" />
</p>

Using Plotly, three-dimensional visualizations were created to explore the relationships among diagnosis, inflammation scores, and bacterial abundance. Analysis of these visualizations indicated that maximum bacterial abundance was associated with specific diagnoses as well as FecalCal scores greater than 150. These findings suggest that overgrowth of a single bacterial species may serve as a potential indicator of disease status and inflammation severity.

<p align="center">
<img width="670" height="396" alt="image" src="https://github.com/user-attachments/assets/e4bc2483-b0bf-4aa8-bb2e-e2ef0ee90550" />
</p>

During the initial phase of the project, bacterial features were not scaled. Applying the StandardScaler() function to center the data around a mean of 0 with a standard deviation of 1 altered the rankings of the top bacterial species identified in these analyses. This finding suggests that outliers had a substantial influence on the original results.

Two separate datasets were created using the top bacterial species identified by mean and maximum abundance. The first dataset was designed to predict disease diagnosis based on the bacterial composition of a microbiome sample. The second dataset was developed to predict whether a sample's inflammation score was greater than or less than 150 based on its microbiome profile.

## 5.	Methodology and various tools used in the process

Two machine learning models were applied to each dataset: a Random Forest Classifier (RFC) from scikit-learn and a Deep Neural Network (DNN) implemented using TensorFlow Keras.

The DNN architecture consisted of two layers. The hidden layer contained 95 neurons and used the ReLU activation function, while the output layer used the Softmax activation function with the number of neurons corresponding to the number of target classes. The model was compiled using the RMSprop optimizer, and loss was calculated using the sparse categorical cross-entropy loss function (GeeksforGeeks 2025). 

Each dataset was split into training (80%) and testing (20%) subsets. Model performance was evaluated using a cross-validation framework. Precision, recall, F1-score, and accuracy were calculated for both the training and test sets to assess predictive performance and identify potential overfitting or underfitting.

## 6.	Findings and Conclusions

The Random Forest Classifier demonstrated substantially stronger performance for disease diagnosis prediction. It achieved high and consistent precision, recall, and F1-scores across all diagnostic classes on both the training and test datasets.

In comparison, the Deep Neural Network performed less effectively. The DNN exhibited a higher false-positive rate for healthy diagnoses and a higher false-negative rate for both Crohn's disease and Ulcerative Colitis diagnoses, resulting in reduced overall predictive performance.

For inflammation classification, both models performed well when predicting whether FecalCal scores were above or below 150. However, the DNN again performed slightly worse than the Random Forest Classifier. The primary limitation of the DNN was a higher false-negative rate for samples with inflammation scores greater than 150. Specifically, the DNN achieved a recall of 0.84 for this class, compared to 0.97 for the Random Forest Classifier. 

<p align="center">
<img width="46%" height="46%" alt="image" src="https://github.com/user-attachments/assets/4c49cbfb-ba00-4e96-95a0-7c63ec2a6aa6" />
<img width="49%" height="49%" alt="image" src="https://github.com/user-attachments/assets/79d4a148-235d-4104-8e53-550b4219fd87" />
</p>

## 7.	Lessons Learned and Recommendations 

Several opportunities were identified for future improvement:

- Address class imbalance, as uneven class distributions may have negatively affected model performance.
- Investigate the impact of diet, fasting status, and other lifestyle factors on bacterial abundance and disease prediction (Paukkonen et al 2024 ).
- Apply Principal Component Analysis (PCA) to larger datasets to reduce dimensionality and identify key patterns in the data.
- Further stratify FecalCal results by diagnosis to identify bacteria that are uniquely associated with inflammation within each disease group.
- Experiment with alternative deep learning architectures, including additional hidden layers, regularization methods, and hyperparameter tuning.
- Explore correlations between gut microbiome composition and other inflammatory or metabolic disorders.
