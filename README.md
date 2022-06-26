# Trained Deep Neural Network for evaluating XZ redshift estimates

In this repository, you can find the fitted Multi-Layer Perceptron (MLP) neural network model produced by [**Sicilian et al. (2022)**](https://arxiv.org/abs/2203.13825). Below are instructions for loading and using the model to classify XZ redshift values as either good (+) or bad (-) estimates of the true redshift.

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

```python
classifier = joblib.load("fitted_FINAL_MLP.sav")
```

## Preparing your data

```python
# Load your csv as a DataFrame
df = pd.read_csv(your_data_csv_filename)

# Rename the columns
df = df.rename(columns={})

# Rescale the values
sc = StandardScaler()
df = sc.fit_transform(df)
```

## Applying the model to your data

```python
y_pred = classifier.predict(df)
```
