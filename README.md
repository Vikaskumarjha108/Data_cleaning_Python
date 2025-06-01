import pandas as pd
import numpy as np
from google.colab import files
uploaded = files.upload()
import pandas as pd
df = pd.read_csv('indian_employee_data(RAW).csv', encoding='ISO-8859-1')
print(df)
print("Missing values in each columns")
print(df.isnull().sum())
df['Salary (INR)'].fillna(df['Salary (INR)'].mean(), inplace=True)
df['Performance Rating'].fillna(df['Performance Rating'].median(), inplace=True)
df.replace([np.inf, -np.inf], np.nan, inplace=True)
df.fillna(df.select_dtypes(include=[np.number]).mean(), inplace=True)
df.drop_duplicates(inplace=True)
df['Salary (INR)']=np.where(df['Salary (INR)']<0,df['Salary (INR)'].mean(),df['Salary (INR)'])
salary_mean=df['Salary (INR)'].mean()
salary_std=df['Salary (INR)'].std()
lb=salary_mean-3*salary_std
ub=salary_mean+3*salary_std
df['Salary (INR)']=np.where(df['Salary (INR)']<lb,lb,df['Salary (INR)'])
df['Salary (INR)']=np.where(df['Salary (INR)']>ub,ub,df['Salary (INR)'])
df.to_csv('cleaned_data.csv', index=False)
df.dropna(subset=['Name'], inplace=True)
df['Department'] = df['Department'].fillna('New Joining')
df = df.astype({'Age': 'int','Experience (Years)': 'int'})
print(df)
from google.colab import files
files.download("cleaned_data.csv")
