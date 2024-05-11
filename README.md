# CSC245-lab5

import pandas as pd
# 1. Write a Pandas program to print a concise summary of the dataset (titanic.csv).
# 2. Write a Pandas program to extract the column labels, shape and data types of the dataset (titanic.csv).
# 3. Write a Pandas program to create a Pivot table with multiple indexes from the data set of titanic.csv.
# 4. Write a Pandas program to create a Pivot table and find survival rate by gender on various classes.
# 5. Write a Pandas program to create a Pivot table and find survival rate by gender.
# 6. Write a Pandas program to create a Pivot table and find survival rate by gender, age wise of various classes.
# 7. Write a Pandas program to partition each of the passengers into four categories based on their age.
# Note: Age categories (0, 10), (10, 30), (30, 60), (60, 80)
# 8. Write a Pandas program to create a Pivot table and count survival by gender, categories wise age of various classes.
# Note: Age categories (0, 10), (10, 30), (30, 60), (60, 80)
# 9. Write a Pandas program to create a Pivot table and find survival rate by gender, age of the different categories of various classes.
# 10. Write a Pandas program to create a Pivot table and find survival rate by gender, age of the different categories of various classes. Add the fare as a dimension of columns and partition fare column into 2 categories based on the values present in fare columns
print('1:')
data=pd.read_csv('titanic.csv')
summary=data.info()
print(summary,'\n\n')


print('2:')
data=pd.read_csv('titanic.csv')
column_label=data.columns.tolist()
shape=data.shape
data_types=data.dtypes
print('Column Labels:',column_label,'\n')
print('Shape:',shape,'\n')
print('Data types:')
print(data_types,'\n\n')

print('3:')
pivot_table = data.pivot_table('survived', index=['sex', 'pclass'])
print(pivot_table)

print('4:')
data = pd.read_csv('titanic.csv')
pivot_table = data.pivot_table('survived', index='sex', columns='pclass')
print(pivot_table, '\n\n')

print('5:')
data=pd.read_csv('titanic.csv')
pivot_table=data.pivot_table('survived',index='sex')
print(pivot_table, '\n\n')

print('6:')
data=pd.read_csv('titanic.csv')
pivot_table = data.pivot_table('survived', index=['sex', pd.cut(data['age'], bins=[0, 18, 25, 40, 60, 100], right=False)], columns='pclass', observed=False)
print(pivot_table, '\n\n')

print('7:')
data = pd.read_csv('titanic.csv')
age= pd.cut(data['age'], bins=[0, 10, 30, 60, 80], labels=['0-10', '10-30', '30-60', '60-80'])
data['age_category'] = age
print(data[['age', 'age_category']],'\n\n')

print('8:')
data = pd.read_csv('titanic.csv')
age_bins = [0, 10, 30, 60, 80]
age_labels = ['0-10', '10-30', '30-60', '60-80']
data['age_category'] = pd.cut(data['age'], bins=age_bins, labels=age_labels)
pivot_table = data.pivot_table(index=['sex', 'age_category'], columns='pclass', values='survived', aggfunc='count', observed=False)
print(pivot_table,'\n\n')

print('9:')
data = pd.read_csv('titanic.csv')
age_bins = [0, 10, 30, 60, 80]
age_labels = ['0-10', '10-30', '30-60', '60-80']
data['age_category'] = pd.cut(data['age'], bins=age_bins, labels=age_labels)
pivot_table = data.pivot_table(index=['sex', 'age_category'], columns='pclass', values='survived', aggfunc='mean', observed=False)
print(pivot_table,'\n\n')

print('10:')
data = pd.read_csv('titanic.csv')
age_bins = [0, 10, 30, 60, 80]
age_labels = ['0-10', '10-30', '30-60', '60-80']
data['age_category'] = pd.cut(data['age'], bins=age_bins, labels=age_labels)
fare_bins = [data['fare'].min(), data['fare'].mean(), data['fare'].max()]
fare_labels = ['Low', 'High']
data['fare_category'] = pd.cut(data['fare'], bins=fare_bins, labels=fare_labels)
pivot_table = data.pivot_table(index=['sex', 'age_category'], columns=['pclass', 'fare_category'], values='survived', aggfunc='mean', observed=False)
print(pivot_table)
