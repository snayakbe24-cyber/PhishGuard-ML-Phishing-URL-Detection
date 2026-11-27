\# PhishGuard – Machine Learning-Based Phishing URL Detection



PhishGuard is a machine learning-based phishing URL detection system that analyses the structural and lexical characteristics of URLs to identify potentially malicious links.



The project uses feature engineering and supervised machine learning to distinguish phishing URLs from legitimate URLs. It compares multiple classification models, investigates benign URL structure augmentation, and evaluates model performance through internal testing and independent external evaluation.



\*\*Project Domain:\*\* Machine Learning / Cybersecurity

\*\*Project Type:\*\* Individual Academic Project

\*\*Course:\*\* UML501 – Machine Learning

\*\*Institution:\*\* Thapar Institute of Engineering and Technology



\---



\## 1. Problem Statement



Phishing attacks use deceptive URLs to trick users into visiting malicious websites and exposing sensitive information.



PhishGuard addresses this problem by extracting 16 lexical and structural features from a URL and using machine learning to classify it as either phishing or legitimate.



\*\*Important:\*\* PhishGuard is an offline URL analysis prototype. It analyses the URL as text and does not open, visit or contact the destination website.



\## 2. Key Features



\- Extraction of 16 lexical and structural URL features.

\- Comparison of Logistic Regression, Random Forest and XGBoost.

\- Three Logistic Regression configurations: baseline, class-balanced and benign-path augmented.

\- Domain-aware training, validation and test splitting.

\- Synthetic benign-path augmentation to address a structural data limitation.

\- Independent external evaluation using the PhiUSIIL dataset.

\- Interactive prediction interface.

\- Phishing score, classification, risk category and extracted feature display.

\- Saved model artifacts for reproducible predictions.



\## 3. Machine Learning Algorithms



Five model candidates were developed and evaluated.



\### 3.1 Logistic Regression – Baseline



The baseline Logistic Regression model uses L2 regularisation with C = 1.0 and the LBFGS solver.



\### 3.2 Logistic Regression – Class-Balanced



This variant uses `class\\\_weight="balanced"` to adjust the contribution of each class during model training.



\### 3.3 Logistic Regression – Benign-Path Augmented



This variant introduces synthetic legitimate URL paths using legitimate domains from the training partition.



It was designed to address a structural limitation in the original dataset, where legitimate URLs rarely contained non-empty paths or queries.



\### 3.4 Random Forest



An ensemble learning algorithm that combines multiple decision trees to perform classification.



\### 3.5 XGBoost



A gradient boosting algorithm that builds an ensemble of decision trees sequentially to improve classification performance.



\## 4. Feature Engineering



PhishGuard extracts the following 16 features from URLs:



| No. | Feature |

|---|---|

| 1 | URL Length |

| 2 | IP Address Presence |

| 3 | Dot Count |

| 4 | HTTPS Flag |

| 5 | URL Entropy |

| 6 | Token Count |

| 7 | Subdomain Count |

| 8 | Query Parameter Count |

| 9 | TLD Length |

| 10 | Path Length |

| 11 | Hyphen in Domain |

| 12 | Number of Digits |

| 13 | TLD Popularity |

| 14 | Suspicious File Extension |

| 15 | Domain Name Length |

| 16 | Percentage of Numeric Characters |



These features are extracted directly from URL strings without contacting the destination website.



\*\*Class mapping:\*\*

\- `0 = Phishing`

\- `1 = Legitimate`



\## 5. Dataset



\### 5.1 LegitPhish v2



The primary dataset was used for model development, training and internal evaluation.



\- Original dataset: 101,219 rows.

\- Missing target labels and exact duplicate rows were removed.

\- Final cleaned dataset: 100,872 rows.

\- Phishing URLs: 63,492.

\- Legitimate URLs: 37,380.



The data was divided into training, validation and test partitions using domain-aware grouping to reduce overlap between domains.



| Partition | Rows |

|---|---:|

| Training | 64,433 |

| Validation | 16,200 |

| Test | 20,239 |



\### 5.2 PhiUSIIL Phishing URL Dataset



The PhiUSIIL dataset from the UCI Machine Learning Repository (Dataset ID: 967) was used for independent external evaluation.



\- Raw dataset: 235,795 rows.

\- Usable unique URLs: 235,370.

\- Extracted features: 16.

\- Feature extraction failures: 0.



The external evaluation used two groups to examine generalisation under different overlap restrictions.



The raw datasets are not included in this repository.



\## 6. Internal Model Evaluation



\### 6.1 Validation Performance – Five Model Candidates



All five candidates were evaluated on the same validation partition.



Phishing was treated as the positive class for the reported precision, recall and F1 metrics.



| Model | Accuracy | Phishing Precision | Phishing Recall | Phishing F1 |

|---|---:|---:|---:|---:|

| Logistic Regression – Baseline | 99.72% | 99.92% | 99.63% | 99.77% |

| Logistic Regression – Class-Balanced | 99.70% | 99.93% | 99.59% | 99.76% |

| Logistic Regression – Benign-Path Augmented | 99.55% | 99.99% | 99.29% | 99.64% |

| Random Forest | 99.75% | 99.66% | 99.95% | 99.80% |

| XGBoost | 99.91% | 99.91% | 99.94% | 99.93% |



\### 6.2 Logistic Regression Variant Selection



The three Logistic Regression variants were further compared using validation phishing F1 and a separate synthetic benign-path stress test.



The stress test used 5,942 legitimate validation-domain hostnames with generated URL paths.



| Logistic Regression Variant | Synthetic Benign-Path FPR | Deployment Validation Score |

|---|---:|---:|

| Baseline | 47.04% | 0.7637 |

| Class-Balanced | 20.14% | 0.8981 |

| Benign-Path Augmented | 0.02% | 0.9981 |



The selection score was calculated as:



`0.5 × Validation Phishing F1 + 0.5 × (1 − Synthetic Benign-Path FPR)`



The augmented Logistic Regression model was selected because it achieved the strongest combined score, particularly reducing false positives on the controlled benign-path stress test.



\*\*Note:\*\* The synthetic stress test is a controlled experiment, not a benchmark of independently verified real-world URLs.



\### 6.3 Final Model – Held-Out Test Performance



The selected augmented Logistic Regression model was refitted using the combined training and validation partitions.



The untouched test partition was excluded from model fitting and model selection.



| Metric | Final Model |

|---|---:|

| Accuracy | 99.32% |

| Balanced Accuracy | 99.37% |

| Phishing Precision | 99.73% |

| Phishing Recall | 99.19% |

| Phishing F1-Score | 99.46% |

| ROC-AUC | 99.84% |

| PR-AUC | 99.94% |



\*\*Test set size:\*\* 20,239 URLs.



\## 7. External Evaluation – PhiUSIIL



External evaluation was conducted using the existing fitted baseline Logistic Regression, Random Forest and XGBoost models.



No model retraining, scaler refitting or threshold tuning was performed during this evaluation.



\### 7.1 Evaluation Groups



\- \*\*Group A – Exact URL Overlap Removed:\*\* Exact normalized URL overlaps with the original dataset were removed.

\- \*\*Group B – Exact URL and Registered-Domain Overlap Removed:\*\* Both exact URL and registered-domain overlaps were removed. This represents the stricter evaluation.



| Evaluation Group | Remaining URLs |

|---|---:|

| Group A | 198,426 |

| Group B | 167,842 |



\### 7.2 Group A – Exact URL Overlap Removed



| Model | Accuracy | Phishing Precision | Phishing Recall | Phishing F1 | ROC-AUC | PR-AUC |

|---|---:|---:|---:|---:|---:|---:|

| Logistic Regression – Baseline | 90.81% | 99.99% | 81.86% | 90.02% | 99.07% | 99.38% |

| Random Forest | 78.49% | 100.00% | 57.54% | 73.05% | 99.72% | 99.76% |

| XGBoost | 77.86% | 100.00% | 56.29% | 72.03% | 99.76% | 99.84% |



\### 7.3 Group B – Exact URL and Registered-Domain Overlap Removed



| Model | Accuracy | Phishing Precision | Phishing Recall | Phishing F1 | ROC-AUC | PR-AUC |

|---|---:|---:|---:|---:|---:|---:|

| Logistic Regression – Baseline | 93.95% | 99.99% | 85.86% | 92.39% | 99.18% | 99.29% |

| Random Forest | 76.55% | 100.00% | 45.20% | 62.26% | 99.63% | 99.63% |

| XGBoost | 75.96% | 100.00% | 43.82% | 60.94% | 99.69% | 99.73% |



\### 7.4 External Evaluation Findings



\- Baseline Logistic Regression achieved the strongest phishing F1-score among the three externally evaluated models in both groups.

\- Random Forest and XGBoost experienced substantial reductions in phishing recall on the external dataset.

\- The high ROC-AUC values did not translate into equally strong phishing recall at the fixed 0.50 decision threshold.

\- The results demonstrate the importance of independent evaluation and the challenges of model generalisation across datasets.



\*\*Important evaluation distinction:\*\* External evaluation was performed only on baseline Logistic Regression, Random Forest and XGBoost. The class-balanced Logistic Regression, benign-path-augmented Logistic Regression and final deployed augmented model were not externally evaluated.



\## 8. Final Prediction System



The final prediction system uses the saved augmented Logistic Regression model.



The prediction workflow:



1\. Accepts a URL from the user.

2\. Validates the URL format.

3\. Extracts the 16 lexical features.

4\. Checks feature names and their order.

5\. Loads the saved Logistic Regression model and scaler.

6\. Calculates the phishing score.

7\. Displays the prediction, risk category and extracted features.



\### 8.1 Risk Categories



| Phishing Score | Classification | Risk Level |

|---|---|---|

| Below 30% | Legitimate | LOW |

| 30%–80% | Legitimate – Manual Review | CAUTION |

| Above 80% | Phishing | HIGH |



The displayed scores are uncalibrated model outputs. They should not be interpreted as calibrated real-world probabilities or guarantees of website safety.



\## 9. Saved Model Artifacts



The final model and its supporting files are saved separately.



| Artifact | Purpose |

|---|---|

| `LogisticRegression\\\_model.joblib` | Trained Logistic Regression classifier |

| `LogisticRegression\\\_scaler.joblib` | Fitted StandardScaler |

| `LogisticRegression\\\_features.joblib` | Ordered list of 16 features |

| `LogisticRegression\\\_config.joblib` | Model configuration and experiment metadata |



The model must be used with the corresponding scaler and feature order to reproduce predictions correctly.



\## 10. Repository Structure



```text

PhishGuard-ML-Phishing-URL-Detection/

│

├── Notebooks/

│   └── PhishGuard\\\_Final\\\_Notebook.ipynb

│

├── Models/

│   ├── LogisticRegression\\\_model.joblib

│   ├── LogisticRegression\\\_scaler.joblib

│   ├── LogisticRegression\\\_features.joblib

│   └── LogisticRegression\\\_config.joblib

│

├── Screenshots/

│

├── Report/

│   └── PhishGuard\\\_Final\\\_Project\\\_Report.pdf

│

├── README.md

├── requirements.txt

└── .gitignore

```



\## 11. Technologies Used



\- \*\*Programming Language:\*\* Python

\- \*\*Development Environment:\*\* Google Colab

\- \*\*Data Processing:\*\* Pandas, NumPy

\- \*\*Machine Learning:\*\* Scikit-learn, XGBoost

\- \*\*Data Visualisation:\*\* Matplotlib, Seaborn

\- \*\*Model Persistence:\*\* Joblib

\- \*\*Interactive Interface:\*\* IPyWidgets

\- \*\*Domain Processing:\*\* tldextract



\## 12. Running the Project



The project was developed in Google Colab.



1\. Clone or download this repository.

2\. Open the notebook inside the `Notebooks` folder using Google Colab.

3\. Install the required Python libraries if they are not already available.

4\. Obtain the required datasets separately.

5\. Upload the required dataset when prompted by the notebook.

6\. Run the notebook cells sequentially to reproduce the experiments.

7\. Follow the final cells to launch the offline prediction interface.



The saved model artifacts are available in the `Models` folder.



\## 13. Limitations



\- The system relies on lexical URL features and does not analyse webpage content.

\- It does not inspect DNS records, SSL certificates, redirects, HTML or JavaScript.

\- External evaluation revealed a generalisation gap, particularly for Random Forest and XGBoost.

\- Long legitimate URLs may sometimes receive high phishing scores.

\- The final augmented Logistic Regression model was not independently evaluated on the external dataset.

\- The synthetic benign-path experiment does not replace evaluation on independently labelled real-world URLs.

\- The system is an academic research prototype, not a production-grade security solution.



\## 14. Future Improvements



\- Evaluate the final augmented model on additional independent datasets.

\- Improve generalisation across different URL structures and domains.

\- Explore additional URL and domain-based features.

\- Investigate probability calibration.

\- Extend the system with DNS, certificate and webpage-level analysis.

\- Evaluate performance on more diverse and independently collected URL datasets.



\## 15. Project Information



\*\*Project Name:\*\* PhishGuard – Machine Learning-Based Phishing URL Detection



\*\*Domain:\*\* Machine Learning / Cybersecurity



\*\*Project Type:\*\* Individual Academic Project



\*\*Course:\*\* UML501 – Machine Learning



\*\*Institution:\*\* Thapar Institute of Engineering and Technology



\---



\*PhishGuard is an academic machine learning prototype intended for research and educational purposes. Its predictions should not be treated as a guarantee of website safety.\*

