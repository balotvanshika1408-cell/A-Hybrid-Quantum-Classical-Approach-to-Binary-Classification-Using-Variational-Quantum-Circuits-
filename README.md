# A-Hybrid-Quantum-Classical-Approach-to-Binary-Classification-Using-Variational-Quantum-Circuits-
1.Introduction 
This project explores a Hybrid Quantum–Classical Machine Learning (QML) approach for a binary 
classification task. The goal is to study how quantum-generated features, obtained using a Variational 
Quantum Circuit (VQC), perform when combined with a classical machine learning classifier, and to 
compare this performance with standard classical models. 
The project is implemented entirely using quantum simulation (Qiskit) and classical machine learning 
(scikit-learn), focusing on correct workflow design, reproducibility, and fair comparison rather than 
claiming quantum advantage. 
2. Objectives 
The main objectives of this project are: 
 To preprocess and reduce a classical dataset so that it is compatible with near-term quantum 
circuits. 
 To design a Variational Quantum Circuit(VQC) with data re-uploading for feature encoding. 
 To generate quantum features using expectation values from the quantum circuit. 
 To train a hybrid quantum–classical classifier using these quantum features. 
 To compare the hybrid model with classical baseline models using Accuracy and AUC-ROC. 
 To analyze performance stability, seeding effects, and potential sources of error such as data 
leakage. 
3. Data Analysis and Preprocessing 
3.1 Dataset Understanding 
The dataset consists of 8 total columns, including: 
 7 feature columns 
 1 target column (Fraud) representing a binary classification task. 
The dataset contains numerical and categorical attributes, along with a small number of missing values. 
3.2 Handling Missing Values 
Missing values were handled using manual imputation via the fillna() method. 
 Numerical features were imputed using the median 
 Categorical features were imputed using the mode 
To prevent data leakage, median and mode values were computed only on the training dataset and then 
applied to the test dataset. 
3.3 Outlier Analysis 
Outliers were analyzed using boxplots and distribution plots. Since the dataset is relatively small and the 
outliers were not extreme, aggressive removal was avoided to prevent loss of potentially important 
fraud-related patterns. 
3.3 Feature Selection and Dimensionality Reduction 
Due to quantum hardware limitations, the number of features was reduced to at most 4, as each feature 
corresponds to one qubit .  
Feature selection was performed using: 
1. Correlation analysis 
2. Statistical dependency tests 
3. Domain relevance 
This step reduced noise and ensured the dataset was suitable for quantum encoding. 
3.4 Feature Scaling 
All selected numerical features were scaled using Min–Max Scaling, mapping values to the range [0,1]. 
This scaling is essential for quantum circuits, as rotation angles are sensitive to input magnitude. 
3.5 Train–Test Split 
A single train–test split was created at the beginning. 
The same split was reused for: 
 Classical machine learning models 
 Quantum feature generation 
 Quantum–Classical hybrid model evaluation 
This ensured a fair and consistent comparison across all models. 
4. Classical Baseline Models 
To establish benchmarks, the following classical models were trained using scaled classical features: 
1. Logistic Regression 
2. Random Forest 
3. Classical Neural Network (MLP) 
These models were evaluated using: 
1. Accuracy              
2. AUC-ROC 
5. Hybrid Quantum–Classical Model 
5.1 Quantum Data Encoding 
Classical features were encoded into quantum states using parameterized rotation gates (RY). Each 
feature was mapped to the rotation angle of a corresponding qubit. To ensure stable encoding, all 
features were scaled to the range [0,1] before being mapped to rotation angles. 
5.2 Variational Quantum Circuit (VQC) 
A Variational Quantum Circuit was constructed using: 
1. Layered RY rotations 
2. Entanglement via CNOT gates 
3. Data re-uploading across multiple layers 
The trainable parameters of the VQC are denoted by θ and are optimized during training using a classical 
optimizer. 
5.3 Quantum Feature Extraction 
Once the quantum circuit is constructed and parameterized, it is executed using a quantum simulator. 
Measurements are not taken directly; instead, expectation values of Pauli-Z operators are computed for 
each qubit. 
These expectation values produce a real-valued feature vector for each data sample. This vector 
represents quantum-generated features, which capture complex interactions between the original input 
features 
5.4 Classical Co-Processor 
The quantum-generated features are then passed to a classical neural network, which acts as a co
processor. In this project, a Multi-Layer Perceptron (MLP) classifier was used. 
The classical model: 
1. Receives quantum features as input 
2. Learns decision boundaries using standard backpropagation 
3. Outputs final class predictions and probabilities 
6. Training and Optimization 
6.1 Hybrid Training Loop 
The model was trained using a hybrid optimization pipeline: 
1. Quantum parameters (θ) were optimized using the COBYLA gradient-free optimizer 
2. Binary cross-entropy (log_loss) was used as the loss function 
3. Classical neural network training occurred inside the optimization loop 
6.2 Reproducibility 
Random seeds were fixed for: 
1. NumPy 
2. Python random 
3. Scikit-learn models 
This ensured consistent train–test splits and reproducible results. 
7. Comparative Analysis  
To evaluate the effectiveness of the proposed hybrid quantum–classical model, its performance was 
compared against multiple classical machine learning baselines trained on the same pre-processed 
dataset. The comparison was conducted using consistent train–test splits, identical feature sets, and 
standard evaluation metrics. 
7.1 Models Compared 
The following models were evaluated: 
1. Logistic Regression (LR) 
2. Random Forest (RF) 
3. Classical Neural Network (MLP) 
4. Quantum–Classical Hybrid Model 
7.2 Evaluation Metrics 
Model performance was assessed using: 
1. Accuracy: Measures the overall correctness of predictions. 
2. AUC–ROC: Measures the model’s ability to distinguish between classes across different decision 
thresholds. 
AUC–ROC is particularly important in fraud detection scenarios due to potential class imbalance. 
7.3 Quantitative Comparison 
Model                                            
1. Logistic Regression                                
2. Random Forest                                              
3. Classical Neural Network                 
4. Quantum–Classical Hybrid                 
Accuracy        
0.937               
0.974               
0.965               
0.961               
AUC-ROC 
0.952 
0.992 
0.941 
0.979 
The hybrid quantum–classical model demonstrated competitive and, in some runs, superior AUC–ROC 
values compared to classical baselines. While accuracy values were similar across models, improvements 
in AUC suggest better class separability in the hybrid approach. 
7.4 ROC Curve Interpretation 
The ROC curve also supports the numerical results. The curve of the quantum–classical hybrid model 
was closer to the top-left corner of the graph, which means it was better at correctly identifying both 
fraud and non-fraud cases. 
This indicates that the features generated by the quantum circuit helped the model separate the two 
classes more clearly. 
7.5 Key Observations 
1. Logistic Regression performed adequately but was limited by its linear nature. 
2. The Random Forest model was able to learn complex, non-linear patterns in the data, but its 
performance changed noticeably depending on which features were selected.  
3. The Classical MLP improved upon linear models but relied entirely on classical features. 
4. The Hybrid Quantum–Classical model benefited from quantum feature transformations, leading to 
improved discrimination performance in several experiments. 
8. Discussion 
The experimental results indicate that hybrid quantum–classical models can serve as effective feature 
generators rather than standalone classifiers. The quantum circuit acts as a non-linear transformation 
layer, enriching the feature space before classical classification. 
However, several important considerations emerged during experimentation: 
1. Initial implementations suffered from data leakage, which was later corrected by strict 
separation of training and testing pipelines. 
2. The model results changed slightly when different random seed values were used, showing that 
the training process depends on random initialization. 
3. Also, the improved performance of the quantum model does not guarantee true quantum 
advantage, since all experiments were conducted using quantum simulators running on classical 
computers. 
Despite these limitations, the observed improvements in AUC–ROC suggest that quantum-enhanced 
feature representations can be beneficial for certain datasets. 
9. Limitations and Experimental Challenges 
1. Quantum circuits were simulated due to hardware limitations. 
2. Circuit depth and number of qubits were restricted to maintain feasibility. 
3. Gradient-free optimization (COBYLA) is computationally expensive and sensitive to initialization. 
4. Results may vary depending on random seeds and hyperparameter choices. 
These challenges reflect the current state of near-term quantum machine learning research. 
10. Conclusion and Future Work 
This project presented a systematic implementation and evaluation of a Hybrid Quantum–Classical 
Machine Learning framework for binary classification. Through careful data preprocessing, feature 
reduction, and hybrid optimization, the proposed model achieved competitive performance compared 
to classical baselines. 
Key conclusions include: 
1. Hybrid models are practical and implementable on near-term quantum simulators. 
2. Quantum circuits are effective as feature transformers rather than standalone classifiers. 
3. Proper experimental design is critical to avoid misleading performance gains. 
Future Work 
Potential directions for future research include: 
1. Deployment on real quantum hardware. 
2. Exploration of alternative feature maps and entanglement strategies. 
3. Integration of advanced optimizers and regularization techniques. 
4. Evaluation on larger and more diverse datasets.
