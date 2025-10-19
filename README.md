**Investigating a Hybrid LSTM-Oscillatory Network (LSTM-ONN) for ECG Classification**

This project details the research and development of a novel hybrid neural network architecture, the LSTM-ONN. This model is designed to effectively classify 12-lead ECG signals by combining the sequential processing power of LSTMs with the principles of oscillatory dynamics.

The model is applied to the challenging task of distinguishing between a healthy "Athlete's Heart" and pathological Hypertrophic Cardiomyopathy (HCM) using an extremely small medical dataset (N=57). The project's key contribution is the demonstration that while purely oscillatory networks are too unstable for this task, a hybrid approach (using an LSTM as a feature extractor for an oscillatory-style classifier) is a robust and novel solution.

**Problem Statement** : 
Hypertrophic Cardiomyopathy (HCM) is a genetic heart condition that can be difficult to distinguish from the benign cardiac remodeling that occurs in trained athletes ("Athlete's Heart"). Differentiating these two conditions is critical, as a misdiagnosis can be fatal or needlessly end an athlete's career.
This project uses this classification problem as a testbed for a novel hybrid neural network architecture.

**Datasets** : 
The dataset is a small, combined cohort of 57 patients:
Athlete Data (N=28): Sourced from the Norwegian Athlete ECG Database.
HCM Data (N=29): Sourced from the PTB-XL Clinical ECG Database, filtered for the HYP (hypertrophy) diagnostic code.

**Preprocessing** : 
All signals were resampled to a uniform length of 5,000 time steps.
All 12 leads were used, resulting in an input shape of (5000, 12).
Data was scaled using StandardScaler after splitting.

**Iteration 1: OscillatoryRNN (The "Pure" Novel Model)** : 
The initial hypothesis was that a recurrent network based entirely on coupled oscillator dynamics could model the ECG signal.
Concept: A custom-built recurrent model where the phases of the oscillators at step t serve as the hidden state to compute the phases at step t+1.
Result: Model Failure. Even after extensive hyperparameter tuning, the OscillatoryRNN was too complex and dynamically unstable for the N=57 dataset. It consistently failed to learn meaningful patterns and collapsed into a single-class dominance (e.g., accuracy ~39-44%), proving the architecture was unsuitable in its pure form.

**Iteration 2: The Hybrid LSTM-ONN (The Successful Solution)** : 
The failure of the pure ORNN motivated a hybrid solution. This model combines the best of both worlds: the proven feature extraction power of a standard LSTM with the novelty of an oscillatory-inspired classification layer.
Concept: A hybrid, Keras-based model.
Feature Extractor: A standard LSTM layer first processes the raw (5000, 12) ECG sequence. It learns to find relevant temporal patterns and outputs a single, information-rich feature vector that summarizes the entire signal.
