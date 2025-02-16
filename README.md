# Restaurant-tips-analysis (Python - Pandas, Matplotlib.pyplot)
Restaurant tips analysis sample project.

In this project, I analyze restaurant tipping patterns using data from this dataset: "https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv". The goal is to uncover insights into how various factors—such as smoking preference, gender, and mealtime—affect tipping behavior.

### Stage 1: Import and examine the data set.

Input:
```python
#Import the libraries
import pandas as pd
import matplotlib.pyplot as plt

#Import the dataset into dataframe (df)
df = pd.read_csv("https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv")

# Display the first few rows of the dataset
print(df.head())
```
Output:
| id | total_bill | tip  | sex    | smoker | day  | time   | size |
|----|-----------|------|--------|--------|------|--------|------|
|  0 | 16.99     | 1.01 | Female | No     | Sun  | Dinner | 2    |
|  1 | 10.34     | 1.66 | Male   | No     | Sun  | Dinner | 3    |
|  2 | 21.01     | 3.50 | Male   | No     | Sun  | Dinner | 3    |
|  3 | 23.68     | 3.31 | Male   | No     | Sun  | Dinner | 2    |
|  4 | 24.59     | 3.61 | Female | No     | Sun  | Dinner | 4    |
![image](https://github.com/user-attachments/assets/a9dca1eb-88d3-477a-bb7a-c600e3cb4f7c)

