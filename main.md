---
title: "수원시 반려동물의 분포에 대한 분석"
excerpt: "제가 처음으로 시도했던 데이터 분석 프로젝트입니다. pandas와 matplotlib를 활용하여 반려동물 데이터를 적절하게 가공하고 시각화하는 과정을 담았습니다."

url: /category/
categories: [visualization]
# tags: [python]

# classes: wide
author_profile: true
toc: true
toc_sticky: true
---

## 1) 프로젝트 목표 및 내용

이 프로젝트의 주제는 수원시 동별 반려동물의 분포를 분석하는 것입니다. 우선 프로젝트를 통해서 파이썬 데이터 분석 도구중 하나인 pandas의 사용 방법을 배우고, 다량의 데이터를 다루는 것에 조금이나마 익숙해지는 것을 목표로 하고 있습니다. 또한 주제와 관련해서는 수원시 반려동물의 등록 현황을 통해서 수원시 반려동물의 분포를 정리하고, 기존 자료에 비해 더 다양한 방법들을 통해 데이터를 표현함으로써 반려동물 분포를 더 가독성 있게 표현하는 것이 목표입니다.

## 2) 주제 선정 이유

저는 수원시 영통구 매탄동에 거주하고 있으며, 강아지를 기르고 있어서 반려동물에 관심이 많습니다. 어떤 종류의 데이터를 분석할지에 대해 고민하던 중, 제가 살고 있는 수원시에 반려동물이 대략 몇 마리가 있으며, 동별로 몇마리가 있는지에 대한 데이터를 접하게 되었습니다. 수원시에 동별로 몇마리의 반려동물이 있는지를 알아보고, 그것과 관련된 여러 다른 데이터들을 분석함으로써 반려동물의 분포를 보기 쉽게 나타내보고 싶었기 때문에 이 주제를 선택하게 되었습니다.

## 3) 데이터 출처

공공 데이터 포털(http://www.data.go.kr/index.do) 에서 기획조정실 정보통신과 부서가 관리하는 「경기도_수원시_반려동물현황」자료를 사용하였습니다.

경기도_수원시_반려동물현황
http://www.data.go.kr/data/15040321/fileData.do

## 4) 구현 내용 설명

pandas import 하기


```python
import pandas as pd
```

엑셀파일 열기


```python
animals = pd.read_excel('경기도_수원시_반려동물등록현황_20191010.xlsx',
                                             usecols = 'C, D, E, F, G')
animals.head(3) 
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
      <th>구청명</th>
      <th>법정동명</th>
      <th>등록품종수</th>
      <th>등록개체수</th>
      <th>소유자수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>권선구</td>
      <td>탑동</td>
      <td>57</td>
      <td>1169</td>
      <td>872</td>
    </tr>
    <tr>
      <th>1</th>
      <td>권선구</td>
      <td>평동</td>
      <td>29</td>
      <td>117</td>
      <td>91</td>
    </tr>
    <tr>
      <th>2</th>
      <td>권선구</td>
      <td>고색동</td>
      <td>46</td>
      <td>818</td>
      <td>615</td>
    </tr>
  </tbody>
</table>
</div>



한사람당동물수 = 등록개체수/소유자수 계산해서 추가하기


```python
animals['한사람당동물수']=animals['등록개체수']/animals['소유자수']
animals = animals.loc[:, ['법정동명', '등록개체수', '한사람당동물수', '등록품종수']]
```

반려동물 개체수 많은 순으로 정렬하기


```python
Chart_1 = animals.sort_values( by = '등록개체수', ascending = False, ignore_index = True)
Chart_1.head(5)
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
      <th>법정동명</th>
      <th>등록개체수</th>
      <th>한사람당동물수</th>
      <th>등록품종수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>매탄동</td>
      <td>4321</td>
      <td>1.258299</td>
      <td>78</td>
    </tr>
    <tr>
      <th>1</th>
      <td>권선동</td>
      <td>4003</td>
      <td>1.302212</td>
      <td>74</td>
    </tr>
    <tr>
      <th>2</th>
      <td>정자동</td>
      <td>3798</td>
      <td>1.251813</td>
      <td>73</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영통동</td>
      <td>3379</td>
      <td>1.204205</td>
      <td>69</td>
    </tr>
    <tr>
      <th>4</th>
      <td>세류동</td>
      <td>2981</td>
      <td>1.343398</td>
      <td>80</td>
    </tr>
  </tbody>
</table>
</div>



한 사람이 평균적으로 기르는 반려동물 수 많은 순으로 정렬하기 


```python
Chart_2 = animals.sort_values( by = '한사람당동물수', ascending = False, ignore_index = True)
Chart_2.head(5)
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
      <th>법정동명</th>
      <th>등록개체수</th>
      <th>한사람당동물수</th>
      <th>등록품종수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>매산로1가</td>
      <td>149</td>
      <td>1.674157</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1</th>
      <td>교동</td>
      <td>168</td>
      <td>1.584906</td>
      <td>34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>하광교동</td>
      <td>19</td>
      <td>1.583333</td>
      <td>13</td>
    </tr>
    <tr>
      <th>3</th>
      <td>팔달로1가</td>
      <td>26</td>
      <td>1.529412</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>북수동</td>
      <td>103</td>
      <td>1.514706</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
</div>



반려동물 품종수 많은 순으로 정렬하기


```python
Chart_3 = animals.sort_values( by = '등록품종수', ascending = False, ignore_index = True)
Chart_3.head(5)
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
      <th>법정동명</th>
      <th>등록개체수</th>
      <th>한사람당동물수</th>
      <th>등록품종수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>세류동</td>
      <td>2981</td>
      <td>1.343398</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>매탄동</td>
      <td>4321</td>
      <td>1.258299</td>
      <td>78</td>
    </tr>
    <tr>
      <th>2</th>
      <td>권선동</td>
      <td>4003</td>
      <td>1.302212</td>
      <td>74</td>
    </tr>
    <tr>
      <th>3</th>
      <td>정자동</td>
      <td>3798</td>
      <td>1.251813</td>
      <td>73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>인계동</td>
      <td>2619</td>
      <td>1.359107</td>
      <td>71</td>
    </tr>
  </tbody>
</table>
</div>



pandas의 matplotlib 한글출력 위한 font setting


```python
import matplotlib.pyplot as plt
%matplotlib inline
import platform

from matplotlib import font_manager, rc
plt.rcParams['axes.unicode_minus'] = False

if platform.system() == 'Windows':
    path = "c:\Windows\Fonts\malgun.TTF"
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc('font', family=font_name)
```

입력한 동의 반려동물 분포 정보를 알려주는 함수 companion_animal_info() 만들기


```python
def companion_animal_info(place):
    place_index = int(Chart_1[Chart_1['법정동명'] == place].index.values)
    a = Chart_1.loc[place_index, '등록개체수']
    b = place_index + 1       
    print("{}:".format(place))
    print("반려동물 등록 개체수가 {} 마리로, 수원시 55개 동중에 {}번째로 많습니다.".format(a, b))

    place_index = int(Chart_2[Chart_2['법정동명'] == place].index.values)
    a = Chart_2.loc[place_index, '한사람당동물수']
    b = place_index + 1
    print("한사람이 키우는 반려동물 개체수가 평균 {:4f}마리로, 수원시 55개 동중에 {} 번째로 높습니다.".format(a, b))

    place_index = int(Chart_3[Chart_3['법정동명'] == place].index.values)
    a = Chart_3.loc[place_index, '등록품종수']
    b = place_index + 1
    print("등록 품종의 수가 {}종으로, 수원시 55개 동중에 {}번째로 많습니다.".format(a, b))
```

## 5)분석 결과 및 구현 결과

입력한 동에 대한 반려동물 분포 정보 확인(매탄동, 인계동, 망포동)


```python
companion_animal_info("매탄동")
```

    매탄동:
    반려동물 등록 개체수가 4321 마리로, 수원시 55개 동중에 1번째로 많습니다.
    한사람이 키우는 반려동물 개체수가 평균 1.258299마리로, 수원시 55개 동중에 47 번째로 높습니다.
    등록 품종의 수가 78종으로, 수원시 55개 동중에 2번째로 많습니다.
    


```python
companion_animal_info("인계동")
```

    인계동:
    반려동물 등록 개체수가 2619 마리로, 수원시 55개 동중에 6번째로 많습니다.
    한사람이 키우는 반려동물 개체수가 평균 1.359107마리로, 수원시 55개 동중에 19 번째로 높습니다.
    등록 품종의 수가 71종으로, 수원시 55개 동중에 5번째로 많습니다.
    


```python
companion_animal_info("망포동")
```

    망포동:
    반려동물 등록 개체수가 2044 마리로, 수원시 55개 동중에 9번째로 많습니다.
    한사람이 키우는 반려동물 개체수가 평균 1.236540마리로, 수원시 55개 동중에 50 번째로 높습니다.
    등록 품종의 수가 59종으로, 수원시 55개 동중에 12번째로 많습니다.
    

등록개체수 Top 20 그래프


```python
Chart_1.index = Chart_1['법정동명']
_ = Chart_1['등록개체수'].head(20).sort_values().plot(kind = 'barh')
plt.show()
```


 
<img src="/images/output_26_0.png">
    


한사람당동물수 Top 20 그래프


```python
Chart_2.index = Chart_2['법정동명']
_ = Chart_2['한사람당동물수'].head(20).sort_values().plot(kind = 'barh')
plt.show()
```


    
<img src="/images/output_28_0.png">
    


등록품종수 Top 20  그래프 


```python
Chart_3.index = Chart_3['법정동명']
_ = Chart_3['등록품종수'].head(20).sort_values().plot(kind = 'barh')
plt.show()
```


    
<img src="/images/output_30_0.png">
    


## 6) 프로젝트 결론

  먼저, 데이터 분석 도구 pandas에 대해 느낀 점부터 말하겠습니다. 저는 이번 텀프로젝트 때 pandas를 처음 접해봤습니다. 처음보는 여러가지 기능들이 있어서 초반에는 사용하는 데에 있어서 많이 어색하고 시간이 오래 걸렸습니다. 그런데 여러 함수들을 써보게 되면서 점차 그것들의 기능에 익숙해졌습니다. 또한 기존에 알고 있던 반복문이나 조건문 등과 pandas 함수들을 결합하여 사용하니 정말 효율적으로 데이터를 제가 원하는 방식으로 가공할 수 있다는 것을 알게 되었습니다. 이번 텀프로잭트를 통해 pandas와 확실히 더 친해진 것 같아서 pandas의 사용 방법에 조금이나마 익숙해지겠다는 목표는 달성한 것 같습니다.

  프로젝트의 궁극적 목표는 반려동물의 분포를 더 다양한 방식으로, 그리고 더 보기 쉽게 표현하는 것이었습니다. 우선 기존 데이터중 주요 지표인 반려동물 개체수, 품종수에 따라 높은 순으로 동들을 나열한 새로운 표를 만들었으며, 이를 통해 개체수, 품종수가 어떤 동에서 많고 적은지를 더 쉽게 알 수 있었습니다. 또한 한사람이 평균적으로 몇마리의 반려동물을 키우는지를 알기 위해서 동별로 (등록개체수/소유자수)값을 새로운 값으로 저장하고, 이에 따라 새로운 표를 만들었습니다. 이 세 표는 반려동물 분포 현황을 여러 지표에 따라 다양한 방식으로 볼 수 있게 해주었습니다.
  
  마지막으로 두 함수를 구현했는데, 첫번째 함수는 사용자가 입력한 동에 대한 반려동물 분포 정보를 출력하는 함수입니다. 이로 인해서 사용자가 자신이 원하는 동의 데이터에 더 쉽게 접근할 수 있게 된 것 같습니다. 두번째 함수는 개체수, 품종수, 한명당 기르는 동물 수가 가장 많은 10개 동을 각각 그래프로 나타낸 함수였습니다. 이 함수는 반려동물 관련 데이터를 더 직관적으로 볼 수 있게 해주었다고 생각합니다.
  
  이렇게 위에서 말한 것처럼 pandas를 사용하는데에 어느 정도 익숙해졌고, 원본 데이터를 더 다양한 방식으로, 더 보기 쉽게 나타내었기 때문에 프로젝트 목표를 이루었다고 생각합니다. 어려웠던 부분도 있었지만 프로그래밍하는 과정부터 결과까지 너무 즐거웠으며, 앞으로도 텀프로젝트와 같은 프로젝트에 많이 참여해야겠다는 생각이 들었습니다.
