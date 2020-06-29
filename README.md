# Data Cleaning Beginner

A series of tutorials on Data Cleaning and Processing

## Tutorials

1. [The complete beginner’s guide to data cleaning and preprocessing](https://towardsdatascience.com/the-complete-beginners-guide-to-data-cleaning-and-preprocessing-2070b7d4c6d) by [Annie Bonner](https://www.linkedin.com/in/contentsimplicity/?challengeId=AQHmwMSTsQYcZAAAAXLyZ7cpKbroCaomb_QZBJdpixQua482LOFNe9QZv2KBxKPkz6wcLwEpOi6kP5Vu-LcxDiTIpLfOC2M1jw&submissionId=1e12fbf4-4e33-1c16-56e6-b0908afde643)
2. Next

## Notes

1. Load data
2. Create independen partiiton using pandas dataframe *`.iloc[-range-].values`*, which returns a numpy array
3. Create dependent dataset partition
4. Impute, or replace, missing values
   1. Import from `from sklearn.impute import SimpleImputer`
   2. SimpleImputer takes missing values and a replacement strategy as inputs
   3. Replacement strategy can be:
      1. Mean
      2. Median
      3. Most Frequent
      4. Constant
5. Use imputer to fill in the missing values using values generated using data from the rest of the column
6. Categorical data isn't understood by algorithms, so we convert it into numerical ratios
7. We use label encoder to encode the column specified
8. However, these series of 1,2,3 could be misinterpretated as actually incremental values by the learner module, instead of the categorical values they are.
   1. insert about above from article
9. Thus, we use OneHotEncoder to split the column into columns of binary variables, one column each for each category
   1. Create encoder object
   2. Reshape row into column
   3. Perform fit and transform on data
   4. Convert to array
   5. Concatenate to x
10.
11. More Notes

```python
```

## Output

```markdown

```

## Figures

Add figures

## Project status

Project is currently under development.
