# Data Cleaning Beginner

A series of tutorials on Data Cleaning and Processing

## Tutorials

1. [The complete beginner’s guide to data cleaning and preprocessing](https://towardsdatascience.com/the-complete-beginners-guide-to-data-cleaning-and-preprocessing-2070b7d4c6d) by [Annie Bonner](https://www.linkedin.com/in/contentsimplicity/?challengeId=AQHmwMSTsQYcZAAAAXLyZ7cpKbroCaomb_QZBJdpixQua482LOFNe9QZv2KBxKPkz6wcLwEpOi6kP5Vu-LcxDiTIpLfOC2M1jw&submissionId=1e12fbf4-4e33-1c16-56e6-b0908afde643)
2. Next

## Notes

1. Load data

```python
dataset = pd.read_csv("my_data.csv")
dataset
```

```markdown

| num  | Animal |  Age  |  Worth  | Friendly |
| :--- | :----: | :---: | :-----: | -------: |
| 0    |  Cat   |  4.0  | 72000.0 |       No |
| 1    |  Dog   | 17.0  | 48000.0 |      Yes |
| 2    | Moose  |  6.0  | 54000.0 |       No |
| 3    |  Dog   |  8.0  | 61000.0 |       No |
| 4    | Moose  |  4.0  |   NaN   |      Yes |
| 5    |  Cat   | 15.0  | 58000.0 |      Yes |
| 6    |  Dog   |  NaN  | 52000.0 |       No |
| 7    |  Cat   | 12.0  | 79000.0 |      Yes |
| 8    | Moose  |  5.0  | 83000.0 |       No |
| 9    |  Cat   |  7.0  | 67000.0 |      Yes |
```

2. Create independen partiiton using pandas dataframe *`.iloc[-range-].values`*, which returns a numpy array

```python
x = dataset.iloc[:, :-1].values
x
```

```python
array(
    [
        ["Cat", 4.0, 72000.0],
        ["Dog", 17.0, 48000.0],
        ["Moose", 6.0, 54000.0],
        ["Dog", 8.0, 61000.0],
        ["Moose", 4.0, nan],
        ["Cat", 15.0, 58000.0],
        ["Dog", nan, 52000.0],
        ["Cat", 12.0, 79000.0],
        ["Moose", 5.0, 83000.0],
        ["Cat", 7.0, 67000.0],
    ],
    dtype=object,
)
```

3. Create dependent dataset partition

```python
y = dataset.iloc[:, -1].values
y
```

```python
array(["No", "Yes", "No", "No", "Yes", "Yes", "No", "Yes", "No", "Yes"], dtype=object)
```

4. Impute, or replace, missing values
   1. Import from `from sklearn.impute import SimpleImputer`
   2. SimpleImputer takes missing values and a replacement strategy as inputs

```python
imputer = SimpleImputer(missing_values=np.nan, strategy="most_frequent")
imputer = imputer.fit(x[:, 1:3])
```

   3. Replacement strategy can be:
      1. Mean
      2. Median
      3. Most Frequent
      4. Constant
5. Use imputer to fill in the missing values using values generated using data from the rest of the column

```python
x[:, 1:3] = imputer.transform(x[:, 1:3])
x
```

```python
array(
    [
        ["Cat", 4.0, 72000.0],
        ["Dog", 17.0, 48000.0],
        ["Moose", 6.0, 54000.0],
        ["Dog", 8.0, 61000.0],
        ["Moose", 4.0, 48000.0],
        ["Cat", 15.0, 58000.0],
        ["Dog", 4.0, 52000.0],
        ["Cat", 12.0, 79000.0],
        ["Moose", 5.0, 83000.0],
        ["Cat", 7.0, 67000.0],
    ],
    dtype=object,
)
```

6. Categorical data isn't understood by algorithms, so we convert it into numerical ratios
7. We use label encoder to encode the column specified

```python
labelencoder_x = LabelEncoder()
x[:, 0] = labelencoder_x.fit_transform(x[:, 0])
x
```

```python
array(
    [
        [0, 4.0, 72000.0],
        [1, 17.0, 48000.0],
        [2, 6.0, 54000.0],
        [1, 8.0, 61000.0],
        [2, 4.0, 48000.0],
        [0, 15.0, 58000.0],
        [1, 4.0, 52000.0],
        [0, 12.0, 79000.0],
        [2, 5.0, 83000.0],
        [0, 7.0, 67000.0],
    ],
    dtype=object,
)
```

8. However, these series of 1,2,3 could be misinterpretated as actually incremental values by the learner module, instead of the categorical values they are.
   1. insert about above from article
9.  Thus, we use OneHotEncoder to split the column into columns of binary variables, one column each for each category

```python
x_labeled = x[:, 0]
x_labeled
onehotencoder_x = OneHotEncoder(handle_unknown="ignore")
x_labeled = x_labeled.reshape(-1, 1)
x_encoded = onehotencoder_x.fit_transform(x_labeled)
x_encoded = x_encoded.toarray()
x_encoded
x = np.concatenate([x_encoded, x[:, 1:]], axis=1)
x
```

```python
array([0, 1, 2, 1, 2, 0, 1, 0, 2, 0], dtype=object)
```

```python
array(
    [
        [1.0, 0.0, 0.0],
        [0.0, 1.0, 0.0],
        [0.0, 0.0, 1.0],
        [0.0, 1.0, 0.0],
        [0.0, 0.0, 1.0],
        [1.0, 0.0, 0.0],
        [0.0, 1.0, 0.0],
        [1.0, 0.0, 0.0],
        [0.0, 0.0, 1.0],
        [1.0, 0.0, 0.0],
    ]
)
```

```python
array(
    [
        [1.0, 0.0, 0.0, 4.0, 72000.0],
        [0.0, 1.0, 0.0, 17.0, 48000.0],
        [0.0, 0.0, 1.0, 6.0, 54000.0],
        [0.0, 1.0, 0.0, 8.0, 61000.0],
        [0.0, 0.0, 1.0, 4.0, 48000.0],
        [1.0, 0.0, 0.0, 15.0, 58000.0],
        [0.0, 1.0, 0.0, 4.0, 52000.0],
        [1.0, 0.0, 0.0, 12.0, 79000.0],
        [0.0, 0.0, 1.0, 5.0, 83000.0],
        [1.0, 0.0, 0.0, 7.0, 67000.0],
    ],
    dtype=object,
)
```

   2. Create encoder object
   3. Reshape row into column
   4. Perform fit and transform on data
   5. Convert to array
   6. Concatenate to x
10. Data should be of the same scale, otherwise the manhattan distance between different attributes will lead the model to place greater importance than not on attributes with unusual scales, and relationships between attributes with a great scale difference
    1. insert about above from article
    2. Scaled data will bring all the data in the same ranges
    3. it's not advised to do this for binary or dummy variables, since this might significantly change their meaning
    4. insert about above from article
11. Scale data using StandardScaler

```python
sc_x = StandardScaler()
x_sc = x[:, 3:]
x_sc
x_sc = sc_x.fit_transform(x_sc)
x_sc
x = np.concatenate([x[:, :3], x_sc], axis=1)
x
```

```python
array(
    [
        [4.0, 72000.0],
        [17.0, 48000.0],
        [6.0, 54000.0],
        [8.0, 61000.0],
        [4.0, 48000.0],
        [15.0, 58000.0],
        [4.0, 52000.0],
        [12.0, 79000.0],
        [5.0, 83000.0],
        [7.0, 67000.0],
    ],
    dtype=object,
)
```

```python
array(
    [
        [-0.92179769, 0.82020574],
        [1.93138564, -1.18846138],
        [-0.48284641, -0.6862946],
        [-0.04389513, -0.10043336],
        [-0.92179769, -1.18846138],
        [1.49243436, -0.35151675],
        [-0.92179769, -0.85368353],
        [0.83400743, 1.40606699],
        [-0.70232205, 1.74084484],
        [-0.26337077, 0.40173343],
    ]
)
```

```python
array(
    [
        [1.0, 0.0, 0.0, -0.9217976907429086, 0.8202057433801607],
        [0.0, 1.0, 0.0, 1.931385637747047, -1.188461383265131],
        [0.0, 0.0, 1.0, -0.4828464094367616, -0.686294601603808],
        [0.0, 1.0, 0.0, -0.043895128130614545, -0.10043335633226458],
        [0.0, 0.0, 1.0, -0.9217976907429086, -1.188461383265131],
        [1.0, 0.0, 0.0, 1.4924343564409002, -0.351516747162926],
        [0.0, 1.0, 0.0, -0.9217976907429086, -0.8536835288242489],
        [1.0, 0.0, 0.0, 0.8340074344816796, 1.406066988651704],
        [0.0, 0.0, 1.0, -0.7023220500898352, 1.740844843092586],
        [1.0, 0.0, 0.0, -0.2633707687836881, 0.4017334253290583],
    ],
    dtype=object,
)
```

    1. Create scaler object
    2. partition out range of variables that need scaling
    3. Perform fit and transform on data
    4. Concatenate scaled data to rest of the dataset

## Output

```markdown

```

## Figures

Add figures

## Project status

Project is currently under development.
