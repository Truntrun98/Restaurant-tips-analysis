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

### 🚬 Do people who smoke give more tips?

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
### Insights and Conclusions on Tipping Behavior: Smokers vs. Non-Smokers

#### Insight 1: Smokers Tend to Tip Slightly More Than Non-Smokers

- The mean tip value for smokers is slightly higher (3.00871) compared to non-smokers (2.991854).

- The median tip value for smokers (3.00) is also higher than that of non-smokers (2.74).

- This indicates that, on average, smokers tend to tip more than non-smokers.

#### Insight 2: Distribution Range of Tips

- The range of tips (difference between max and min values) is the same for both the common dataset and smokers (9.000).

- For non-smokers, the maximum tip is slightly lower (9.000), suggesting that non-smokers have a narrower range of tip values.

- Smokers contribute to the higher end of the tip distribution.

### Histogram Observations

- The histogram for smokers shows a higher frequency at the upper end of the tip values compared to non-smokers.

- Both histograms share a similar pattern at the lower end, indicating that the starting point for tipping is consistent across both groups.

### General Conclusion

- Smokers, on average, tend to tip slightly more than non-smokers. This is reflected in both the mean and median tip values, which are higher for smokers compared to non-smokers. The mean tip value for smokers is approximately 3.01, while it is around 2.99 for non-smokers. Similarly, the median tip value for smokers is 3.00 compared to 2.74 for non-smokers.

- The visual representation highlights the higher frequency of larger tips among smokers. This insight can help businesses and service providers better understand customer behavior and potentially tailor their services accordingly.

### 👨👩 Do males give more tips?
In this question, I will perform the same steps as in the previous question: creating males_df and females_df, generating their histograms, and comparing them.

Input:
```Python
#Creating new df for sex:
males_df = df[df['sex'] == 'Male']
females_df = df[df['sex'] == 'Female']

#Calculate central tendency:


males_tip_min = males_df['tip'].min()
males_tip_max = males_df['tip'].max()
males_tip_mean = males_df['tip'].mean()
males_tip_median = males_df['tip'].median()

females_tip_min = females_df['tip'].min()
females_tip_max = females_df['tip'].max()
females_tip_mean = females_df['tip'].mean()
females_tip_median = females_df['tip'].median()

#Create new data frame for central tendency:
common_males_females_tip = {
    'Common': [common_tip_min, common_tip_max, common_tip_mean, common_tip_median],
    'Males': [males_tip_min, males_tip_max, males_tip_mean, males_tip_median],
    'Females': [females_tip_min, females_tip_max, females_tip_mean, females_tip_median]
}

index_labels = ["Min", "Max", "Mean", "Median"]
df_common_males_females_tip = pd.DataFrame(common_males_females_tip, index=index_labels)

#Create chart
fig, axis = plt.subplots(1, 3, figsize=(45, 5))
# Chart 1: Whole dataset tip values
axis[0].hist(df['tip'], bins = 20, color='#74b9ff')
axis[0].set_xlabel('Tip value')
axis[0].set_ylabel('Frequency')
axis[0].set_title('Whole dataset tip values')
axis[0].grid(True)
# Chart 2: Males tip values
axis[1].hist(males_df['tip'], bins = 20, color='#ff7675')
axis[1].set_xlabel('Tip value')
axis[1].set_ylabel('Frequency')
axis[1].set_title('Males tip values')
axis[1].grid(True)
# Chart 3: Females tip values
axis[2].hist(females_df['tip'],  bins = 20,color='#55efc4')
axis[2].set_xlabel('Tip value')
axis[2].set_ylabel('Frequency')
axis[2].set_title('Females tip values')
axis[2].grid(True)

plt.tight_layout()
plt.show()

#Print data frame
df_common_males_females_tip
```
Output:
![image](https://github.com/user-attachments/assets/4355ae45-5f52-4432-9dae-ddfa58819129)
| | Common  | Males   | Females  |
|------------|---------|---------|----------|
| Min       | 1.000   | 1.000   | 1.000    |
| Max       | 10.000  | 10.000  | 6.500    |
| Mean      | 2.998   | 3.090   | 2.833    |
| Median    | 2.900   | 3.000   | 2.750    |

### Insights and Conclusions on Tipping Behavior: Males vs. Females

#### Insight 1: Males Tend to Tip Slightly More Than Females

- The mean tip value for males is slightly higher (3.090) compared to females (2.833).

- The median tip value for males (3.00) is also higher than that of females (2.75).

- This indicates that, on average, males tend to tip more than females.

#### Insight 2: Distribution Range of Tips

- The range of tips (difference between max and min values) is the same for the common dataset and males (10.000).

- For females, the maximum tip is lower (6.500), suggesting that females have a narrower range of tip values.

- Males contribute to the higher end of the tip distribution.

#### Histogram Observations

- The histogram for males shows a higher frequency at the upper end of the tip values compared to females.

- Both histograms share a similar pattern at the lower end, indicating that the starting point for tipping is consistent across both groups.

#### General Conclusion

- Males, on average, tend to tip slightly more than females. This is reflected in both the mean and median tip values, which are higher for males compared to females. The mean tip value for males is approximately 3.09, while it is around 2.83 for females. Similarly, the median tip value for males is 3.00 compared to 2.75 for females.

- The visual representation highlights the higher frequency of larger tips among males. This insight can help businesses and service providers better understand customer behavior and potentially tailor their services accordingly.

### 📆 Do weekends bring more tips?

In this question, I will repeat the steps and adjust the codes to fit the purpose. Instead of creating the dataframe by genders, I will create it by weekdays and show the histogram of the days in order to compare them to each other and to the average.
I encountered a situation where, despite completing the coding part, I discovered that the dataset is missing for Monday, Tuesday, and Wednesday. This missing data is crucial for the analysis, and I need assistance in obtaining or generating the necessary information to fill the gap.

Input:
```python
#Creating new df for day:
sun_df = df[df['day'] == 'Sun']
mon_df = df[df['day'] == 'Mon']
tue_df = df[df['day'] == 'Tue']
wed_df = df[df['day'] == 'Wed']
thu_df = df[df['day'] == 'Thur']
fri_df = df[df['day'] == 'Fri']
sat_df = df[df['day'] == 'Sat']

# Calculate measures of central tendency for each day
def calculate_central_tendency(day_df):
  return { 'min': day_df['tip'].min(),
           'max': day_df['tip'].max(),
           'mean': day_df['tip'].mean(),
           'median': day_df['tip'].median()
}

sun_central_tendency = calculate_central_tendency(sun_df)
mon_central_tendency = calculate_central_tendency(mon_df)
tue_central_tendency = calculate_central_tendency(tue_df)
wed_central_tendency = calculate_central_tendency(wed_df)
thu_central_tendency = calculate_central_tendency(thu_df)
fri_central_tendency = calculate_central_tendency(fri_df)
sat_central_tendency = calculate_central_tendency(sat_df)

#Create new data frame
day_central_tendency = { 'Common': {'min': common_tip_min,
                                    'max': common_tip_max,
                                    'mean': common_tip_mean,
                                    'median': common_tip_median},
                         'Sun': sun_central_tendency,
                         'Mon': mon_central_tendency,
                         'Tue': tue_central_tendency,
                         'Wed': wed_central_tendency,
                         'Thu': thu_central_tendency,
                         'Fri': fri_central_tendency,
                         'Sat': sat_central_tendency }

df_day_central_tendency = pd.DataFrame(day_central_tendency)

#Print data frame
print(df_day_central_tendency)


# Create chart
fig, axos = plt.subplots(1, 7, figsize=(45, 5))
# Chart 1: Sunday tip values
axos[0].hist(sun_df['tip'], bins=20, color='#74b9ff')
axos[0].set_xlabel('Tip value')
axos[0].set_ylabel('Frequency')
axos[0].set_title('Sunday tip values')
axos[0].grid(True)
# Chart 2: Monday tip values
axos[1].hist(mon_df['tip'], bins=20, color='#ff7675')
axos[1].set_xlabel('Tip value')
axos[1].set_ylabel('Frequency')
axos[1].set_title('Monday tip values')
axos[1].grid(True)
# Chart 3: Tuesday tip values
axos[2].hist(tue_df['tip'], bins=20, color='#55efc4')
axos[2].set_xlabel('Tip value')
axos[2].set_ylabel('Frequency')
axos[2].set_title('Tuesday tip values')
axos[2].grid(True)
# Chart 4: Wednesday tip values
axos[3].hist(wed_df['tip'], bins=20, color='#ffeaa7')
axos[3].set_xlabel('Tip value')
axos[3].set_ylabel('Frequency')
axos[3].set_title('Wednesday tip values')
axos[3].grid(True)
# Chart 5: Thursday tip values
axos[4].hist(thu_df['tip'], bins=20, color='#a29bfe')
axos[4].set_xlabel('Tip value')
axos[4].set_ylabel('Frequency')
axos[4].set_title('Thursday tip values')
axos[4].grid(True)
# Chart 6: Friday tip values
axos[5].hist(fri_df['tip'], bins=20, color='#fab1a0')
axos[5].set_xlabel('Tip value')
axos[5].set_ylabel('Frequency')
axos[5].set_title('Friday tip values')
axos[5].grid(True)
# Chart 7: Saturday tip values
axos[6].hist(sat_df['tip'], bins=20, color='#fd79a8')
axos[6].set_xlabel('Tip value')
axos[6].set_ylabel('Frequency')
axos[6].set_title('Saturday tip values')
axos[6].grid(True)
plt.tight_layout()
plt.show()

# Extract the mean values for each day
mean_values = df_day_central_tendency.loc['mean', ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']]
# Create a column chart
plt.figure(figsize=(10, 6))
mean_values.plot(kind='bar', color='#74b9ff')
plt.xlabel('Day of the Week')
plt.ylabel('Average Tip Value')
plt.title('Average (Mean) Tip Values by Day of the Week')
plt.grid(axis='y')
plt.show()
```
Output:
|            | Common  | Sun       | Mon       | Tue       | Wed       | Thu       | Fri       | Sat      |          
|---------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
| min     | 1.000000  | 1.010000  | NaN       | NaN       | NaN       | 1.250000  | 1.000000  | 1.000000  |
| max     | 10.000000 | 6.500000  | NaN       | NaN       | NaN       | 6.700000  | 4.730000  | 10.000000 |
| mean    | 2.998279  | 3.255132  | NaN       | NaN       | NaN       | 2.771452  | 2.734737  | 2.993103  |
| median  | 2.900000  | 3.150000  | NaN       | NaN       | NaN       | 2.305000  | 3.000000  | 2.750000  |

![image](https://github.com/user-attachments/assets/25d6ecaa-20d7-4b62-8ba1-7d170a82e47b)
![image](https://github.com/user-attachments/assets/88343ae2-5e42-4acf-b174-4634afcd5415)

