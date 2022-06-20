# Data Factory

Generate your own data.

# Install

```bash
$ pip install git+https://github.com/yoyoyo-yo/data_factory.git
```

# Requirments

```bash
numpy, pandas
```

# Easy Usage

## Generate random data

```python
from data_factory import data_factory
dfac = data_factory.DataFactory()
dfac.random_generate(auto=True, col_num=10)
df = dfac.get_data()   # return pandas.DataFrame
```

df is like below

```bash
          col_0  col_1  col_2      col_3       col_4  col_5     col_6  \
0   category_16     79    -60  79.463262  category_2    1.0  8.000000   
1   category_25    144   -150  80.684038  category_2    1.0  7.238616   
2   category_28    209   -240  84.256503  category_2    1.0  5.099392   
3   category_12    274   -330  83.462233  category_2    1.0  1.989519   
4   category_13    339   -420  80.984804  category_2    1.0 -1.499051   
..          ...    ...    ...        ...         ...    ...       ...   
95   category_4   6254  -8610  83.392761  category_2    1.0 -4.702282   
96  category_30   6319  -8700  82.064492  category_2    1.0 -1.499051   
97  category_21   6384  -8790  81.479830  category_1    1.0  1.989519   
98  category_14   6449  -8880  82.777377  category_2    1.0  5.099392   
99   category_4   6514  -8970  76.492746  category_2    1.0  7.238616   

       col_7     col_8  col_9  
0   6.000000  0.000000    1.0  
1   5.893724  0.368125    1.0  
2   5.578659  0.684547    1.0  
3   5.065968  0.904827    1.0  
4   4.373812  0.998027    1.0  
..       ...       ...    ...  
95  3.526712 -0.951057    1.0  
96  4.373812 -0.998027    1.0  
97  5.065968 -0.904827    1.0  
98  5.578659 -0.684547    1.0  
99  5.893724 -0.368125    1.0  

[100 rows x 10 columns]
```

## Generate from your pre-setting

You can set column config list.

Each list contains [***column name***, ***data type***, ***config***].


```python
from data_factory import data_factory
dfac = data_factory.DataFactory(
    [
        ['date', 'date', dict(start='2022-04-01', end='2022-04-14', freq='D')],
        ['id', 'raw', dict(val=np.arange(14))],
        ['id2', 'num', dict(type='order')],
        ['temperature', 'num', dict(type='gauss', mean=25, std=5)],
        ['sin', 'num', dict(type='sin', freq=5, scale=2)],
        ['const_val', 'num', dict(type='uniform', val=2)],
        ['Friend', 'cat', dict(val=['Cat', 'Dog', 'Salamander'])],
        ['Fruits', 'cat', dict(val={'Orange':3, 'Apple':1, 'Banana':2})],
        ['sin_corr', 'num', dict(type='corr', anchor='sin', corr=0.4)],
        ['sin_noised', 'expr', dict(expr=[dict(type='gauss'), '*', dict(type='sin', freq=3, scale=10, shift=10)])],
        ['exp_sin', 'expr', dict(expr=[dict(type='exp-', scale=2), '*', dict(type='sin', freq=1), 'f', 'log1p'])],
        ['Strength', 'expr', dict(expr=[dict(type='uniform', val=2), '+', dict(type='noise', noise_type='gauss', prob=0.2, scale=10)])],
    ],
)

df = dfac.get_data()
```


```bash
         date  id  id2  temperature           sin  const_val      Friend  Fruits  sin_corr  sin_noised       exp_sin   Strength
0  2022-04-01   0    0    33.820262  0.000000e+00        1.0  Salamander  Orange -2.380665         0.0  0.000000e+00   1.000000
1  2022-04-02   1    1    27.000786  1.563663e+00        1.0         Cat  Orange -1.234665         0.0  4.641742e-01   1.000000
2  2022-04-03   2    2    29.893690 -1.949856e+00        1.0         Dog  Orange -3.461842         0.0  5.449682e-01   1.000000
3  2022-04-04   3    3    36.204466  8.677675e-01        1.0  Salamander   Apple -6.076986         0.0  4.793511e-01   1.000000
4  2022-04-05   4    4    34.337790  8.677675e-01        1.0         Cat  Orange -1.226625         0.0  3.497098e-01   1.000000
5  2022-04-06   5    5    20.113611 -1.949856e+00        1.0  Salamander  Banana -5.729155         0.0  2.058261e-01  -5.759378
6  2022-04-07   6    6    29.750442  1.563663e+00        1.0         Cat  Orange  2.371843         0.0  8.280936e-02   1.848609
7  2022-04-08   7    7    24.243214  1.224647e-15        1.0         Dog  Banana  5.739868         0.0  1.658771e-17   1.000000
8  2022-04-09   8    8    24.483906 -1.563663e+00        1.0  Salamander   Apple -3.209569         0.0 -4.082705e-02 -15.359159
9  2022-04-10   9    9    27.052993  1.949856e+00        1.0  Salamander  Banana  1.859075         0.0 -5.031483e-02   1.000000
10 2022-04-11  10   10    25.720218 -8.677675e-01        1.0         Cat  Orange -0.320109         0.0 -4.254464e-02   1.000000
11 2022-04-12  11   11    32.271368 -8.677675e-01        1.0         Dog  Banana -0.400157         0.0 -2.876296e-02   1.000000
12 2022-04-13  12   12    28.805189  1.949856e+00        1.0         Dog  Orange  0.425005         0.0 -1.559875e-02   1.000000
13 2022-04-14  13   13    25.608375 -1.563663e+00        1.0  Salamander  Banana -0.392099         0.0 -5.864132e-03  23.181519
```


# Parameters

## raw

```python
['id', 'raw', dict(val=np.arange(30))],
```

- val : array ... raw data

## date

```python
['date', 'date', dict(start='2022-04-01', end='2022-04-30', freq='D')],
```

- start : str ... Start date.
- end : str ... End date.
- freq : str ... Frequency.

## numerical

```python
['temperature', 'num', dict(type='gauss', mean=25, std=5)],
```

- type : str ... Data name.
- scale : float (default=1) ... Scaling value.
- shift : float (defualt=0) ... Shifting value.
- abs : bool (default=False) ... Return absolute value.

### gauss

Gaussian noise.

```python
['gauss_test', 'num', dict(type='gauss', mean=25, std=5)],
```

- mean : float ... Mean value of gaussian distribution.
- std : float ... Standard deviation of gaussian distribution.

### order

Order value.

```python
['order_test', 'num', dict(type='order', freq=1, start=1)],
```

- freq : int ... Frequency.
- start : int ... Start value.

### sin, cos

f = sin(2 * pi * frequency * ind / data_length)

```python
['sin_test', 'num', dict(type='sin', freq=3)],
```

- freq : float ... Frequency.

### uniform

```python
['uniform_test', 'num', dict(type='uniform', val=2)],
```

- value : float ... Constant value.

### exp, exp-

f = exp(x), f = exp(-x)

```python
['exp_test', 'num', dict(type='exp')],
```

### noise

```python
['noise_test', 'expr', dict(expr=[dict(type='uniform', val=2), '+', dict(type='noise', noise_type='gauss', prob=0.2, scale=10)])],
```

- noise_type : str (default='gauss') ... Noise type. ['gauss', 'const']
- prob : float (defualt=0.5) ... Noise frequency.

### corr

```python
['corr_test', 'num', dict(type='corr', anchor='exp_test', corr=0.8)],
```

- anchor : str ... Anchor column name.
- corr : float (default=0.5) ... Target correlation value.
- epsilon : float (default=1e-4) ... Difference between target correlation and generated correlation.
- iter_max : int (default=10_000) ... Max iteration number.

## cat

```python
['Friend', 'cat', dict(val=['Cat', 'Dog', 'Salamander'])],
['Fruits', 'cat', dict(val={'Orange':3, 'Apple':1, 'Banana':2})],
```

- val : list, dict ... Category list. If list, each category appears at the same frequency. If dict, key is cateogyr and value is frequency weight.

## expr

```python
['sin_noised', 'expr', dict(expr=[dict(type='gauss'), '*', dict(type='sin', freq=3, scale=10, shift=10)])]
```

- expr : list ... Mathematical expression. List number is surely odd. You can use '+', '-', '*' and '/' .

# Function Usage

## DataFactory(**args)

### Args

- data_defs : list (default=[]) ... Data config list.
- data_num : int (default=None) ... Data row number.
- random_state : int (default=0) ... Random seed.
- verbose : int (default=0) ... Print verbose information.
- drop_rate : float (default=None) ... Drop rate along row axis.


## random_generate(**args)

Generate random data using random determined seed.

### Usage

```python
dfac = data_factory.DataFactory()
dfac.random_generate(auto=True, col_num=10)
df = dfac.get_data()
```

### Args

- auto : bool (default=False) ... Whether generate random data automatically.
- data_num : int (default=100) ... Data row number.
- col_num : int (default=10) ... Data column number.
- key_date : bool (default=False) ... Whether key column is data or not.
- contain_nan : float (default=-1) ... Frequency rate for each column contain nan.
- verbose : int (default=0) ... Print verbose information.




## auto_generate(**args)

Generate random data using fixed random_state.

### Usage

```python
dfac = data_factory.DataFactory()
dfac.auto_generate(col_num=10)
df = dfac.get_data()
```

### Args

- data_num : int (default=100) ... Data row number.
- col_num : int (default=10) ... Data column number.
- random_state : int (default=0) ... Random seed.
- key_date : bool (default=False) ... Whether key column is data or not.
- contain_nan : float (default=-1) ... Frequency rate for each column contain nan.
- verbose : int (default=0) ... Print verbose information.

## get_data(**args)

### Args

- return_dtype : str (default='pd') ... Data type.


# License

MIT License