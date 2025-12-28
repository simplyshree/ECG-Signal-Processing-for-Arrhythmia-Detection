# Arrhythmia Detection
<h1>MIT-BIH Arrhythmia Classification using Random Forest</h1>


<h2>1. Dataset Description</h2>
<p>
The MIT-BIH Arrhythmia Database is a widely used benchmark dataset for evaluating
ECG-based arrhythmia classification algorithms. Each record contains continuous ECG
signals sampled at 360 Hz along with annotation files that specify the occurrence and
type of each heartbeat. In this project, only the MLII lead is used for analysis, as it
provides clear QRS complexes and is commonly adopted in clinical studies.
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
Stratified sampling was applied to preserve the original class distribution in both
sets, ensuring unbiased model evaluation.
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

<h2>10. Model Evaluation</h2>
<p>
The trained model was evaluated on the test dataset using multiple performance
metrics:
</p>

<ul>
  <li>Overall classification accuracy</li>
  <li>Confusion matrix</li>
</ul>

<p>
These metrics provide a comprehensive assessment of the model’s ability to correctly
classify different types of arrhythmias.
</p>

<h2>11. Experimental Setup</h2>
<p>
To analyze the effect of beat window size on classification performance, experiments
were conducted using different window lengths. A window size of 200 samples on
either side of the R-peak was selected to capture sufficient heartbeat morphology
while minimizing noise.
</p>

<h2>12. Workflow Summary</h2>
<ol>
  <li>Load ECG signals and annotation files</li>
  <li>Select the MLII ECG lead</li>
  <li>Extract fixed-length heartbeat segments around R-peaks</li>
  <li>Map beat symbols to standard arrhythmia classes</li>
  <li>Combine heartbeat segments from all records</li>
  <li>Split the dataset into training and testing sets</li>
  <li>Train the Random Forest classifier</li>
  <li>Evaluate model performance</li>
</ol>

