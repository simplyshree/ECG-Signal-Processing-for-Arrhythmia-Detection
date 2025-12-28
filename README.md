<h1>ECG Signal Classification using the MIT BIH Datasets</h1>
<p style="color:red; font-weight:bold; font-size:18px;">
  Results Below
</p>


<h2>1. Dataset Description</h2>
<p>
The MIT-BIH Arrhythmia Database is a widely used benchmark dataset for evaluating
ECG-based arrhythmia classification algorithms. Each record contains continuous ECG
signals sampled at 360 Hz along with annotation files that specify the occurrence and
type of each heartbeat. In this project, only the MLII lead is used for analysis, as it
provides clear QRS complexes and is commonly adopted in clinical studies.
</p>

<p>
Link to Datasets 
<ul>
<li> 1. For main python file : https://www.kaggle.com/datasets/shayanfazeli/heartbeat</li>
<li> 1. For MLII Lead only (Window based Feature Extraction) : https://www.kaggle.com/datasets/taejoongyoon/mitbit-arrhythmia-database/data </li>

</ul>
</p> 

<h2>2. Data Organization</h2>
<p>
Each ECG record is stored as a CSV file with the following columns:
</p>
<ul>
  <li><strong>Sample #</strong> – Index of the ECG sample</li>
  <li><strong>MLII</strong> – ECG signal from the MLII lead</li>
  <li><strong>V1</strong> – ECG signal from the V1 lead</li>
</ul>

<p>
Corresponding annotation files contain beat-level information including:
</p>
<ul>
  <li>Time of beat occurrence</li>
  <li>Sample number of the R-peak</li>
  <li>Beat type symbol (e.g., N, V, A, F, Q)</li>
</ul>

<h2>3. Signal Selection</h2>
<p>
Only the MLII lead was selected for further processing due to its effectiveness in
capturing heartbeat morphology and its widespread use in arrhythmia detection
literature.
</p>

<h2>4. Beat Segmentation (Feature Extraction)</h2>
<p>
Arrhythmia classification is treated as a beat-level problem rather than a
sample-level problem. Using the R-peak locations provided in the annotation files,
individual heartbeats were segmented from the continuous ECG signal.
</p>

<p>
For each R-peak:
</p>
<ul>
  <li>200 samples before the R-peak were extracted</li>
  <li>200 samples after the R-peak were extracted</li>
  <li>Each heartbeat was represented by a fixed-length vector of 400 samples</li>
</ul>

<p>
These segmented beats capture the morphological characteristics of individual
heartbeats and serve as the feature vectors for classification.
</p>

<h2>5. Beat Labeling</h2>
<p>
Each extracted heartbeat was assigned a class label based on the beat type symbol
from the annotation file. Original MIT-BIH symbols were mapped into five
AAMI-standard heartbeat classes:
</p>

<ul>
  <li><strong>Class 0</strong> – Normal beats (N, L, R, e, j)</li>
  <li><strong>Class 1</strong> – Supraventricular ectopic beats (A, a, J, S)</li>
  <li><strong>Class 2</strong> – Ventricular ectopic beats (V, E)</li>
  <li><strong>Class 3</strong> – Fusion beats (F)</li>
  <li><strong>Class 4</strong> – Unknown or paced beats (Q)</li>
</ul>

<h2>6. Dataset Construction</h2>
<p>
Heartbeat segments extracted from all patient records were combined into a single
dataset. Each row in the dataset represents one heartbeat with 400 features, and
each corresponding label indicates the heartbeat class.
</p>

<h2>7. Train–Test Split</h2>
<p>
The complete dataset was divided into training and testing sets using a 70:30 split.
</p>

<h2>8. Classification Model</h2>
<p>
A Random Forest Classifier was used for arrhythmia classification. Random Forest is
an ensemble learning technique that constructs multiple decision trees and outputs
the most frequent class prediction. It is robust to noise, handles high-dimensional
data effectively, and reduces overfitting.
</p>

<p>
Class weighting was applied to mitigate the effects of class imbalance present in
the dataset.
</p>

<h2>9. Model Training</h2>
<p>
The Random Forest model was trained using the extracted heartbeat segments from
the training dataset. Each beat was treated as an independent sample, allowing the
classifier to learn the relationship between ECG morphology and heartbeat class.
</p>

<h2>Results and Performance Analysis</h2>

<p>
This section summarizes the classification performance of different machine learning
models evaluated on the MIT-BIH Arrhythmia dataset. All experiments were conducted
using a beat-based ECG segmentation approach and a 70:30 train–test split unless
otherwise specified.
</p>

<h3>Experiment 1: Baseline Model Comparison</h3>

<p>
In the first set of experiments, multiple machine learning models were evaluated
using the complete feature set. The objective was to compare classical classifiers
and assess the effectiveness of ensemble learning.
</p>

<table border="1" cellpadding="8" cellspacing="0">
  <thead>
    <tr>
      <th>Model</th>
      <th>Evaluation Method</th>
      <th>Accuracy (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Logistic Regression</td>
      <td>Train–Test Split</td>
      <td>91.3393</td>
    </tr>
    <tr>
      <td>Logistic Regression</td>
      <td>10-Fold Cross Validation</td>
      <td>91.3744</td>
    </tr>
    <tr>
      <td>Random Forest Classifier</td>
      <td>Train–Test Split</td>
      <td>97.4785</td>
    </tr>
  </tbody>
</table>

<h3>Experiment 2: MLII Lead with Fixed Beat Window</h3>

<p>
In the second set of experiments, only the MLII ECG lead was used. Beat-level features
were extracted using fixed-length windows centered around the R-peak. The impact of
window size on classification performance was analyzed.
</p>

<table border="1" cellpadding="8" cellspacing="0">
  <thead>
    <tr>
      <th>Model</th>
      <th>Lead Used</th>
      <th>Window Size</th>
      <th>Accuracy (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Random Forest Classifier</td>
      <td>MLII</td>
      <td>360 (180 + 180)</td>
      <td>99.2736</td>
    </tr>
    <tr>
      <td>Random Forest Classifier</td>
      <td>MLII</td>
      <td>400 (200 + 200)</td>
      <td>99.6368</td>
    </tr>
  </tbody>
</table>

<h3>Performance Discussion</h3>

<p>
The baseline comparison demonstrated that ensemble-based methods significantly
outperformed linear classifiers. While Logistic Regression achieved an accuracy of
approximately 91%, the Random Forest classifier improved performance to over 97%,
highlighting its ability to capture complex nonlinear patterns in ECG signals.
</p>

<p>
Using only the MLII lead with beat-level segmentation resulted in a substantial
performance improvement. Increasing the beat window size from 180 to 200 samples
on either side of the R-peak further enhanced accuracy, suggesting that additional
morphological information contributes positively to arrhythmia discrimination.
</p>

<h2>Conclusion</h2>

<p>
This project presents an effective approach for automated arrhythmia classification
using the MIT-BIH Arrhythmia Database. A beat-based segmentation strategy was employed
to extract fixed-length ECG segments centered on R-peaks, ensuring meaningful feature
representation of individual heartbeats.
</p>

<p>
Among the evaluated models, the Random Forest classifier consistently achieved the
highest performance. The best accuracy of <strong>99.6368%</strong> was obtained using
only the MLII lead with a window size of 200 samples on either side of the R-peak. This
demonstrates that high classification accuracy can be achieved using a single ECG
lead when appropriate feature extraction and ensemble learning techniques are applied.
</p>

<p>
The results indicate that beat window size plays a critical role in capturing ECG
morphology, and that Random Forest classifiers are well-suited for arrhythmia
detection tasks. Future work may explore deep learning models, feature optimization,
and real-time implementation for clinical decision support systems.
</p>

