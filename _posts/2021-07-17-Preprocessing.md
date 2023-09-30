---
layout: single
title:  "데이터 전처리"
categories: GAN
toc: true
---


# 데이터 전처리

> From github(https://github.com/PacktPublishing/Generative-Adversarial-Networks-Cookbook)
> 데이터 전치리란 일반적으로 데이터를 읽고 도메인에서 데이터를 사용할 수 있도록 기본적인 작업을 수행하는 단계를 의미한다.



#### Data Read 

------

###### - 사용 데이터 :  UCI Machine Learning income data 

```python
# 방식1) 데이터 그대로 Read
df0 = pd.read_csv('./data/adult.data')

# 방식2) 컬럼(즉, 데이터 제목) 제외하고 Read
df1 = pd.read_csv('./data/adult.data', header = None)

# 방식3) 컬럼(즉, 데이터 제목) 제외하고 지정된 이름을 붙여 Read
df2 = pd.read_csv('./data/adult.data', names = ['age', 'workclass', 'fnlwgt', 'education', 'education-num', 'marital-status', 'occupation', 'relationship', 'race', 'sex', 'capital-gain', 'capital-loss', 'hours-per-week', 'native-country','Label']) 

```



#### 데이터 처리

------

```python
    # Object 타입 -> 카테고리 형으로 변환함
    if(df2[col_name].dtype == 'object'):
        # 카테고리형 타입이므로, 
        df2[col_name]= df2[col_name].astype('category')
```

```python
# df2[col_name]에는 numerical값의 모음으로 들어있다.
# mapping_index는 category index이다. 문자열 값까지도 포함한다.

df2[col_name], mapping_index = pd.Series(df2[col_name]).factorize()
```

> < mapping index 예시 >
>
> CategoricalIndex([' State-gov', ' Self-emp-not-inc', ' Private',
>                   ' Federal-gov', ' Local-gov', ' ?', ' Self-emp-inc',
>                   ' Without-pay', ' Never-worked'],
>                  categories=[' ?', ' Federal-gov', ' Local-gov', ' Never-worked', ' Private', ' Self-emp-inc', ' Self-emp-not-inc', ' State-gov', ...], ordered=False, dtype='category')

