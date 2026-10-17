# Explainable Bayesian Network for Probabilistic Crop Risk Assessment Under Environmental Uncertainty

A probabilistic crop recommendation project that uses a **Bayesian Network (BN)** to estimate crop-class probabilities from soil and environmental conditions. The project evaluates predictive performance, examines how environmental evidence changes posterior crop probabilities, and summarizes prediction uncertainty.

## Project Overview

Crop suitability depends on interacting factors such as soil nutrients, temperature, humidity, soil pH, and rainfall. A conventional classifier often returns a single predicted crop, while a Bayesian Network can represent dependencies between variables and estimate a probability distribution over possible crop classes.

This project builds and evaluates a discretized Bayesian Network using seven environmental features and 22 crop classes. It also compares the model with conventional machine-learning classifiers and studies selected rainfall scenarios.

> **Important result:** In the reported 440-sample holdout test set, the Bayesian Network achieved **75.68% accuracy** and a **0.73 macro F1-score**. Random Forest, Extra Trees, and Gaussian Naive Bayes each achieved **99.55% accuracy** on the same split. The Bayesian Network is therefore presented as a probabilistic and interpretable baseline, not as the most accurate model in this experiment.

## Objectives

- Build a Bayesian Network representing dependencies between environmental features and crop labels.
- Estimate posterior probabilities for 22 crop classes using Bayesian inference.
- Examine how changes in environmental evidence affect crop probability distributions.
- Measure prediction uncertainty using normalized Shannon entropy.
- Compare the Bayesian Network's predictive performance with conventional classifiers.

## Dataset

The experiment uses a crop recommendation dataset with **2,200 samples**, seven input features, and 22 crop classes.

| Property | Description |
|---|---|
| Total samples | 2,200 |
| Input features | 7 |
| Crop classes | 22 |
| Samples per class | 100 |
| Missing values reported | 0 |
| Exact duplicate rows reported | 0 |
| Training samples | 1,760 |
| Holdout test samples | 440 |
| Train-test split | Stratified 80:20, `random_state=42` |

### Input Features

- Nitrogen (`N`)
- Phosphorus (`P`)
- Potassium (`K`)
- Temperature
- Humidity
- Soil pH
- Rainfall

**Target:** Crop label (22 classes).

Dataset reference: [Crop Recommendation Dataset on Kaggle](https://www.kaggle.com/datasets/atharvaingle/crop-recommendation-dataset). Verify dataset provenance and feature construction before using the results in a real-world agricultural setting.

## Methodology

1. **Preprocessing:** Split the dataset using a stratified 80:20 train-test split.
2. **Feature discretization:** Convert each numerical feature into `Low`, `Medium`, and `High` categories. Quantile boundaries are calculated from the training data and then applied to the test data.
3. **Structure learning:** Learn the Bayesian Network structure with Hill Climbing search and the Bayesian Dirichlet equivalent uniform (BDeu) score. Edges from the crop label to environmental features are forbidden during structure learning.
4. **Parameter learning:** Estimate conditional probability distributions using Bayesian parameter estimation with Dirichlet pseudo-counts.
5. **Inference:** Use variable elimination to estimate posterior probabilities for the crop classes given environmental evidence.
6. **Prediction:** Select the crop class with the highest posterior probability.
7. **Evaluation:** Assess classification performance with accuracy, macro F1-score, weighted F1-score, and a confusion matrix.
8. **Sensitivity and uncertainty analysis:** Vary selected environmental evidence and examine posterior changes and normalized Shannon entropy.

The Bayesian Network represents the joint distribution as:

`P(X1, ..., Xn) = product of P(Xi | Parents(Xi))`

For observed environmental evidence `E`, inference estimates `P(Crop | E)`.

## Tools and Technologies

The project review identifies the following tools and libraries:

- Python
- Pandas
- NumPy
- scikit-learn
- Matplotlib
- Seaborn
- NetworkX
- Bayesian Network structure learning and inference libraries used in the implementation
- CSV dataset

## Results

### Bayesian Network Holdout Performance

| Metric | Result |
|---|---:|
| Test samples | 440 |
| Correct predictions | 333 |
| Accuracy | 75.68% |
| Macro F1-score | 0.73 |
| Weighted F1-score | 0.73 |

### Comparison with Conventional Classifiers

| Model | Accuracy | Macro F1-score | Weighted F1-score |
|---|---:|---:|---:|
| Random Forest | 99.55% | 0.9955 | 0.9955 |
| Gaussian Naive Bayes | 99.55% | 0.9954 | 0.9954 |
| Extra Trees | 99.55% | 0.9955 | 0.9955 |
| Gradient Boosting | 98.86% | 0.9887 | 0.9887 |
| RBF Support Vector Machine | 98.41% | 0.9840 | 0.9840 |
| Decision Tree | 97.95% | 0.9794 | 0.9794 |
| K-Nearest Neighbors | 97.95% | 0.9793 | 0.9793 |
| Logistic Regression | 97.27% | 0.9725 | 0.9725 |
| Bayesian Network | 75.68% | 0.7300 | 0.7300 |

These are the reported results from the project's evaluation. The unusually high baseline scores should be independently reproduced, and feature columns, label handling, and the train-test split should be checked before drawing broader conclusions.

### Rainfall Scenario Analysis

For the selected baseline evidence, the model reported the following outcomes:

| Rainfall state | Most probable crop | Posterior probability | Normalized entropy |
|---|---|---:|---:|
| Low | Blackgram | 33.61% | 0.5858 |
| Medium | Kidneybeans | 43.06% | 0.6682 |
| High | Pigeonpeas | 66.39% | 0.4407 |

The results show that the fitted model's posterior distribution changes when rainfall evidence changes. These are model-specific scenario results, **not proof of causal relationships or universal agricultural rules**.

### Feature Sensitivity

The maximum total variation distances reported for the selected baseline evidence were:

| Feature | Maximum total variation distance |
|---|---:|
| Phosphorus | 0.9578 |
| Potassium | 0.9459 |
| Humidity | 0.5997 |
| Rainfall | 0.5112 |
| Nitrogen | 0.0000 |
| Temperature | 0.0000 |
| Soil pH | 0.0000 |

A larger distance means the posterior crop distribution changed more under the tested feature-state variations. Zero change in this experiment does not mean that a feature is unimportant in real agriculture; the result may depend on the learned network, conditional probability tables, discretization, and selected evidence.

## Visualizations

The project includes or describes these evaluation visualizations:

- Learned Bayesian Network graph
- Confusion matrix
- Per-crop F1-score plot
- Rainfall scenario probability chart
- Feature sensitivity analysis
- Normalized entropy comparison

Add the generated figures to your repository and update this section with their actual paths if you want them displayed on GitHub.

Example:

```markdown
![Learned Bayesian Network](images/bayesian_network.png)
![Confusion Matrix](images/confusion_matrix.png)
![Rainfall Scenario Analysis](images/rainfall_scenarios.png)
```

## Repository Structure

Suggested organization for the GitHub repository (adjust it to match the files you actually upload):

```text
crop-risk-bayesian-network/
├── README.md
├── data/
│   └── crop_recommendation.csv
├── notebooks/
│   └── crop_risk_bayesian_network.ipynb
├── images/
│   ├── bayesian_network.png
│   ├── confusion_matrix.png
│   └── rainfall_scenarios.png
├── report/
│   └── Probability_Paper.pdf
└── requirements.txt
```

Only include files and folders that are actually present in your repository. Check the dataset's licence and terms before redistributing it.

## How to Run

The uploaded project review and paper describe the approach and results, but this README does not assume a specific notebook filename or exact package versions. Adapt the steps below to match the implementation you upload.

1. Clone your repository:

   ```bash
   git clone <YOUR_GITHUB_REPOSITORY_URL>
   cd <YOUR_REPOSITORY_FOLDER>
   ```

2. Create and activate a Python virtual environment (recommended):

   ```bash
   python -m venv .venv
   ```

   Windows:

   ```bash
   .venv\Scripts\activate
   ```

   macOS/Linux:

   ```bash
   source .venv/bin/activate
   ```

3. Install dependencies from the project's requirements file, if provided:

   ```bash
   pip install -r requirements.txt
   ```

4. Open the notebook in Jupyter and run its cells in order, or execute the Python entry point provided by your repository.

**Before publishing:** Add a `requirements.txt` containing the packages and versions your code actually uses, and document the real entry point and dataset path.

## Limitations

- The Bayesian Network uses a single stratified holdout split and three-bin feature discretization.
- Results may vary with discretization boundaries, scoring function, prior settings, and learned network structure.
- The dataset is balanced and relatively small; its provenance and real-world representativeness need verification.
- The current evaluation does not report Brier score, log loss, or probability calibration results for the Bayesian Network.
- Equivalent cross-validation for the Bayesian Network and independent external agricultural validation remain future work.
- Scenario sensitivity results do not establish causal agricultural effects.

## Future Improvements

- Evaluate the Bayesian Network across multiple random splits and cross-validation.
- Retain full posterior probability vectors and evaluate calibration, Brier score, and log loss.
- Verify the unusually high baseline scores and investigate possible data or evaluation issues.
- Test alternative discretization strategies and network structures.
- Validate the model using independent agricultural datasets and real field observations.
- Integrate real-time weather data and crop-failure or yield-loss observations.
- Improve explanations so users can understand which evidence most changes a prediction.
- Build a user interface for entering environmental values and viewing crop probabilities and uncertainty.
- 
## Contributors

- [Bhadresh RP]-(https://github.com/BHADHRESH) — Contributor
- [B. Cibi Kumar](https://github.com/Cibi-kumar777) — Contributor
- [Ch.Priya madhav](https://github.com/madhav1206) — Contributor
- [G.Rishi kanth](https://github.com/GeethamRishiKanth) — Contributor


## Team Members

**Course:** 22AIE301 — Probabilistic Reasoning  
**Group:** C-13  
**Department:** Artificial Intelligence, Amrita Vishwa Vidyapeetham, Coimbatore, Tamil Nadu, India

- **Bhadresh RP** — `CB.SC.U4AIE24208`
- **Ch. Priya Madhav** — `CB.SC.U4AIE24211`
- **B. Cibi Kumar** — `CB.SC.U4AIE24212`
- **G. Rishi Kanth** — `CB.SC.U4AIE24218`

**Mentor:** Dr. Navaneeth Haridasan

## References

1. N. Sharmin and C. Kiekintveld, “Optimizing Crop Recommendations for Sustainable Agriculture: Leveraging Bayesian Networks in a Smart Crop Recommendation System,” IEEE SECON, 2023. [https://doi.org/10.1109/SECON58729.2023.10287510](https://doi.org/10.1109/SECON58729.2023.10287510)
2. N. N. Thilakarathne et al., “A Cloud Enabled Crop Recommendation Platform for Machine Learning-Driven Precision Farming,” *Sensors*, 2022. [https://doi.org/10.3390/s22166299](https://doi.org/10.3390/s22166299)
3. F. S. Prity et al., “Enhancing Agricultural Productivity: A Machine Learning Approach to Crop Recommendations,” *Human-Centric Intelligent Systems*, 2024. [https://doi.org/10.1007/s44230-024-00081-3](https://doi.org/10.1007/s44230-024-00081-3)
4. S. Shastri et al., “Advancing Crop Recommendation System with Supervised Machine Learning and Explainable Artificial Intelligence,” *Scientific Reports*, 2025. [https://doi.org/10.1038/s41598-025-07003-8](https://doi.org/10.1038/s41598-025-07003-8)
5. M. S. B. Alam et al., “An Approach for Crop Recommendation with Uncertainty Quantification Based on Machine Learning for Sustainable Agricultural Decision-Making,” *Results in Engineering*, 2025. [https://doi.org/10.1016/j.rineng.2025.105505](https://doi.org/10.1016/j.rineng.2025.105505)
6. D. Heckerman, “A Tutorial on Learning with Bayesian Networks,” Microsoft Research Technical Report MSR-TR-95-06, 1995.
7. N. Friedman, D. Geiger, and M. Goldszmidt, “Bayesian Network Classifiers,” *Machine Learning*, vol. 29, pp. 131–163, 1997. [https://doi.org/10.1023/A:1007465528199](https://doi.org/10.1023/A:1007465528199)
8. D. Koller and N. Friedman, *Probabilistic Graphical Models: Principles and Techniques*. MIT Press, 2009.
9. A. Ingle, “Crop Recommendation Dataset,” Kaggle. [Dataset link](https://www.kaggle.com/datasets/atharvaingle/crop-recommendation-dataset)

---

## Acknowledgement

This project was completed as part of the **22AIE301 Probabilistic Reasoning** course project at Amrita Vishwa Vidyapeetham, Coimbatore.

## License

Choose and add a licence (for example, MIT) only if it is appropriate for your code and you have the rights to license the included materials. A licence for your code does not automatically grant permission to redistribute third-party datasets or papers.
