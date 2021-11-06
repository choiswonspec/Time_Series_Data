* pandas 는 표준 시계열 도구와 데이터 알고리즘을 제공한다..
* 대량의 시계열 데이터를 쉽게 나누고, 집계하며 효과적으로 다루는 것이 가능
* 불규칙적이며 고정된 빈도를 갖는 시계열을 리샘플링 가능하다.
* 금융, 경제, 서버 로그 데이터 분석 등에 용이


# 시계열 핵심 개념 요약

* pd.Timestamp(2017,1,1,12) 는 특정 시간을 저장하여 객체 생성
* pd.Period(2007) 는 특정 기간을 저장하여 객체 생성

* period를 timestamp로 바꾸기

new_timestamp = period.to_timestamp(freq='H', how ='start')

* date_range(): fixed frequency datetimeindex를 리턴함. 일정 기간 내의 인덱스를 생성한다.  리스트가 생성된다!!

dr2 = pd.date_range(start='1/1/18', end='1/1/19', freq='M')

# 파이썬 표준 라이브러리 ... datetime, time, calender 모듈

# datetime 모듈
* datetime 모듈은 날짜와 시간을 모두 저장하며 마이크로초까지 지원

* datetime 모듈의 자료형
    * date : 그레고리안 달력을 사용해서 날짜(연, 월, 일)을 저장
    * time : 하루의 시간을 시, 분, 초, 마이크로초 단위로 저장
    * datetime : 날짜와 시간을 저장
    * timedelta : 두 datetime 값 간의 차이(일, 초, 마이크로초)를 표현
    * tzinfo : 지역시간대를 저장하기 위한 기본 자료형

# 현재시간


```python
from datetime import datetime
now = datetime.now()
now
```




    datetime.datetime(2020, 5, 26, 15, 27, 29, 747284)




```python
now.year, now.month, now.day # tab을 통해 확인
```




    (2020, 5, 26)



# 두 datetime 객체 간의 시간 차 ... 
datetime.timedelta() 안에 표현됨.


```python
delta = datetime(2011, 1, 7) - datetime(2008, 6, 24, 8, 15)
delta
```




    datetime.timedelta(days=926, seconds=56700)




```python
delta.days, delta.seconds
```




    (926, 56700)



timedelta를 더하거나 빼서 새로운 datetime 객체 생성


```python
from datetime import timedelta

start = datetime(2011, 1, 7)
start + timedelta(12)
```




    datetime.datetime(2011, 1, 19, 0, 0)




```python
start - 2*timedelta(12)
```




    datetime.datetime(2010, 12, 14, 0, 0)



# datetime을 문자열로 변환
* datetime 객체와 pandas의 Timestamp 객체는 str 메서드나 strftime 메서드에 포맷 규칙을 넘겨 문자열로 나타낼 수 있다


```python
stamp = datetime(2011, 1, 3)
str(stamp)
```




    '2011-01-03 00:00:00'




```python
stamp.strftime('%Y-%m-%d'), stamp.strftime('%y-%m-%d') 

''' <포맷코드>
%H : 시간. 24시간 형식 [00, 23]
%H : 시간. 12시간 형식 [01, 12]
%H : 2자리 분[00,59]
%H : 초[00, 61]   # 60, 61은 윤초
%H : 정수로 나타낸 요일[0(일요일), 6]
%H : 연중 주차[00, 53]. 일요일을 그 주의 첫번째 날로 간주하며, 그 해에서 첫번째 일요일 앞에 있는 날은 0주차가 된다
%H : 연중 주차[00, 53]. 월요일을 그 주의 첫번째 날로 간주하며, 그 해에서 첫번째 월요일 앞에 있는 날은 0주차가 된다
%H : UTC 시간대 오프셋을 +HHMM 또는 -HHMM 으로 표현.  만약 시간대를 신경 쓰지 않는다면 비워둔다.
%H : %Y-%m-%d 형식에 대한 축약 (ex: 2012-4-18)
%H : %m/$d/%y 형식에 대한 축약 (ex: 04/18/12)
'''
```




    ('2011-01-03', '11-01-03')



# 문자열을 datetime 으로 변환 
* 위의 포맷 코드와 datetime.strptime을 사용


```python
value = '2011-01-03'
datetime.strptime(value, '%Y-%m-%d')
```




    datetime.datetime(2011, 1, 3, 0, 0)




```python
datestrs = ['7/6/2011','8/6/2011']
[datetime.strptime(x, '%m/%d/%Y') for x in datestrs]
```




    [datetime.datetime(2011, 7, 6, 0, 0), datetime.datetime(2011, 8, 6, 0, 0)]



pandas 와 함께 설치되는 dateutil 패키지 안에 parser.parse 메서드로도 가능
* dateutil 은 거의 대부분의 사람이 인지하는 날짜 표현 방식을 파싱 가능


```python
# pandas 와 함께 설치되는 dateutil 패키지 안에 parser.parse 메서드로도 가능
# dateutil 은 거의 대부분의 사람이 인지하는 날짜 표현 방식을 파싱 가능
# 단점은 날짜로 인식되지 않길 바라는 문자열도 날짜로 인식해버림
import pandas as pd
from dateutil.parser import parse
parse('2011-01-03')
```




    datetime.datetime(2011, 1, 3, 0, 0)




```python
parse('Jan 31, 1997 10:45 PM')
```




    datetime.datetime(1997, 1, 31, 22, 45)




```python
#국제 로케일의 경우 날짜가 월 앞에 오는 경우가 흔하다. 이때 dayfirst = True 로 처리 가능
parse('6/12/2011', dayfirst=True)
```




    datetime.datetime(2011, 12, 6, 0, 0)



# pd.to_datetime()
* pandas 는 NumPy의 datetime64 자료형을 사용해서 나노초의 정밀도를 가지는 Timestamp를 저장한다.
* pandas 의 가장 기본적인 시계열 객체의 종류는 파이썬 문자열이나 datetime 객체로 표현되는 timestamp로 색인된 Series 이다.


```python
# object 타입을 datetime64[ns] 타입으로 바꾼다   ## format 을 지정해줘서 인식이 잘되게 한다
df['Birth'] = pd.to_datetime(df['Birth'], format='%Y-%m-%d %H:%M:%S', errors='raise')
```


```python
datestrs = ['2011-07-06 12:00:00', '2011-08-06 00:00:00']

pd.to_datetime(datestrs)
```




    DatetimeIndex(['2011-07-06 12:00:00', '2011-08-06 00:00:00'], dtype='datetime64[ns]', freq=None)




```python
# 누락된 값(None, 빈 문자열 등) 으로 간주되어야 할 값도 처리 가능
idx = pd.to_datetime(datestrs + [None])
idx, idx[2] 
```




    (DatetimeIndex(['2011-07-06 12:00:00', '2011-08-06 00:00:00', 'NaT'], dtype='datetime64[ns]', freq=None),
     NaT,
     array([False, False,  True]))




```python
pd.isnull(idx)
# NaT : Not a Time 은 pandas에서 누락된 Timestamp data를 나타낸다
```

# 색인, 선택, 부분 선택
pandas 의 Series 와 동일하게 동작


```python
from datetime import datetime
import numpy as np

dates = [datetime(2011, 1, 2), datetime(2011, 1, 5),
        datetime(2011, 1, 7), datetime(2011, 1, 8),
        datetime(2011, 1, 10), datetime(2011, 1, 12)]
ts = pd.Series(np.random.randn(6), index=dates)
ts #ts.index, ts+ts[::2], ts.index.dtype
```




    2011-01-02   -0.308131
    2011-01-05    0.272502
    2011-01-07    1.406795
    2011-01-08    0.920643
    2011-01-10    0.328677
    2011-01-12   -0.011445
    dtype: float64




```python
stamp = ts.index[2]
ts[stamp]
```




    -0.3385513467419298



해석할 수 있는 날짜를 문자열로 넘겨 편리하게 사용 가능


```python
ts['1/02/2011'], ts['20110102']
```




    (-0.30813051356639, -0.30813051356639)



연을 넘기거나 연, 월 만 넘기거나 등으로 데이터의 일부 구간 선택 가능


```python
longer_ts = pd.Series(np.random.rand(1000), index=pd.date_range('1/1/2000', periods=1000))
longer_ts
```




    2000-01-01    0.519301
    2000-01-02    0.071997
    2000-01-03    0.383181
    2000-01-04    0.257046
    2000-01-05    0.937582
                    ...   
    2002-09-22    0.240221
    2002-09-23    0.811621
    2002-09-24    0.808616
    2002-09-25    0.671505
    2002-09-26    0.093859
    Freq: D, Length: 1000, dtype: float64




```python
longer_ts['2001']
```




    2001-01-01    0.156985
    2001-01-02    0.271970
    2001-01-03    0.738269
    2001-01-04    0.381865
    2001-01-05    0.511474
                    ...   
    2001-12-27    0.557630
    2001-12-28    0.846412
    2001-12-29    0.998712
    2001-12-30    0.303630
    2001-12-31    0.678010
    Freq: D, Length: 365, dtype: float64




```python
longer_ts['2001-05']
```




    2001-05-01    0.776796
    2001-05-02    0.016056
    2001-05-03    0.792217
    2001-05-04    0.059198
    2001-05-05    0.475798
    2001-05-06    0.773729
    2001-05-07    0.560737
    2001-05-08    0.595965
    2001-05-09    0.270381
    2001-05-10    0.078765
    2001-05-11    0.962131
    2001-05-12    0.559143
    2001-05-13    0.432665
    2001-05-14    0.658965
    2001-05-15    0.194054
    2001-05-16    0.530115
    2001-05-17    0.130923
    2001-05-18    0.334771
    2001-05-19    0.923295
    2001-05-20    0.290330
    2001-05-21    0.721626
    2001-05-22    0.417263
    2001-05-23    0.690877
    2001-05-24    0.388637
    2001-05-25    0.445232
    2001-05-26    0.249443
    2001-05-27    0.833219
    2001-05-28    0.566839
    2001-05-29    0.373835
    2001-05-30    0.251154
    2001-05-31    0.851242
    Freq: D, dtype: float64



# 구간 잘라내기. Slicing
* 날짜문자열, datetime, 타임스탬프 모두 넘길 수 있다
* 이 방식은 데이터 복사가 발생하지 않고 원본에 바로 반영된다.


```python
ts[datetime(2011, 1, 7):]
```




    2011-01-07    1.406795
    2011-01-08    0.920643
    2011-01-10    0.328677
    2011-01-12   -0.011445
    dtype: float64




```python
ts['2011-01-07':]
```




    2011-01-07    1.406795
    2011-01-08    0.920643
    2011-01-10    0.328677
    2011-01-12   -0.011445
    dtype: float64




```python
# 대부분의 시계열 데이터는 연댛순으로 정렬되기 때문에 범위를 지정하기 위해 시계열에 포함하지 않고 타임스탬프를 이용해서 Series를 나눌 수 있다
ts['1/6/2011' : '1/11/2011']
```




    2011-01-07    1.406795
    2011-01-08    0.920643
    2011-01-10    0.328677
    dtype: float64



truncate() 를 이용한 slicing
* ts.truncate(before=None, after=None, axis=None, copy: bool = True)


```python
ts.truncate(after='1/9/2011')
```




    2011-01-02   -0.308131
    2011-01-05    0.272502
    2011-01-07    1.406795
    2011-01-08    0.920643
    dtype: float64



DataFrame 에 index가 datetime 일 경우 슬라이싱
* row 참조 기능 .loc 사용


```python
dates = pd.date_range('1/1/2000', periods=100, freq='W-WED')
long_df = pd.DataFrame(np.random.randn(100,4),
                      index=dates,
                      columns=['Colorado','Texas','New York','Ohio'])
long_df.loc['5-2001']  
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2001-05-02</th>
      <td>0.270703</td>
      <td>0.345239</td>
      <td>1.551062</td>
      <td>-0.700505</td>
    </tr>
    <tr>
      <th>2001-05-09</th>
      <td>0.169896</td>
      <td>0.393084</td>
      <td>-1.429712</td>
      <td>0.722444</td>
    </tr>
    <tr>
      <th>2001-05-16</th>
      <td>-1.650167</td>
      <td>-1.522499</td>
      <td>0.242961</td>
      <td>0.010436</td>
    </tr>
    <tr>
      <th>2001-05-23</th>
      <td>-0.078233</td>
      <td>-0.998946</td>
      <td>-0.162862</td>
      <td>0.661041</td>
    </tr>
    <tr>
      <th>2001-05-30</th>
      <td>-0.194965</td>
      <td>0.811979</td>
      <td>-0.058347</td>
      <td>0.324458</td>
    </tr>
  </tbody>
</table>
</div>



# 중복된 색인을 갖는 시계열


```python
dates = pd.DatetimeIndex(['1/1/2000', '1/2/2000', '1/2/2000',
                         '1/2/2000', '1/3/2000',])
dup_ts = pd.Series(np.arange(5), index=dates)
dup_ts
```




    2000-01-01    0
    2000-01-02    1
    2000-01-02    2
    2000-01-02    3
    2000-01-03    4
    dtype: int32



is_unique 를 통해 유일하지 않음을 확인


```python
dup_ts.index.is_unique
```




    False




```python
dup_ts['1/3/2000'] # 중복 없는 인덱스
```




    4




```python
dup_ts['1/2/2000'] # 중복 있는 인덱스
```




    2000-01-02    1
    2000-01-02    2
    2000-01-02    3
    dtype: int32



중복되는 타임스탬프를 가지는 데이터 집계
* groupby 에 level = 0 (단일 단계 인덱싱) 


```python
dup_ts.groupby(level=0).mean()
```




    2000-01-01    0
    2000-01-02    2
    2000-01-03    4
    dtype: int32




```python
dup_ts.groupby(level=0).count()
```




    2000-01-01    1
    2000-01-02    3
    2000-01-03    1
    dtype: int64




```python
dup_ts.groupby(level=0).sum()
```




    2000-01-01    0
    2000-01-02    6
    2000-01-03    4
    dtype: int32



# 날짜 범위 생성하기... pd.date_range()
* 지정한 길이 만큼 DatetimeIndex 생성
* date_range 는 기본적으로 입력해준 시작 시간이나 종료 시간의 타임스태프를 보존함


```python
index = pd.date_range('2012-04-01', '2012-06-01')
index
```




    DatetimeIndex(['2012-04-01', '2012-04-02', '2012-04-03', '2012-04-04',
                   '2012-04-05', '2012-04-06', '2012-04-07', '2012-04-08',
                   '2012-04-09', '2012-04-10', '2012-04-11', '2012-04-12',
                   '2012-04-13', '2012-04-14', '2012-04-15', '2012-04-16',
                   '2012-04-17', '2012-04-18', '2012-04-19', '2012-04-20',
                   '2012-04-21', '2012-04-22', '2012-04-23', '2012-04-24',
                   '2012-04-25', '2012-04-26', '2012-04-27', '2012-04-28',
                   '2012-04-29', '2012-04-30', '2012-05-01', '2012-05-02',
                   '2012-05-03', '2012-05-04', '2012-05-05', '2012-05-06',
                   '2012-05-07', '2012-05-08', '2012-05-09', '2012-05-10',
                   '2012-05-11', '2012-05-12', '2012-05-13', '2012-05-14',
                   '2012-05-15', '2012-05-16', '2012-05-17', '2012-05-18',
                   '2012-05-19', '2012-05-20', '2012-05-21', '2012-05-22',
                   '2012-05-23', '2012-05-24', '2012-05-25', '2012-05-26',
                   '2012-05-27', '2012-05-28', '2012-05-29', '2012-05-30',
                   '2012-05-31', '2012-06-01'],
                  dtype='datetime64[ns]', freq='D')




```python
pd.date_range(start='2012-04-01', periods=20)  # 시작 날짜 설정, periods 로 갯수 설정
```




    DatetimeIndex(['2012-04-01', '2012-04-02', '2012-04-03', '2012-04-04',
                   '2012-04-05', '2012-04-06', '2012-04-07', '2012-04-08',
                   '2012-04-09', '2012-04-10', '2012-04-11', '2012-04-12',
                   '2012-04-13', '2012-04-14', '2012-04-15', '2012-04-16',
                   '2012-04-17', '2012-04-18', '2012-04-19', '2012-04-20'],
                  dtype='datetime64[ns]', freq='D')




```python
pd.date_range(end='2012-06-01', periods=20) # 종료 날짜 설정
```




    DatetimeIndex(['2012-05-13', '2012-05-14', '2012-05-15', '2012-05-16',
                   '2012-05-17', '2012-05-18', '2012-05-19', '2012-05-20',
                   '2012-05-21', '2012-05-22', '2012-05-23', '2012-05-24',
                   '2012-05-25', '2012-05-26', '2012-05-27', '2012-05-28',
                   '2012-05-29', '2012-05-30', '2012-05-31', '2012-06-01'],
                  dtype='datetime64[ns]', freq='D')




```python
pd.date_range('2000-01-01','2000-12-01', freq='BM') # BM 을 통해 월의 마지막 영업일을 포함하도록 설정 
```




    DatetimeIndex(['2000-01-31', '2000-02-29', '2000-03-31', '2000-04-28',
                   '2000-05-31', '2000-06-30', '2000-07-31', '2000-08-31',
                   '2000-09-29', '2000-10-31', '2000-11-30'],
                  dtype='datetime64[ns]', freq='BM')



기본 시계열 빈도 옵션
* 사진 참조
![%EA%B8%B0%EB%B3%B8%EC%8B%9C%EA%B3%84%EC%97%B4%EB%B9%88%EB%8F%84.jpg](attachment:%EA%B8%B0%EB%B3%B8%EC%8B%9C%EA%B3%84%EC%97%B4%EB%B9%88%EB%8F%84.jpg)

등등 자세한것은 구글링

자정에 맞추어 '''정규화'''하고 싶을 경우 normalize 옵션 사용


```python
pd.date_range('2012-05-02 12:56:31', periods=5, normalize=True)
```




    DatetimeIndex(['2012-05-02', '2012-05-03', '2012-05-04', '2012-05-05',
                   '2012-05-06'],
                  dtype='datetime64[ns]', freq='D')



freq 옵션을 통해 일정 간격 설정


```python
pd.date_range('2000-01-01', '2000-01-03 23:59', freq='4h')
```




    DatetimeIndex(['2000-01-01 00:00:00', '2000-01-01 04:00:00',
                   '2000-01-01 08:00:00', '2000-01-01 12:00:00',
                   '2000-01-01 16:00:00', '2000-01-01 20:00:00',
                   '2000-01-02 00:00:00', '2000-01-02 04:00:00',
                   '2000-01-02 08:00:00', '2000-01-02 12:00:00',
                   '2000-01-02 16:00:00', '2000-01-02 20:00:00',
                   '2000-01-03 00:00:00', '2000-01-03 04:00:00',
                   '2000-01-03 08:00:00', '2000-01-03 12:00:00',
                   '2000-01-03 16:00:00', '2000-01-03 20:00:00'],
                  dtype='datetime64[ns]', freq='4H')




```python
pd.date_range('2000-01-01', periods=10, freq='1h30min')
```




    DatetimeIndex(['2000-01-01 00:00:00', '2000-01-01 01:30:00',
                   '2000-01-01 03:00:00', '2000-01-01 04:30:00',
                   '2000-01-01 06:00:00', '2000-01-01 07:30:00',
                   '2000-01-01 09:00:00', '2000-01-01 10:30:00',
                   '2000-01-01 12:00:00', '2000-01-01 13:30:00'],
                  dtype='datetime64[ns]', freq='90T')




```python
# pandas 에 없는 날짜 연산을 제공하기 위해 사용자가 직접 빈도 클래스 정의 가능.. 관련 내용은 찾아봐라!
```

# 데이터 이동 ... shift()
* 시간 축에서 앞이나 뒤로 이동
* shift를 하게 되면 시작이나 끝에 결측치가 발생
* 주로 퍼센트 변화 계산할 때 흔히 사용


```python
ts = pd.Series(np.random.randn(4), index=pd.date_range('1/1/2000', periods=4, freq='M'))
ts
```




    2000-01-31    0.028530
    2000-02-29   -0.187200
    2000-03-31   -1.786450
    2000-04-30   -0.167646
    Freq: M, dtype: float64




```python
# 값들이 앞 뒤로 움직임
ts.shift(2)
```




    2000-01-31        NaN
    2000-02-29        NaN
    2000-03-31    0.02853
    2000-04-30   -0.18720
    Freq: M, dtype: float64




```python
ts.shift(-2)
```




    2000-01-31   -1.786450
    2000-02-29   -0.167646
    2000-03-31         NaN
    2000-04-30         NaN
    Freq: M, dtype: float64



년, 월, 일 단위를 변경함


```python
ts.shift(2, freq='M') # 두달씩 올라감 
```




    2000-03-31    0.028530
    2000-04-30   -0.187200
    2000-05-31   -1.786450
    2000-06-30   -0.167646
    Freq: M, dtype: float64




```python
ts.shift(3, freq='D')  # 3일씩
```




    2000-02-03    0.028530
    2000-03-03   -0.187200
    2000-04-03   -1.786450
    2000-05-03   -0.167646
    dtype: float64




```python
ts.shift(1, freq='90T')   # 90분씩
```




    2000-01-31 01:30:00    0.028530
    2000-02-29 01:30:00   -0.187200
    2000-03-31 01:30:00   -1.786450
    2000-04-30 01:30:00   -0.167646
    Freq: M, dtype: float64



오프셋만큼 날짜 shift 하기


```python
from pandas.tseries.offsets import Day, MonthEnd

now = datetime(2011, 11, 17)
now + 3 * Day()
```




    Timestamp('2011-11-20 00:00:00')




```python
now + MonthEnd() # 월 말
```




    Timestamp('2011-11-30 00:00:00')




```python
now + MonthEnd(2) # 다음달 말
```




    Timestamp('2011-12-31 00:00:00')



rollforward 와 rollback 메서드를 사용해 명시적으로 각 날짜를 앞, 뒤로 이동


```python
MonthEnd()
```




    <MonthEnd>




```python
MonthEnd().rollforward(now)
```




    Timestamp('2011-11-30 00:00:00')




```python
MonthEnd().rollback(now)
```




    Timestamp('2011-10-31 00:00:00')



위 메서드를 groupby 와 함께 사용


```python
ts = pd.Series(np.random.randn(20), index=pd.date_range('1/15/2000', periods=20, freq='4d'))
ts
```




    2000-01-15   -0.609506
    2000-01-19    1.844518
    2000-01-23   -0.230221
    2000-01-27   -0.691114
    2000-01-31   -0.736334
    2000-02-04    0.693934
    2000-02-08    0.083407
    2000-02-12    1.436977
    2000-02-16   -0.109462
    2000-02-20   -1.226281
    2000-02-24   -0.080209
    2000-02-28   -1.132787
    2000-03-03   -2.638454
    2000-03-07    0.483537
    2000-03-11    0.494531
    2000-03-15   -1.021041
    2000-03-19   -0.621121
    2000-03-23    0.355639
    2000-03-27   -1.846258
    2000-03-31   -0.084035
    Freq: 4D, dtype: float64



# 시간대 다루기
* 국제표준시 UTC 를 주로 사용
* python 에서 시간대 정보는 전 세계의 시간대 정보를 모아둔 '올슨 데이터베이스'를 담고 있는 서드파티 라이브러리 'pytz' 에서 얻어온다.

pytz 에서 pytz.timezone 을 사용해 시간대 객체를 얻는다.


```python
import pytz
pytz.common_timezones[-10:]
```




    ['Pacific/Wake',
     'Pacific/Wallis',
     'US/Alaska',
     'US/Arizona',
     'US/Central',
     'US/Eastern',
     'US/Hawaii',
     'US/Mountain',
     'US/Pacific',
     'UTC']




```python
tz = pytz.timezone('America/New_York')
tz
```




    <DstTzInfo 'America/New_York' LMT-1 day, 19:04:00 STD>



시간대 지정해서 날짜 범위 생성


```python
pd.date_range('3/9/2012 9:30', periods=10, freq='D', tz='UTC')
```




    DatetimeIndex(['2012-03-09 09:30:00+00:00', '2012-03-10 09:30:00+00:00',
                   '2012-03-11 09:30:00+00:00', '2012-03-12 09:30:00+00:00',
                   '2012-03-13 09:30:00+00:00', '2012-03-14 09:30:00+00:00',
                   '2012-03-15 09:30:00+00:00', '2012-03-16 09:30:00+00:00',
                   '2012-03-17 09:30:00+00:00', '2012-03-18 09:30:00+00:00'],
                  dtype='datetime64[ns, UTC]', freq='D')



지역화 시간으로 변환 ... tz_localize


```python
ts
```




    2000-01-15   -0.609506
    2000-01-19    1.844518
    2000-01-23   -0.230221
    2000-01-27   -0.691114
    2000-01-31   -0.736334
    2000-02-04    0.693934
    2000-02-08    0.083407
    2000-02-12    1.436977
    2000-02-16   -0.109462
    2000-02-20   -1.226281
    2000-02-24   -0.080209
    2000-02-28   -1.132787
    2000-03-03   -2.638454
    2000-03-07    0.483537
    2000-03-11    0.494531
    2000-03-15   -1.021041
    2000-03-19   -0.621121
    2000-03-23    0.355639
    2000-03-27   -1.846258
    2000-03-31   -0.084035
    Freq: 4D, dtype: float64




```python
ts_utc = ts.tz_localize('UTC')
ts_utc
```




    2000-01-15 00:00:00+00:00   -0.609506
    2000-01-19 00:00:00+00:00    1.844518
    2000-01-23 00:00:00+00:00   -0.230221
    2000-01-27 00:00:00+00:00   -0.691114
    2000-01-31 00:00:00+00:00   -0.736334
    2000-02-04 00:00:00+00:00    0.693934
    2000-02-08 00:00:00+00:00    0.083407
    2000-02-12 00:00:00+00:00    1.436977
    2000-02-16 00:00:00+00:00   -0.109462
    2000-02-20 00:00:00+00:00   -1.226281
    2000-02-24 00:00:00+00:00   -0.080209
    2000-02-28 00:00:00+00:00   -1.132787
    2000-03-03 00:00:00+00:00   -2.638454
    2000-03-07 00:00:00+00:00    0.483537
    2000-03-11 00:00:00+00:00    0.494531
    2000-03-15 00:00:00+00:00   -1.021041
    2000-03-19 00:00:00+00:00   -0.621121
    2000-03-23 00:00:00+00:00    0.355639
    2000-03-27 00:00:00+00:00   -1.846258
    2000-03-31 00:00:00+00:00   -0.084035
    Freq: 4D, dtype: float64



먼저 지역화 후, 다른 시간대로 변환 가능 tz_convert()


```python
ts_utc.tz_convert('Europe/Berlin')
```




    2000-01-15 01:00:00+01:00   -0.609506
    2000-01-19 01:00:00+01:00    1.844518
    2000-01-23 01:00:00+01:00   -0.230221
    2000-01-27 01:00:00+01:00   -0.691114
    2000-01-31 01:00:00+01:00   -0.736334
    2000-02-04 01:00:00+01:00    0.693934
    2000-02-08 01:00:00+01:00    0.083407
    2000-02-12 01:00:00+01:00    1.436977
    2000-02-16 01:00:00+01:00   -0.109462
    2000-02-20 01:00:00+01:00   -1.226281
    2000-02-24 01:00:00+01:00   -0.080209
    2000-02-28 01:00:00+01:00   -1.132787
    2000-03-03 01:00:00+01:00   -2.638454
    2000-03-07 01:00:00+01:00    0.483537
    2000-03-11 01:00:00+01:00    0.494531
    2000-03-15 01:00:00+01:00   -1.021041
    2000-03-19 01:00:00+01:00   -0.621121
    2000-03-23 01:00:00+01:00    0.355639
    2000-03-27 02:00:00+02:00   -1.846258
    2000-03-31 02:00:00+02:00   -0.084035
    Freq: 4D, dtype: float64



Timestamp 객체도 시간대를 고려한 형태로 변환 가능


```python
stamp = pd.Timestamp('2011-03-12 04:00')
stamp_utc = stamp.tz_localize('utc')
stamp_utc.tz_convert('America/New_York')
```




    Timestamp('2011-03-11 23:00:00-0500', tz='America/New_York')




```python
# timestamp 객체 생성 시 시간대를 직접 넘겨주기도 가능
stamp_moscow = pd.Timestamp('2011-03-12 04:00', tz='Europe/Moscow')
stamp_moscow
```




    Timestamp('2011-03-12 04:00:00+0300', tz='Europe/Moscow')




```python
stamp_utc.value
```




    1299902400000000000




```python
stamp_utc.tz_convert('America/New_York').value
```




    1299902400000000000



pandas 의 DateOffset 객체를 이용해 시간 연산 수행.. 이때 가능하면 일광절약시간 고려


```python
from pandas.tseries.offsets import Hour
stamp = pd.Timestamp('2012-03-12 01:30', tz='US/Eastern')
stamp, stamp + Hour()
```




    (Timestamp('2012-03-12 01:30:00-0400', tz='US/Eastern'),
     Timestamp('2012-03-12 02:30:00-0400', tz='US/Eastern'))



다른 시간대 간의 연산
* 서로 다른 시간대 두 시계열이 하나로 합쳐지면 결과는 UTC가 된다.


```python
rng = pd.date_range('3/7/2012 9:30', periods=10, freq='B')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts
```




    2012-03-07 09:30:00   -1.268508
    2012-03-08 09:30:00   -0.473910
    2012-03-09 09:30:00    0.171508
    2012-03-12 09:30:00    1.398610
    2012-03-13 09:30:00   -0.222705
    2012-03-14 09:30:00    0.273888
    2012-03-15 09:30:00   -0.059277
    2012-03-16 09:30:00    0.610620
    2012-03-19 09:30:00    1.414807
    2012-03-20 09:30:00    0.697891
    Freq: B, dtype: float64




```python
ts1 = ts[:7].tz_localize('Europe/London')
ts2 = ts1[2:].tz_convert('Europe/Moscow')
result = ts1 + ts2
```


```python
result.index
```




    DatetimeIndex(['2012-03-07 09:30:00+00:00', '2012-03-08 09:30:00+00:00',
                   '2012-03-09 09:30:00+00:00', '2012-03-12 09:30:00+00:00',
                   '2012-03-13 09:30:00+00:00', '2012-03-14 09:30:00+00:00',
                   '2012-03-15 09:30:00+00:00'],
                  dtype='datetime64[ns, UTC]', freq='B')



# 기간과 기간 연산
Period 클래스를 통해 며칠, 몇 개월, 몇 분기, 몇 해 같은 기간을 표현 가능.  앞서 봤던 freq를 이용해 생성


```python
p = pd.Period(2007, freq='A-DEC')
p
```




    Period('2007', 'A-DEC')



정수를 더하거나 빼서 편리하게 기간 이동 가능


```python
p + 5
```




    Period('2012', 'A-DEC')




```python
p - 2
```




    Period('2005', 'A-DEC')



같은 빈도를 가지면 차이는 둘 사이의 간격이 된다


```python
pd.Period('2014', freq='A-DEC') - p
```




    <7 * YearEnds: month=12>



일반적인 기간 범위는 period_range 함수로 생성


```python
rng = pd.period_range('2000-01-01', '2000-06-30', freq='M')
rng
```




    PeriodIndex(['2000-01', '2000-02', '2000-03', '2000-04', '2000-05', '2000-06'], dtype='period[M]', freq='M')



PeriodIndex 클래스는 순차적인 기간을 저장하며 pandas 자료구조에서 index로 사용 가능


```python
pd.Series(np.random.randn(6), index = rng )
```




    2000-01    1.778841
    2000-02   -0.246339
    2000-03    1.721819
    2000-04    0.186121
    2000-05   -0.235685
    2000-06   -0.743171
    Freq: M, dtype: float64



문자열 배열을 이용해 PeriodIndex 클래스 생성도 가능


```python
values = ['2001Q3', '2002Q2', '2003Q1']
index = pd.PeriodIndex(values, freq='Q-DEC')
index
```




    PeriodIndex(['2001Q3', '2002Q2', '2003Q1'], dtype='period[Q-DEC]', freq='Q-DEC')



# Period 의 빈도 변환
기관과 PeriodIndex 객체는 asfreq 메서드를 통해 다른 빈도로 변환 가능.
* ex) 새해 첫날부터 시작하는 연간 빈도를 월간 빈도로 변환


```python
# 연간 빈도를 월간 빈도로 변환
p = pd.Period('2007', freq='A-DEC')
p, p.asfreq('M', how='start'), p.asfreq('M', how='end')
```




    (Period('2007', 'A-DEC'), Period('2007-01', 'M'), Period('2007-12', 'M'))




```python
# 회계연도 마감이 12월이 아닌 경우 결과값이 달라짐
p = pd.Period('2007', freq='A-JUN')
p, p.asfreq('M', how='start'), p.asfreq('M', how='end')
```




    (Period('2007', 'A-JUN'), Period('2006-07', 'M'), Period('2007-06', 'M'))




```python
내용 더있음...
```

# 분기 빈도


```python

```

# 타임스탬프와 기간 서로 변환하기


```python

```

# 배열로 PeriodIndex 생성하기


```python

```

# Grouping Rows by Time

## resample 과 빈도 변환
* 다운샘플링 : 상위 빈도의 데이터를 하위 빈도로 집계
* 업샘플링 : 반대 과정
* 모든 리샘플링이 위에 두가지에 속하는 것은 아니다 ex) W-WED(수요일 기준으로 한주) 를 W-FRI 로 변경.

* pandas 객체는 resample 메서드를 가진다. 빈도 변환 과 관련된 모든 작업에서 유용.
* resample 은 groupby와 비슷,, 데이터를 그룹 짓고 요약함수 적용


```python
rng = pd.date_range('2001-01-01', periods=100, freq='D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts
```




    2001-01-01   -1.578534
    2001-01-02    1.365715
    2001-01-03   -0.478872
    2001-01-04   -0.324351
    2001-01-05   -0.423557
                    ...   
    2001-04-06    0.390752
    2001-04-07   -1.365153
    2001-04-08    1.990838
    2001-04-09    0.002768
    2001-04-10   -0.432058
    Freq: D, Length: 100, dtype: float64




```python
ts.resample('M').mean()  ## 월단위로 정리.. W 이면 주 단위
```




    2001-01-31   -0.075583
    2001-02-28   -0.176329
    2001-03-31   -0.099528
    2001-04-30    0.123627
    Freq: M, dtype: float64




```python
ts.resample('M', kind='period').mean()
```




    2001-01   -0.075583
    2001-02   -0.176329
    2001-03   -0.099528
    2001-04    0.123627
    Freq: M, dtype: float64



resample 인자....  사진 참고

# 다운 샘플링
* 고려해야할 사항 1. 각 간격의 양끝 중 어느 쪽을 닫아둘 것인가.. 2. 집계하려는 구간의 라벨을 간격의 시작으로 할지 끝으로 할지


```python
rng = pd.date_range('2001-01-01', periods=12, freq='T')
ts = pd.Series(np.arange(12), index=rng)
ts
```




    2001-01-01 00:00:00     0
    2001-01-01 00:01:00     1
    2001-01-01 00:02:00     2
    2001-01-01 00:03:00     3
    2001-01-01 00:04:00     4
    2001-01-01 00:05:00     5
    2001-01-01 00:06:00     6
    2001-01-01 00:07:00     7
    2001-01-01 00:08:00     8
    2001-01-01 00:09:00     9
    2001-01-01 00:10:00    10
    2001-01-01 00:11:00    11
    Freq: T, dtype: int32




```python
# 5분 단위로 묶어서 그룹의 합 집계
ts.resample('5min', closed='right').sum()
```




    2000-12-31 23:55:00     0
    2001-01-01 00:00:00    15
    2001-01-01 00:05:00    40
    2001-01-01 00:10:00    11
    Freq: 5T, dtype: int32




```python
ts.resample('5min', closed='right', label = 'right').sum()  # 각 그룹의 오른쪽 값을 라벨로 사용
```




    2001-01-01 00:00:00     0
    2001-01-01 00:05:00    15
    2001-01-01 00:10:00    40
    2001-01-01 00:15:00    11
    Freq: 5T, dtype: int32




```python
# 간격을 좀 더 명확히 보여주고 싶은 경우 loffset 옵션 사용 ... shift 메서드를 사용해도 같은 결과
ts.resample('5min', closed='right', label = 'right', loffset='-1s').sum()
```




    2000-12-31 23:59:59     0
    2001-01-01 00:04:59    15
    2001-01-01 00:09:59    40
    2001-01-01 00:14:59    11
    Freq: 5T, dtype: int32



# OHLC  resampling
* 각 버킷에 4가지 값을 계산하여 시계열 데이터를 집계
* 시가open, 고가high, 저가low, 종가close .. 
* ohlc()  .. 를 통해 해당 값을 담고 있는 DataFrame 을 얻을 수 있다.


```python
ts.resample('5min').ohlc()
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
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2001-01-01 00:00:00</th>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2001-01-01 00:05:00</th>
      <td>5</td>
      <td>9</td>
      <td>5</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2001-01-01 00:10:00</th>
      <td>10</td>
      <td>11</td>
      <td>10</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



# 업샘플링과 보간


```python
frame = pd.DataFrame(np.random.randn(2,4), 
                     index=pd.date_range('1/1/2000', periods=2, freq='W-WED'), 
                     columns=['Colorado', 'Texas','New York', 'Ohio'])
frame
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-05</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-12</th>
      <td>-1.021550</td>
      <td>-1.292904</td>
      <td>-0.246678</td>
      <td>0.960577</td>
    </tr>
  </tbody>
</table>
</div>



asfreq 메서드를 이용해 상위 빈도로 리샘플링..   사이 값들은 결측치가 들어간다


```python
df_daily = frame.resample('D').asfreq()
df_daily
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-05</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-08</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-09</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-11</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-12</th>
      <td>-1.021550</td>
      <td>-1.292904</td>
      <td>-0.246678</td>
      <td>0.960577</td>
    </tr>
  </tbody>
</table>
</div>



결측치 문제를 해결하기 위해 ffill() 와 reindex 를 사용해 보간 가능


```python
frame.resample('D').ffill()
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-05</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-08</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-09</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-10</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-11</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-12</th>
      <td>-1.021550</td>
      <td>-1.292904</td>
      <td>-0.246678</td>
      <td>0.960577</td>
    </tr>
  </tbody>
</table>
</div>



limit 옵션을 사용해서 보간법을 적용할 범위 지정 가능


```python
frame.resample('D').ffill(limit=2)
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-05</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-06</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-07</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-08</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-09</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-10</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-11</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2000-01-12</th>
      <td>-1.021550</td>
      <td>-1.292904</td>
      <td>-0.246678</td>
      <td>0.960577</td>
    </tr>
  </tbody>
</table>
</div>



새로운 날짜 색인은 이전 색인과 겹쳐질 필요가 없다


```python
frame.resample("W-THU").ffill()
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01-06</th>
      <td>-0.888422</td>
      <td>-1.572583</td>
      <td>0.660352</td>
      <td>-0.450029</td>
    </tr>
    <tr>
      <th>2000-01-13</th>
      <td>-1.021550</td>
      <td>-1.292904</td>
      <td>-0.246678</td>
      <td>0.960577</td>
    </tr>
  </tbody>
</table>
</div>



# 기간 resampling


```python
frame = pd.DataFrame(np.random.randn(24,4),
                    index = pd.period_range('1-2000','12-2001',freq="M"),
                    columns=['Colorado', 'Texas','New York', 'Ohio'])
frame[:5]
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000-01</th>
      <td>-0.521453</td>
      <td>1.656228</td>
      <td>0.790857</td>
      <td>0.526245</td>
    </tr>
    <tr>
      <th>2000-02</th>
      <td>-1.299952</td>
      <td>-0.548988</td>
      <td>0.700096</td>
      <td>1.049250</td>
    </tr>
    <tr>
      <th>2000-03</th>
      <td>0.785337</td>
      <td>-0.708217</td>
      <td>-0.755342</td>
      <td>-0.007974</td>
    </tr>
    <tr>
      <th>2000-04</th>
      <td>-0.771582</td>
      <td>0.701451</td>
      <td>-0.375758</td>
      <td>0.278037</td>
    </tr>
    <tr>
      <th>2000-05</th>
      <td>-1.036908</td>
      <td>0.921484</td>
      <td>0.759016</td>
      <td>-0.505290</td>
    </tr>
  </tbody>
</table>
</div>




```python
annual_frame = frame.resample('A-DEC').mean()
annual_frame
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Q-DEC : 12워를 연도마감으로 하는 분기 주기
annual_frame.resample('Q-DEC').ffill()
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000Q1</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2000Q2</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2000Q3</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2000Q4</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q1</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
    <tr>
      <th>2001Q2</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
    <tr>
      <th>2001Q3</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
    <tr>
      <th>2001Q4</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
  </tbody>
</table>
</div>




```python
## 새로운 빈도에서 구간의 끝을 어느쪽에 둘지 결정
annual_frame.resample('Q-DEC', convention='end').ffill()  
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000Q4</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q1</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q2</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q3</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q4</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
  </tbody>
</table>
</div>



기간의 업샘플링과 다운샘플링은 좀 더 엄격하다
* 다운샘플링의 경우 대상 빈도는 반드시 원본 빈도의 하위 기간이어야한다
* 업샘플리으이 경우 대상 빈도는 반드시 원본 빈도의 상위기간이어야 한다.

위조건을 만족하지 않으면 예외 발생,,,


```python
annual_frame.resample('Q-MAR').ffill()
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
      <th>Colorado</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000Q4</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q1</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q2</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q3</th>
      <td>-0.165914</td>
      <td>-0.055152</td>
      <td>-0.213472</td>
      <td>-0.233952</td>
    </tr>
    <tr>
      <th>2001Q4</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
    <tr>
      <th>2002Q1</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
    <tr>
      <th>2002Q2</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
    <tr>
      <th>2002Q3</th>
      <td>-0.276265</td>
      <td>-0.067412</td>
      <td>-0.099453</td>
      <td>0.156575</td>
    </tr>
  </tbody>
</table>
</div>



# 이동창 함수


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```

# 시계열 인덱스를 여러 컬럼으로 나누기


```python
drunk_driving = pd.read_csv("./경찰청_음주운전적발_200518.csv",index_col = 0 ,encoding = 'euc-kr')
```


```python
drunk_driving['측정일시'] = pd.to_datetime(drunk_driving['측정일시'])

```


```python
drunk_driving.info()
```


```python
# drunk_driving.set_index('측정일시', inplace=False)
# drunk_driving.head()
```


```python
# 연/월/일/시/분/초 데이터를 추출
drunk_driving['date']= pd.to_datetime(drunk_driving['측정일시'])
drunk_driving['eventdatetime_year']= drunk_driving['date'].dt.year
drunk_driving['eventdatetime_month']= drunk_driving['date'].dt.month
drunk_driving['eventdatetime_day']=  drunk_driving['date'].dt.day
drunk_driving['eventdatetime_hour']= drunk_driving['date'].dt.hour
drunk_driving['eventdatetime_minute']= drunk_driving['date'].dt.minute
drunk_driving['eventdatetime_second']= drunk_driving['date'].dt.second

print(drunk_driving.shape)
drunk_driving[['date','eventdatetime_year','eventdatetime_month','eventdatetime_day','eventdatetime_hour','eventdatetime_minute','eventdatetime_second']].head()
```


```python

```

# pandas Expanding and Rolling


```python
import pandas as pd
import numpy as np
```


```python
s = pd.Series(np.random.randn(1000), index = pd.date_range('1/1/2015', periods=1000))
s.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1f23b127b08>




    
![png](output_177_1.png)
    


# .rollling() 
window 사이즈 만큼 이동을 시켜준다 ... 이동 평균
* 데이터 1개라도 존재하면 평균을 내고 싶으면 min_periods= 1 옵션을 설정해라
* 중간을 기준으로 이동평균을 구하고 싶으면 center = True 옵션을 설정해라
* ex) 사이즈가 2라고, 평균 내는 연산을 한다면

     (1,2) 평균 내고 (2,3) 평균 (3,4) 이런식으로 진행됨


```python
d = s.cumsum() # 누적합 구하기
d.plot()
r = d.rolling(window=60) 
r.mean().plot()    # 이동 평균
f = d.rolling(window=120) 
f.mean().plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1f23bff5a08>




    
![png](output_179_1.png)
    



```python
d
```




    2015-01-01    -0.058374
    2015-01-02     0.080698
    2015-01-03    -0.006886
    2015-01-04    -0.564125
    2015-01-05    -0.265108
                    ...    
    2017-09-22   -20.668795
    2017-09-23   -20.882444
    2017-09-24   -22.019566
    2017-09-25   -21.837187
    2017-09-26   -22.077442
    Freq: D, Length: 1000, dtype: float64




```python


```


```python
df = pd.DataFrame(np.random.randn(1000, 4), index = pd.date_range('1/1/2015', periods=1000), columns=['A','B','C','D'])

```


```python
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2015-01-01</th>
      <td>1.425666</td>
      <td>2.149509</td>
      <td>0.122379</td>
      <td>2.318027</td>
    </tr>
    <tr>
      <th>2015-01-02</th>
      <td>0.751747</td>
      <td>-0.513922</td>
      <td>-0.613403</td>
      <td>-0.388076</td>
    </tr>
    <tr>
      <th>2015-01-03</th>
      <td>0.732418</td>
      <td>-0.186808</td>
      <td>0.994698</td>
      <td>1.148399</td>
    </tr>
    <tr>
      <th>2015-01-04</th>
      <td>-1.480700</td>
      <td>-1.123620</td>
      <td>1.202415</td>
      <td>-1.331936</td>
    </tr>
    <tr>
      <th>2015-01-05</th>
      <td>0.735153</td>
      <td>-2.615513</td>
      <td>-0.768452</td>
      <td>-0.459238</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-09-22</th>
      <td>1.201722</td>
      <td>-2.026786</td>
      <td>-0.124118</td>
      <td>0.882530</td>
    </tr>
    <tr>
      <th>2017-09-23</th>
      <td>0.020828</td>
      <td>-0.539167</td>
      <td>-0.359782</td>
      <td>-0.684966</td>
    </tr>
    <tr>
      <th>2017-09-24</th>
      <td>-0.946336</td>
      <td>-1.220112</td>
      <td>2.242341</td>
      <td>-0.004892</td>
    </tr>
    <tr>
      <th>2017-09-25</th>
      <td>0.171749</td>
      <td>-0.540238</td>
      <td>-2.174289</td>
      <td>0.429598</td>
    </tr>
    <tr>
      <th>2017-09-26</th>
      <td>-0.991697</td>
      <td>0.219724</td>
      <td>1.323421</td>
      <td>-1.350885</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 4 columns</p>
</div>




```python
df = df.cumsum() # 누적합 구하기
df.plot()
df.rolling(window=60).sum().plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1f23bf09fc8>




    
![png](output_184_1.png)
    



    
![png](output_184_2.png)
    


# Expanding
Expanding : Contains all prior values(사이즈 키워가며 연산)

ex) 평균 내기

(1,2) 평균 (1,2,3) 평균 (1,2,3,4) 평균 ....


```python
df.expanding(min_periods=1).mean().plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1f23da7c3c8>




    
![png](output_186_1.png)
    



```python
df.expanding?
```


```python
df = pd.DataFrame({'B': [0, 1, 2, np.nan, 4]})
df
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
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1f23a697688>




    
![png](output_189_1.png)
    



```python
df.expanding(2).sum()
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
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.expanding(2).sum().plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1f23dc33e88>




    
![png](output_191_1.png)
    


Method summary  다양한 통계치 사용 가능

* count() :Number of non-null observations
* sum() : Sum of values
* mean() : Mean of values
* median() :Arithmetic median of values
* min() : Minimum
* max() : Maximum
* std() : Unbiased standard deviation
* var() : Unbiased variance
* skew() : Unbiased skewness (3rd moment)
* kurt() : Unbiased kurtosis (4th moment)
* quantile() : Sample quantile (value at %)
* apply() : Generic apply
* cov() : Unbiased covariance (binary)
* corr() : Correlation (binary)

# Autocorrelation and Partial Autocorrelation
* Autocorrelation : 서로 다른 lags에서 series가 어떻게 상호 연관되는지 측정... 즉, 시점 t와 시점 t-k 둘 사이의 상관관계
* Partial Autocorrelation : 과거 lag에 대한 series regression으로 해석, 표준 선형 회귀 분석과 동일한 방식...  lag K에서, K 간격을 두고 있는 series 값과 현재 t 시점 사이의 모든 상관 관계... 즉, 시점 t와 시점 t-k 둘 사이의 모든 시점들과의 상관관계


```python

```

# Time series decompostion and Random walks
Trends, seasonlaity and noise

* Trend: 일관된 time series의 상승 하강 기울기

* Seasonality: 싸인 함수처럼 time series의 주기적인 패턴

* Noise: 특이치 또는 누락된 값


```python
rcParams['figure.figsize'] = 11, 9
decomposed_google_volume = sm.tsa.seasonal_decompose(google["High"],freq=360) # The frequncy is annual
figure = decomposed_google_volume.plot()
plt.show()

# trend 또는 seasonality를 지닌 time series는 non-stationary하다.
# 이들은 서로 다른 시간대의 time series 값에 영향을 미치기 때문이다.
```

# White noise
time series가 white noise인 경우, 그것은 무작위 숫자의 순서여서 예측할 수 없다.

series의 예측 오류가 white noise가 아니라면 예측 모델에 개선이 이루어질 수 있음을 시사한다.

time series는 변수가 독립적이고 평균 0으로 동일하게 분포된 경우 white noise이다.

이는 모든 변수가 동일한 분산을 가지며 각 값은 series의 다른 모든 값과 0의 상관관계를 갖는다는 것을 의미한다.

white noise는 stationary하다.

series의 변수가 가우스 분포에서 도출된 경우 serires를 Gaussian white noise라고 한다.

# Random Walk
Random walk는 확률적 또는 무작위 과정으로 알려진 수학적 개념으로, 정수와 같은 일부 수학적인 공간에 대한 일련의 무작위 스텝으로 구성된 경로를 설명한다.

일반적으로 주식 얘기하자면, 오늘의 가격=어제의 가격+ noise

Random walk는 noise가 랜덤하기 때문에 잘 예측되지 않는다.

Random walk는 시간에 따른 편차의 평균이 0이지만 분산은 시간에 비례하여 증가하게 된다. 따라서, 앞뒤로 움직일 확률이 동일하다고 해도 시간이 흐름에 따라 평균에서 점차 벗어나는 경향을 보인다.

# Stationarity
Stationarity time series는 시간이 지남에 따라 모두 일정한 평균, 분산, 상관관계 등과 같은 통계적 특성 중 하나이다. 

Strong stationarity: 시간 이동시 무조건적으로 joint probability distribution가 변하지 않는 확률적 과정이다.

                          즉, 평균이나 분산 같은 파라미터는 시간이 지나도 변하지 않는다

Weak stationarity: 평균, 분산, 상관관계가 시간에 따라 항상 동일한 과정

시간에 따라 달라지는 non-stationary series는 time series를 모델링할 때 고려해야 할 파라미터가 너무 많기 때문에 stationarity가 중요하다. diff() method는 non-stationary series를 stationary series로 변환시켜 준다.


```python

```


```python

```

# 시계열 예제 
'파이썬으로 데이터 주무르기' 책 내용


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```

# Modelling using statstools

### AR models
AR(autoregressive) 모델은 random process의 유형을 나타내는 것이다. 따라서 자연, 경제 등에서 특정 시간 변동 프로세스를 설명하는데 사용된다. AR 모델은 출력 변수가 이전의 값과 확률적 용어(완벽하게 예측 불가능한 용어)에 따라 선형적으로 의존한다고 명시한다. 따라서 모델은 확률적 차이 방정식의 형태를 띤다.

AR models: white noise의 현재 값과 자기 자신의 과거값의 선형 가중합으로 이루어진 정상 확률 모형


```python
# AR(1) MA(1) model:AR parameter = +0.9
rcParams['figure.figsize'] = 16, 12
plt.subplot(4,1,1)
ar1 = np.array([1, -0.9]) # We choose -0.9 as AR parameter is +0.9
ma1 = np.array([1])
AR1 = ArmaProcess(ar1, ma1)
sim1 = AR1.generate_sample(nsample=1000)
plt.title('AR(1) model: AR parameter = +0.9')
plt.plot(sim1)
# We will take care of MA model later
# AR(1) MA(1) AR parameter = -0.9
plt.subplot(4,1,2)
ar2 = np.array([1, 0.9]) # We choose +0.9 as AR parameter is -0.9
ma2 = np.array([1])
AR2 = ArmaProcess(ar2, ma2)
sim2 = AR2.generate_sample(nsample=1000)
plt.title('AR(1) model: AR parameter = -0.9')
plt.plot(sim2)
# AR(2) MA(1) AR parameter = 0.9
plt.subplot(4,1,3)
ar3 = np.array([2, -0.9]) # We choose -0.9 as AR parameter is +0.9
ma3 = np.array([1])
AR3 = ArmaProcess(ar3, ma3)
sim3 = AR3.generate_sample(nsample=1000)
plt.title('AR(2) model: AR parameter = +0.9')
plt.plot(sim3)
# AR(2) MA(1) AR parameter = -0.9
plt.subplot(4,1,4)
ar4 = np.array([2, 0.9]) # We choose +0.9 as AR parameter is -0.9
ma4 = np.array([1])
AR4 = ArmaProcess(ar4, ma4)
sim4 = AR4.generate_sample(nsample=1000)
plt.title('AR(2) model: AR parameter = -0.9')
plt.plot(sim4)
plt.show()
```

Forecasting a simulated model


```python
model = ARM(sim1, order=(1,0))
result= model.fit()
# ϕ는 약 0.9이며, 이것이 우리가 처음 시뮬레이션한 모델에서 AR 파라미터로 선택한 것이다.
```

Predicting the models


```python
result.plot_predict(start=900,end=1010)
plt.show()
```


```python
rmse = math.sqrt(mean_squared_error(sim1[900:1011], result.predict(start=900,end=999)))
print("The root mean squared error is {}.".format(rmse))
```

## MA models
MA(moving-average) model은 일변량 time series 모델링을 위한 공통 접근방식이다.

MA 모델은 출력 변수가 확률적 term의 현재 및 다양한 과거 값에 따라 선형적으로 좌우됨을 명시한다.

=> Today's returns = mean + today's noise + yesterday's noise

Simulating MA(1) model


```python
rcParams['figure.figsize'] = 16, 6
ar1 = np.array([1])
ma1 = np.array([1, -0.5])
MA1 = ArmaProcess(ar1, ma1)
sim1 = MA1.generate_sample(nsample=1000)
plt.plot(sim1)
```

Forecasting the simulated MA model


```python
model = ARMA(sim1, order=(0,1))
result = model.fit()
```

Prediction using MA models


```python
# Forecasting and predicting montreal humidity
model = ARMA(humidity["Montreal"].diff().iloc[1:].values, order=(0,3))
result = model.fit()
print(result.summary())
print("μ={} ,θ={}".format(result.params[0],result.params[1]))
result.plot_predict(start=1000, end=1100)
plt.show()
```

## ARMA models
ARMA(Autoregressive moving average) 모델은 두 개의 다항식(AR, MA)의 관점에서 (weakly) stationary stochastic process에 대한 모사적인 설명을 제공. AR과 MA 모델의 융합이다. 
=> Today's return = mean + Yesterday's return + noise + yesterday's noise.


```python

```


```python

```


```python

```


```python

```
