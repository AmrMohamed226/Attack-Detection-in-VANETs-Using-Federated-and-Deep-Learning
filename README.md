# Attack Detection in VANETs Using Federated and Deep Learning

This project focuses on enhancing the security of Vehicular Ad Hoc Networks (VANETs) by using both federated learning and deep learning techniques to detect and classify various attack types. We leverage the VeReMi dataset to implement and evaluate our models, aiming to improve the robustness of communications in Connected and Automated Vehicles (CAVs).

## Dataset
The VeReMi dataset, developed specifically for V2X security testing, provides a comprehensive and realistic simulation of vehicular communications and their potential security threats. The dataset includes various attack and malfunction scenarios, making it suitable for evaluating misbehavior detection systems.

### Dataset Description
The VeReMi dataset, extended from its original version, includes realistic sensor error models, a new set of attacks, and a larger number of data points. The `MixAll_0024` scenario used in this project spans an entire simulation day (24 hours), providing a rich dataset for training and evaluating machine learning models.

We specifically use the `VeReMi_64800_68400_2022-9-11_12.51.1` scenario from the VeReMi dataset, compromising of 2260 different json files (datasets).

**Key Features of the VeReMi Dataset:**
- The dataset is generated using the VEINS simulator, which integrates OMNeT++ and SUMO for network and road traffic simulation.
- Position, velocity, acceleration, and heading data include realistic errors to reflect real-world conditions.
- Includes 19 different attack and malfunction types:
    - 0: Normal
    - 1: ConstPos
    - 2: ConstPosOffset
    - 3: RandomPos
    - 4: RandomPosOffset
    - 5: ConstSpeed
    - 6: ConstSpeedOffset
    - 7: RandomSpeed
    - 8: RandomSpeedOffset
    - 9: EventualStop
    - 10: Disruptive
    - 11: DataReplay
    - 12: DelayedMessages
    - 13: DoS
    - 14: DoSRandom
    - 15: DoSDisruptive
    - 16: GridSybil
    - 17: DataReplaySybil
    - 18: DoSRandomSybil
    - 19: DoSDisruptiveSybil

For more details, refer to the [VeReMi Dataset Description](https://ieeexplore.ieee.org/document/9149132).

### Data Labeling
In this dataset, the labels for each vehicle indicate whether the vehicle is an attacker and the type of attack. The labels are extracted from the JSON file names, which include information about the vehicle number, OMNET++ module number, and the attack type. For example, `JSONlog-15-13-A13-25207-7.json` refers to the 15th vehicle with OMNET++ module ID 13 and attack type 13 (DoS).

## Data Preprocessing
Data preprocessing involves several steps:
- Extracting JSON logs from zipped files.
- Parsing JSON files to extract key data points such as message ID, timestamps, sender information, and vehicle dynamics.
- Structuring extracted features into a tabular format suitable for machine learning models.
- Identifying attack types based on the naming convention of JSON files.
- Applying SMOTE to balance the dataset by creating synthetic examples for minority classes.

## Models

### Deep Learning Models
We implemented two deep learning models to detect and classify attack types in VANETs:
- **Feedforward Neural Network (FFNN):**
  - Input Layer
  - Three hidden layers with ReLU activation
  - Dropout layers with a rate of 0.3
  - Output layer with softmax activation
- **Long Short-Term Memory (LSTM) Network:**
  - LSTM layers with dropout to handle temporal data
  - Output layer with softmax activation

### Federated Learning Models
Federated learning techniques were employed to enhance privacy and security:
- **Binary Classification:** Predicts whether a communication is an attack or not.
- **Multiclass Classification:** Identifies the specific type of attack.
- Models used include Naive Bayes, Decision Trees, Logistic Regression, and Random Forest. 
- 2260 models per model type were fit over the 2260 datasets before weight aggregation.

## Model Training
We used Python scripts to train the models in both centralized and federated learning setups:
- **Deep Learning:** FFNN and LSTM models trained on centralized data with class imbalance handled by SMOTE.
- **Federated Learning:** Models trained on decentralized data with updates aggregated using federated averaging.  

## Evaluation Metrics
The models were evaluated using standard metrics:
- **Accuracy**
- **Precision**
- **Recall**
- **F1 Score**

## Results

Initial results indicate that the LSTM model outperforms the FFNN model in most metrics, particularly in handling sequential data. Undersampling improved precision and F1 scores but reduced overall accuracy.

Federated learning models showed remarkable performance, especially in maintaining privacy. Random Forest and Decision Trees outperformed other models, demonstrating their suitability for federated learning environments.


## Discussion
Federated learning demonstrated superior performance compared to deep learning models. This can be attributed to the more homogeneous distribution of data per vehicle, which allows each model to better learn local distributions per vehicle. Aggregating all the models' weights helps generalize the model on different aspects of the data, improving overall accuracy and robustness.

The results highlight the benefits of federated learning for privacy-preserving applications. Implementing federated learning in vehicular networks can enhance security and accuracy while protecting user privacy. The decentralized approach of federated learning allows models to learn from local data distributions, leading to better generalization when aggregated over deep learning models.