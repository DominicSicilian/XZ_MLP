# Trained deep neural network for evaluating XZ redshift estimates

In this repository, you can find the fitted Multi-Layer Perceptron (MLP) neural network model produced by [**Sicilian et al. (2022)**](https://arxiv.org/abs/2203.13825). Below are instructions for loading and using the model to classify XZ redshift values as either good or bad estimates of the true redshift.

## Dependencies

To load and apply the model, you will need [joblib](https://joblib.readthedocs.io/en/latest/), [pandas](https://pandas.pydata.org/), and [scikit-learn](https://scikit-learn.org/stable/index.html). Install with:

 ```bash
 $ pip install joblib pandas scikit-learn
 ```
 
 Then import the following into your code:
 
 ```python
 import joblib
 import pandas as pd
 from sklearn.preprocessing import StandardScaler
 ```

## Loading the model

Use ```joblib``` to load the fitted classifer from ```fitted_FINAL_MLP.sav```.

```python
classifier = joblib.load("fitted_FINAL_MLP.sav")
```

## Preparing your data

Load a dataset of XZ-analyzed sources containing the relevant features (see the feature set described in [**Sicilian et al. 2022**](https://arxiv.org/abs/2203.13825)). It is most convenient to use a CSV-formatted file, and to read it in as a ```pandas``` DataFrame. The names of the features should be renamed to match the names found in the example below. Lastly, the values should be rescaled using the ```scikit-learn``` standard scaling technique, which adjusts each feature to have a mean of 0 and a variance of 1, as this was done on the training data.

```python
# Load your csv as a DataFrame
df = pd.read_csv(your_data_csv_filename_string)

# Rename the columns
df = df.rename(columns={
    your_XZ_redshift_column_name: "XZ",
    your_XZ_logNH_column_name: "XZ_NH",
    your_sigma_z_column_name: "sigma_z",
    your_sigma_logNH_column_name: "sigma_nh",
    your_reduced_CSTAT_column_name: "CSTATv",
    your_Information_Gain_column_name: "IG",
    your_Counts_column_name: "counts"
    })

# Rescale the values
sc = StandardScaler()
df = sc.fit_transform(df)
```

## Applying the model to your data

To predict the classes for each source, call the classifier's ```predict``` method (which will return ```1``` for a good XZ redshift estimate and ```0``` for a poor estimate). To obtain the model's estimated probabilities associated with each class, call the ```predict_proba``` method.

```python
# Get the predicted class for each source
y_pred = classifier.predict(df)

# Get the corresponding probabilities
y_proba = classifier.predict_proba(df)
```
