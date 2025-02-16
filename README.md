# Restaurant-tips-analysis (Python - Pandas, Matplotlib.pyplot)
Restaurant tips analysis sample project.

In this project, I analyze restaurant tipping patterns using data from this dataset: "https://raw.githubusercontent.com/RusAbk/sca_datasets/main/tips.csv". The goal is to uncover insights into how various factors—such as smoking preference, gender, and mealtime—affect tipping behavior.

### Stage 1: Import and examine the data set.

#### Import data:

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

#### Show the columns of the dataframe and their types:

Input:
```python
print(df.dtypes)
```

Output:
| Column Name  | Data Type |
|-------------|-----------|
| id          | int64     |
| total_bill  | float64   |
| tip         | float64   |
| sex         | object    |
| smoker      | object    |
| day         | object    |
| time        | object    |
| size        | int64     |

We have string columns considered as objects at [sex], [smoker], [day], [time].
Let's fix their types and make them string:

Input:
```python
columns_to_convert = ['sex', 'smoker', 'day', 'time']
df[columns_to_convert] = df[columns_to_convert].astype(pd.StringDtype())
print(df.dtypes)
```
Output:
| Column Name  | Data Type         |
|-------------|------------------|
| id          | int64            |
| total_bill  | float64          |
| tip         | float64          |
| sex         | string (Python)  |
| smoker      | string (Python)  |
| day         | string (Python)  |
| time        | string (Python)  |
| size        | int64            |

#### Basic descriptive statistics

Input:
```python
df.describe()
```

Output:
| Null  | id        | total_bill | tip      | size     |
|------------|----------|------------|----------|----------|
| count      | 244.000  | 244.000    | 244.000  | 244.000  |
| mean       | 121.500  | 19.786     | 2.998    | 2.570    |
| std        | 70.581   | 8.902      | 1.384    | 0.951    |
| min        | 0.000    | 3.070      | 1.000    | 1.000    |
| 25%        | 60.750   | 13.348     | 2.000    | 2.000    |
| 50%        | 121.500  | 17.795     | 2.900    | 2.000    |
| 75%        | 182.250  | 24.128     | 3.563    | 3.000    |
| max        | 243.000  | 50.810     | 10.000   | 6.000    |

### Stage 2: Hypothesis testing

#### 🚬 Do people who smoke give more tips?

In this analysis, I will compare the measures of central tendency of tip values between non-smokers and smokers.
```python
#Create a new dataframe smokers_df containing only info about smokers.
smokers_df = df[df['smoker'] == 'Yes']

#Create a new dataframe non_smokers_df containing only non-smokers.
non_smokers_df = df[df['smoker'] == 'No']

#smokers_df measures of central tendency
smokers_tip_min = smokers_df['tip'].min()
smokers_tip_max = smokers_df['tip'].max()
smokers_tip_mean = smokers_df['tip'].mean()
smokers_tip_median = smokers_df['tip'].median()

#non_smokers_df measures of central tendency
non_smokers_tip_min = non_smokers_df['tip'].min()
non_smokers_tip_max = non_smokers_df['tip'].max()
non_smokers_tip_mean = non_smokers_df['tip'].mean()
non_smokers_tip_median = non_smokers_df['tip'].median()

#whole dataset df measures of central tendency
common_tip_min = df['tip'].min()
common_tip_max = df['tip'].max()
common_tip_mean = df['tip'].mean()
common_tip_median = df['tip'].median()

#Create table to compare
data_smoker_non_smoker = {
    "Common": [df["tip"].min(), df["tip"].max(), df["tip"].mean(), df["tip"].median()],
    "Smokers": [smokers_df["tip"].min(), smokers_df["tip"].max(), smokers_df["tip"].mean(), smokers_df["tip"].median()],
    "Non-Smokers": [non_smokers_df["tip"].min(), non_smokers_df["tip"].max(), non_smokers_df["tip"].mean(), non_smokers_df["tip"].median()]
} # Create a dictionary with the statistics
index_labels = ["Min", "Max", "Mean", "Median"] # Define row labels
smoking_compare_df = pd.DataFrame(ata_smoker_non_smoker, index=index_labels) # Create the DataFrame
smoking_compare_df
```
Output:
|  | Common  | Smokers  | Non-Smokers |
|------------|---------|----------|-------------|
| Min       | 1.000   | 1.000    | 1.000       |
| Max       | 10.000  | 10.000   | 9.000       |
| Mean      | 2.998   | 3.009    | 2.992       |
| Median    | 2.900   | 3.000    | 2.740       |

#### Histogram

Input:
```python
fig, axes = plt.subplots(1, 3, figsize=(45, 5))
# Chart 1: Whole dataset tip values
axes[0].hist(df['tip'], bins = 20, color='#74b9ff')
axes[0].set_xlabel('Tip value')
axes[0].set_ylabel('Frequency')
axes[0].set_title('Whole dataset tip values')
axes[0].grid(True)
# Chart 2: Smokers tip values
axes[1].hist(smokers_df['tip'], bins = 20, color='#ff7675')
axes[1].set_xlabel('Tip value')
axes[1].set_ylabel('Frequency')
axes[1].set_title('Smokers tip values')
axes[1].grid(True)
# Chart 3: Non-smokers tip values
axes[2].hist(non_smokers_df['tip'], bins = 20, color='#55efc4')
axes[2].set_xlabel('Tip value')
axes[2].set_ylabel('Frequency')
axes[2].set_title('Non-smokers tip values')
axes[2].grid(True)

plt.tight_layout()
plt.show()
```

Output:
![image](https://github.com/user-attachments/assets/af671806-04e0-4f95-b10e-d7552d814134)

