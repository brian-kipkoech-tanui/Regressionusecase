# Regressionusecase
# End to End ML Project Use Case

## Import the relevant libraries


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import LabelEncoder
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
```

## Load the dataset


```python
dataset = pd.read_csv("./ames_unprocessed_data.csv")
# Check the basic info
dataset.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1460 entries, 0 to 1459
    Data columns (total 21 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   MSSubClass    1460 non-null   int64  
     1   MSZoning      1460 non-null   object 
     2   LotFrontage   1201 non-null   float64
     3   LotArea       1460 non-null   int64  
     4   Neighborhood  1460 non-null   object 
     5   BldgType      1460 non-null   object 
     6   HouseStyle    1460 non-null   object 
     7   OverallQual   1460 non-null   int64  
     8   OverallCond   1460 non-null   int64  
     9   YearBuilt     1460 non-null   int64  
     10  Remodeled     1460 non-null   int64  
     11  GrLivArea     1460 non-null   int64  
     12  BsmtFullBath  1460 non-null   int64  
     13  BsmtHalfBath  1460 non-null   int64  
     14  FullBath      1460 non-null   int64  
     15  HalfBath      1460 non-null   int64  
     16  BedroomAbvGr  1460 non-null   int64  
     17  Fireplaces    1460 non-null   int64  
     18  GarageArea    1460 non-null   int64  
     19  PavedDrive    1460 non-null   object 
     20  SalePrice     1460 non-null   int64  
    dtypes: float64(1), int64(15), object(5)
    memory usage: 239.7+ KB
    

## Handle missing values


```python
# Check the column with missing values
data = dataset.copy()
dataset.isna().sum()
```




    MSSubClass        0
    MSZoning          0
    LotFrontage     259
    LotArea           0
    Neighborhood      0
    BldgType          0
    HouseStyle        0
    OverallQual       0
    OverallCond       0
    YearBuilt         0
    Remodeled         0
    GrLivArea         0
    BsmtFullBath      0
    BsmtHalfBath      0
    FullBath          0
    HalfBath          0
    BedroomAbvGr      0
    Fireplaces        0
    GarageArea        0
    PavedDrive        0
    SalePrice         0
    dtype: int64




```python
# Use the sklearn's SimpleImputer() class to handle all missing values
imputer = SimpleImputer(missing_values=np.nan, strategy='mean')
# fit the imputer on numerical columns
int_cols = dataset.select_dtypes(include=['float', 'int']).columns.to_list()
imputer.fit(dataset[int_cols])
dataset[int_cols] = imputer.transform(dataset[int_cols])
dataset.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1460 entries, 0 to 1459
    Data columns (total 21 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   MSSubClass    1460 non-null   float64
     1   MSZoning      1460 non-null   object 
     2   LotFrontage   1460 non-null   float64
     3   LotArea       1460 non-null   float64
     4   Neighborhood  1460 non-null   object 
     5   BldgType      1460 non-null   object 
     6   HouseStyle    1460 non-null   object 
     7   OverallQual   1460 non-null   float64
     8   OverallCond   1460 non-null   float64
     9   YearBuilt     1460 non-null   float64
     10  Remodeled     1460 non-null   float64
     11  GrLivArea     1460 non-null   float64
     12  BsmtFullBath  1460 non-null   float64
     13  BsmtHalfBath  1460 non-null   float64
     14  FullBath      1460 non-null   float64
     15  HalfBath      1460 non-null   float64
     16  BedroomAbvGr  1460 non-null   float64
     17  Fireplaces    1460 non-null   float64
     18  GarageArea    1460 non-null   float64
     19  PavedDrive    1460 non-null   object 
     20  SalePrice     1460 non-null   float64
    dtypes: float64(16), object(5)
    memory usage: 239.7+ KB
    

## Checking the distribution of continuous variable


```python
import seaborn as sns
sns.set()
dataset[int_cols].hist(figsize=(20,8))
plt.show()
```


    
![png](output_9_0.png)
    


## Feature Scaling


```python
dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Neighborhood</th>
      <th>BldgType</th>
      <th>HouseStyle</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>...</th>
      <th>GrLivArea</th>
      <th>BsmtFullBath</th>
      <th>BsmtHalfBath</th>
      <th>FullBath</th>
      <th>HalfBath</th>
      <th>BedroomAbvGr</th>
      <th>Fireplaces</th>
      <th>GarageArea</th>
      <th>PavedDrive</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>60.0</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450.0</td>
      <td>CollgCr</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>2003.0</td>
      <td>...</td>
      <td>1710.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>548.0</td>
      <td>Y</td>
      <td>208500.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20.0</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600.0</td>
      <td>Veenker</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>6.0</td>
      <td>8.0</td>
      <td>1976.0</td>
      <td>...</td>
      <td>1262.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>460.0</td>
      <td>Y</td>
      <td>181500.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>60.0</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250.0</td>
      <td>CollgCr</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>2001.0</td>
      <td>...</td>
      <td>1786.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>608.0</td>
      <td>Y</td>
      <td>223500.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>70.0</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550.0</td>
      <td>Crawfor</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>1915.0</td>
      <td>...</td>
      <td>1717.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>642.0</td>
      <td>Y</td>
      <td>140000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>60.0</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260.0</td>
      <td>NoRidge</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>8.0</td>
      <td>5.0</td>
      <td>2000.0</td>
      <td>...</td>
      <td>2198.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>836.0</td>
      <td>Y</td>
      <td>250000.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1455</th>
      <td>60.0</td>
      <td>RL</td>
      <td>62.0</td>
      <td>7917.0</td>
      <td>Gilbert</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>6.0</td>
      <td>5.0</td>
      <td>1999.0</td>
      <td>...</td>
      <td>1647.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>460.0</td>
      <td>Y</td>
      <td>175000.0</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>20.0</td>
      <td>RL</td>
      <td>85.0</td>
      <td>13175.0</td>
      <td>NWAmes</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>1978.0</td>
      <td>...</td>
      <td>2073.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>500.0</td>
      <td>Y</td>
      <td>210000.0</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>70.0</td>
      <td>RL</td>
      <td>66.0</td>
      <td>9042.0</td>
      <td>Crawfor</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>1941.0</td>
      <td>...</td>
      <td>2340.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>252.0</td>
      <td>Y</td>
      <td>266500.0</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>20.0</td>
      <td>RL</td>
      <td>68.0</td>
      <td>9717.0</td>
      <td>NAmes</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>1950.0</td>
      <td>...</td>
      <td>1078.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>240.0</td>
      <td>Y</td>
      <td>142125.0</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>20.0</td>
      <td>RL</td>
      <td>75.0</td>
      <td>9937.0</td>
      <td>Edwards</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>1965.0</td>
      <td>...</td>
      <td>1256.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>276.0</td>
      <td>Y</td>
      <td>147500.0</td>
    </tr>
  </tbody>
</table>
<p>1460 rows × 21 columns</p>
</div>




```python
# Create the features and label
X = dataset.iloc[:,:-1]
y = dataset.iloc[:,-1].values
y = y.reshape(len(y), 1)
```


```python
sc_x = StandardScaler()
sc_y = StandardScaler()
# Standardise the numerical columns
X[int_cols[:-1]] = sc_x.fit_transform(X[int_cols[:-1]])
y = sc_y.fit_transform(y)
X
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Neighborhood</th>
      <th>BldgType</th>
      <th>HouseStyle</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>Remodeled</th>
      <th>GrLivArea</th>
      <th>BsmtFullBath</th>
      <th>BsmtHalfBath</th>
      <th>FullBath</th>
      <th>HalfBath</th>
      <th>BedroomAbvGr</th>
      <th>Fireplaces</th>
      <th>GarageArea</th>
      <th>PavedDrive</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.073375</td>
      <td>RL</td>
      <td>-0.229372</td>
      <td>-0.207142</td>
      <td>CollgCr</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>0.651479</td>
      <td>-0.517200</td>
      <td>1.050994</td>
      <td>-0.954460</td>
      <td>0.370333</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>0.789741</td>
      <td>1.227585</td>
      <td>0.163779</td>
      <td>-0.951226</td>
      <td>0.351000</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.872563</td>
      <td>RL</td>
      <td>0.451936</td>
      <td>-0.091886</td>
      <td>Veenker</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>-0.071836</td>
      <td>2.179628</td>
      <td>0.156734</td>
      <td>-0.954460</td>
      <td>-0.482512</td>
      <td>-0.819964</td>
      <td>3.948809</td>
      <td>0.789741</td>
      <td>-0.761621</td>
      <td>0.163779</td>
      <td>0.600495</td>
      <td>-0.060731</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.073375</td>
      <td>RL</td>
      <td>-0.093110</td>
      <td>0.073480</td>
      <td>CollgCr</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>0.651479</td>
      <td>-0.517200</td>
      <td>0.984752</td>
      <td>1.047712</td>
      <td>0.515013</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>0.789741</td>
      <td>1.227585</td>
      <td>0.163779</td>
      <td>0.600495</td>
      <td>0.631726</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.309859</td>
      <td>RL</td>
      <td>-0.456474</td>
      <td>-0.096897</td>
      <td>Crawfor</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>0.651479</td>
      <td>-0.517200</td>
      <td>-1.863632</td>
      <td>1.047712</td>
      <td>0.383659</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>-1.026041</td>
      <td>-0.761621</td>
      <td>0.163779</td>
      <td>0.600495</td>
      <td>0.790804</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.073375</td>
      <td>RL</td>
      <td>0.633618</td>
      <td>0.375148</td>
      <td>NoRidge</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>1.374795</td>
      <td>-0.517200</td>
      <td>0.951632</td>
      <td>-0.954460</td>
      <td>1.299326</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>0.789741</td>
      <td>1.227585</td>
      <td>1.390023</td>
      <td>0.600495</td>
      <td>1.698485</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1455</th>
      <td>0.073375</td>
      <td>RL</td>
      <td>-0.365633</td>
      <td>-0.260560</td>
      <td>Gilbert</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>-0.071836</td>
      <td>-0.517200</td>
      <td>0.918511</td>
      <td>1.047712</td>
      <td>0.250402</td>
      <td>-0.819964</td>
      <td>-0.241061</td>
      <td>0.789741</td>
      <td>1.227585</td>
      <td>0.163779</td>
      <td>0.600495</td>
      <td>-0.060731</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>-0.872563</td>
      <td>RL</td>
      <td>0.679039</td>
      <td>0.266407</td>
      <td>NWAmes</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>-0.071836</td>
      <td>0.381743</td>
      <td>0.222975</td>
      <td>1.047712</td>
      <td>1.061367</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>0.789741</td>
      <td>-0.761621</td>
      <td>0.163779</td>
      <td>2.152216</td>
      <td>0.126420</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>0.309859</td>
      <td>RL</td>
      <td>-0.183951</td>
      <td>-0.147810</td>
      <td>Crawfor</td>
      <td>1Fam</td>
      <td>2Story</td>
      <td>0.651479</td>
      <td>3.078570</td>
      <td>-1.002492</td>
      <td>1.047712</td>
      <td>1.569647</td>
      <td>-0.819964</td>
      <td>-0.241061</td>
      <td>0.789741</td>
      <td>-0.761621</td>
      <td>1.390023</td>
      <td>2.152216</td>
      <td>-1.033914</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>-0.872563</td>
      <td>RL</td>
      <td>-0.093110</td>
      <td>-0.080160</td>
      <td>NAmes</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>-0.795151</td>
      <td>0.381743</td>
      <td>-0.704406</td>
      <td>1.047712</td>
      <td>-0.832788</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>-1.026041</td>
      <td>-0.761621</td>
      <td>-1.062465</td>
      <td>-0.951226</td>
      <td>-1.090059</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>-0.872563</td>
      <td>RL</td>
      <td>0.224833</td>
      <td>-0.058112</td>
      <td>Edwards</td>
      <td>1Fam</td>
      <td>1Story</td>
      <td>-0.795151</td>
      <td>0.381743</td>
      <td>-0.207594</td>
      <td>-0.954460</td>
      <td>-0.493934</td>
      <td>1.107810</td>
      <td>-0.241061</td>
      <td>-1.026041</td>
      <td>1.227585</td>
      <td>0.163779</td>
      <td>-0.951226</td>
      <td>-0.921624</td>
      <td>Y</td>
    </tr>
  </tbody>
</table>
<p>1460 rows × 20 columns</p>
</div>



## Encoding Categorical Variables


```python
# Check the number of categories for each categorical column
print("Unique Categories for each categorical columns\n")
for col in dataset.select_dtypes(include=['object']).columns.to_list():
    print(col, '\n--------------')
    print(dataset[col].unique(), '\n')
```

    Unique Categories for each categorical columns
    
    MSZoning 
    --------------
    ['RL' 'RM' 'C (all)' 'FV' 'RH'] 
    
    Neighborhood 
    --------------
    ['CollgCr' 'Veenker' 'Crawfor' 'NoRidge' 'Mitchel' 'Somerst' 'NWAmes'
     'OldTown' 'BrkSide' 'Sawyer' 'NridgHt' 'NAmes' 'SawyerW' 'IDOTRR'
     'MeadowV' 'Edwards' 'Timber' 'Gilbert' 'StoneBr' 'ClearCr' 'NPkVill'
     'Blmngtn' 'BrDale' 'SWISU' 'Blueste'] 
    
    BldgType 
    --------------
    ['1Fam' '2fmCon' 'Duplex' 'TwnhsE' 'Twnhs'] 
    
    HouseStyle 
    --------------
    ['2Story' '1Story' '1.5Fin' '1.5Unf' 'SFoyer' 'SLvl' '2.5Unf' '2.5Fin'] 
    
    PavedDrive 
    --------------
    ['Y' 'N' 'P'] 
    
    


```python
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), [1,4,5,6,19])],remainder='passthrough')
X = ct.fit_transform(X)
X
```




    array([[ 0.        ,  0.        ,  0.        , ...,  0.16377912,
            -0.95122649,  0.35100032],
           [ 0.        ,  0.        ,  0.        , ...,  0.16377912,
             0.60049493, -0.06073101],
           [ 0.        ,  0.        ,  0.        , ...,  0.16377912,
             0.60049493,  0.63172623],
           ...,
           [ 0.        ,  0.        ,  0.        , ...,  1.39002276,
             2.15221636, -1.03391416],
           [ 0.        ,  0.        ,  0.        , ..., -1.06246453,
            -0.95122649, -1.09005935],
           [ 0.        ,  0.        ,  0.        , ...,  0.16377912,
            -0.95122649, -0.9216238 ]])



## Splitting the dataset into training and testing datasets


```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

## Use sklearn Pipeline to preprocess and fit a model


```python
from sklearn.preprocessing import FunctionTransformer
from copy import deepcopy

X = data.iloc[:,:-1]
y = data.iloc[:,-1].values
# Fix missing values
imputer = SimpleImputer(missing_values=np.nan, strategy='median')
imputer.fit(X.select_dtypes(include=['int', 'float']))
int_cols = X.select_dtypes(include=['float', 'int']).columns.to_list()
X[int_cols] = imputer.transform(X[int_cols])
# OneHot encode the categorical columns
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), [1,4,5,6,19])],remainder='passthrough')
X = ct.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
# Standardise the whole dataset
std_scaler = StandardScaler().fit(X_train)

def preprocessor(X):
    D = np.copy(X)
    D = std_scaler.transform(D)
    return D
```


```python
len(X_test[0])
```




    61




```python
preprocessor_transformer = FunctionTransformer(preprocessor)
preprocessor_transformer
```




    FunctionTransformer(func=<function preprocessor at 0x000001AC94235048>)




```python
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LinearRegression

p1 = Pipeline([('scaler', preprocessor_transformer),
              ('Linear Regression', LinearRegression())])
p1
```




    Pipeline(steps=[('scaler',
                     FunctionTransformer(func=<function preprocessor at 0x000001AC94235048>)),
                    ('Linear Regression', LinearRegression())])




```python
from sklearn.metrics import mean_absolute_error

def fit_and_print(p, X_train=X_train, X_test=X_test, y_train=y_train, y_test=y_test):
    # Fit the transformer
    p.fit(X_train, y_train)
    # Predict the train and test outputs
    training_prediction = p.predict(X_train)
    test_prediction =p.predict(X_test)
    
    # Print the errors
    print("Training Error:     "+str(mean_absolute_error(training_prediction, y_train)))
    print("Test Error:         "+str(mean_absolute_error(test_prediction, y_test)))
```


```python
fit_and_print(p1)
```

    Training Error:     19612.705234115496
    Test Error:         20056.787013477147
    

## Fit a KNN regressor


```python
# Perform Grid search
from sklearn.model_selection import GridSearchCV
from sklearn.neighbors import KNeighborsRegressor as KNR

# Create a KNN Regressor
knn = KNR()

# Define Hyperparameters to tune
param_grid = {
    'n_neighbors': [3,5,7,9,11]
}

# Perform Grid Search
grid_search = GridSearchCV(estimator=knn, param_grid=param_grid,
                          scoring='neg_mean_squared_error', cv=5)
# Fit Grid Search to Data
grid_search.fit(X_train, y_train)
# Retrieve Best Parameters
best_params = grid_search.best_params_
best_estimator = grid_search.best_estimator_
```


```python
print("best_params: ", best_params, "\nbest_estimator: ", best_estimator)
```

    best_params:  {'n_neighbors': 7} 
    best_estimator:  KNeighborsRegressor(n_neighbors=7)
    


```python
p2 = Pipeline([('Scaler', preprocessor_transformer),
               ('KNN Regression', KNR(n_neighbors=9))])
fit_and_print(p2)
```

    Training Error:     23518.734874429225
    Test Error:         26095.699010654487
    

## Fit a RandomForestRegressor()


```python
from sklearn.ensemble import RandomForestRegressor
# Create a random forest regressor
rf_regressor = RandomForestRegressor()

# Define Hyperparameters to tune
param_grid = {
    'n_estimators': [50, 100, 150, 200],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
#     'min_samples_leaf': [1,2,4,6,8,10],
#     'max_features': ['auto', 'sqrt'],
#     'bootstrap': [True, False]
}

# Perform Grid Search
grid_search = GridSearchCV(estimator=rf_regressor, param_grid=param_grid, 
                           scoring='neg_mean_squared_error', cv=5)
#Fit grid search data
grid_search.fit(X_train, y_train)
# Retrive best parameters
best_params = grid_search.best_params_
best_estimator = grid_search.best_estimator_
print("best_params: ", best_params, "\nbest_estimator: ", best_estimator)
```

    best_params:  {'max_depth': 20, 'min_samples_split': 5, 'n_estimators': 150} 
    best_estimator:  RandomForestRegressor(max_depth=20, min_samples_split=5, n_estimators=150)
    


```python
p3 = Pipeline([('Scaler', preprocessor_transformer),
               ('RFR Regression', RandomForestRegressor(max_depth=20, min_samples_split=5, n_estimators=150))])
fit_and_print(p3)
```

    Training Error:     8106.180156534265
    Test Error:         18495.41643449427
    


```python

```

