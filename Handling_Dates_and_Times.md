# Handling Dates and Times





## pd.to_datetime 데이터 타입 변경

- format 형태에 원본 데이터 형태를 맞춰주면 된다


```python
# Load libraries 
import numpy as np 
import pandas as pd

# Create strings 
date_strings = np.array(['03-04-2005 11:35 PM',  
                         '23-05-2010 12:01 AM',  
                         '04-09-2009 09:09 PM'])

# Convert to datetimes   
[pd.to_datetime(date, format='%d-%m-%Y %I:%M %p') for date in date_strings]
```

결과


    [Timestamp('2005-04-03 23:35:00'),
     Timestamp('2010-05-23 00:01:00'),
     Timestamp('2009-09-04 21:09:00')]



- 에러 날 경우 조치 방법. add an argument to the errors parameter to handle problems


```python
# Convert to datetimes 
[pd.to_datetime(date, format="%d-%m-%Y %I:%M %p", errors="coerce") 
 for date in date_strings]
```

* %Y Full year                      ex) 2001 
* %m Month w/ zero padding             ex) 04 
* %d Day of the month w/ zero padding  ex) 09 
* %I Hour (12hr clock) w/ zero padding ex) 02 
* %p AM or PM                          ex) AM 
* %M Minute w/ zero padding            ex) 05 
* %S Second w/ zero padding            ex) 09





# Handling Time Zones

- timezond : 시간대

- add a time zone using tz during creation


```python
# Load library 
import pandas as pd

# Create datetime 
pd.Timestamp('2017-05-01 06:00:00', tz='Europe/London')
```

- add a time zone to a previously created datetime using tz_localize


```python
# Create datetime 
date = pd.Timestamp('2017-05-01 06:00:00')

# Set time zone 
date_in_london = date.tz_localize('Europe/London')

# Show datetime 
date_in_london
```

- convert to a different time zone


```python
# Change time zone 
date_in_london.tz_convert('Africa/Abidjan')
```

- pandas’ Series objects can apply tz_localize and tz_convert to every element


```python
# Create three dates 
dates = pd.Series(pd.date_range('2/2/2002', periods=3, freq='M'))

# Set time zone 
dates.dt.tz_localize('Africa/Abidjan')
```

- timezone 목록 보기


```python
# Load library 
from pytz import all_timezones

# Show two time zones 
all_timezones
```




    ['Africa/Abidjan', 'Africa/Accra', 'Africa/Addis_Ababa', 'Africa/Algiers', 'Africa/Asmara', 'Africa/Asmera', 'Africa/Bamako', 'Africa/Bangui', 'Africa/Banjul', 'Africa/Bissau', 'Africa/Blantyre', 'Africa/Brazzaville', 'Africa/Bujumbura', 'Africa/Cairo', 'Africa/Casablanca', 'Africa/Ceuta', 'Africa/Conakry', 'Africa/Dakar', 'Africa/Dar_es_Salaam', 'Africa/Djibouti', 'Africa/Douala', 'Africa/El_Aaiun', 'Africa/Freetown', 'Africa/Gaborone', 'Africa/Harare', 'Africa/Johannesburg', 'Africa/Juba', 'Africa/Kampala', 'Africa/Khartoum', 'Africa/Kigali', 'Africa/Kinshasa', 'Africa/Lagos', 'Africa/Libreville', 'Africa/Lome', 'Africa/Luanda', 'Africa/Lubumbashi', 'Africa/Lusaka', 'Africa/Malabo', 'Africa/Maputo', 'Africa/Maseru', 'Africa/Mbabane', 'Africa/Mogadishu', 'Africa/Monrovia', 'Africa/Nairobi', 'Africa/Ndjamena', 'Africa/Niamey', 'Africa/Nouakchott', 'Africa/Ouagadougou', 'Africa/Porto-Novo', 'Africa/Sao_Tome', 'Africa/Timbuktu', 'Africa/Tripoli', 'Africa/Tunis', 'Africa/Windhoek', 'America/Adak', 'America/Anchorage', 'America/Anguilla', 'America/Antigua', 'America/Araguaina', 'America/Argentina/Buenos_Aires', 'America/Argentina/Catamarca', 'America/Argentina/ComodRivadavia', 'America/Argentina/Cordoba', 'America/Argentina/Jujuy', 'America/Argentina/La_Rioja', 'America/Argentina/Mendoza', 'America/Argentina/Rio_Gallegos', 'America/Argentina/Salta', 'America/Argentina/San_Juan', 'America/Argentina/San_Luis', 'America/Argentina/Tucuman', 'America/Argentina/Ushuaia', 'America/Aruba', 'America/Asuncion', 'America/Atikokan', 'America/Atka', 'America/Bahia', 'America/Bahia_Banderas', 'America/Barbados', 'America/Belem', 'America/Belize', 'America/Blanc-Sablon', 'America/Boa_Vista', 'America/Bogota', 'America/Boise', 'America/Buenos_Aires', 'America/Cambridge_Bay', 'America/Campo_Grande', 'America/Cancun', 'America/Caracas', 'America/Catamarca', 'America/Cayenne', 'America/Cayman', 'America/Chicago', 'America/Chihuahua', 'America/Coral_Harbour', 'America/Cordoba', 'America/Costa_Rica', 'America/Creston', 'America/Cuiaba', 'America/Curacao', 'America/Danmarkshavn', 'America/Dawson', 'America/Dawson_Creek', 'America/Denver', 'America/Detroit', 'America/Dominica', 'America/Edmonton', 'America/Eirunepe', 'America/El_Salvador', 'America/Ensenada', 'America/Fort_Nelson', 'America/Fort_Wayne', 'America/Fortaleza', 'America/Glace_Bay', 'America/Godthab', 'America/Goose_Bay', 'America/Grand_Turk', 'America/Grenada', 'America/Guadeloupe', 'America/Guatemala', 'America/Guayaquil', 'America/Guyana', 'America/Halifax', 'America/Havana', 'America/Hermosillo', 'America/Indiana/Indianapolis', 'America/Indiana/Knox', 'America/Indiana/Marengo', 'America/Indiana/Petersburg', 'America/Indiana/Tell_City', 'America/Indiana/Vevay', 'America/Indiana/Vincennes', 'America/Indiana/Winamac', 'America/Indianapolis', 'America/Inuvik', 'America/Iqaluit', 'America/Jamaica', 'America/Jujuy', 'America/Juneau', 'America/Kentucky/Louisville', 'America/Kentucky/Monticello', 'America/Knox_IN', 'America/Kralendijk', 'America/La_Paz', 'America/Lima', 'America/Los_Angeles', 'America/Louisville', 'America/Lower_Princes', 'America/Maceio', 'America/Managua', 'America/Manaus', 'America/Marigot', 'America/Martinique', 'America/Matamoros', 'America/Mazatlan', 'America/Mendoza', 'America/Menominee', 'America/Merida', 'America/Metlakatla', 'America/Mexico_City', 'America/Miquelon', 'America/Moncton', 'America/Monterrey', 'America/Montevideo', 'America/Montreal', 'America/Montserrat', 'America/Nassau', 'America/New_York', 'America/Nipigon', 'America/Nome', 'America/Noronha', 'America/North_Dakota/Beulah', 'America/North_Dakota/Center', 'America/North_Dakota/New_Salem', 'America/Ojinaga', 'America/Panama', 'America/Pangnirtung', 'America/Paramaribo', 'America/Phoenix', 'America/Port-au-Prince', 'America/Port_of_Spain', 'America/Porto_Acre', 'America/Porto_Velho', 'America/Puerto_Rico', 'America/Punta_Arenas', 'America/Rainy_River', 'America/Rankin_Inlet', 'America/Recife', 'America/Regina', 'America/Resolute', 'America/Rio_Branco', 'America/Rosario', 'America/Santa_Isabel', 'America/Santarem', 'America/Santiago', 'America/Santo_Domingo', 'America/Sao_Paulo', 'America/Scoresbysund', 'America/Shiprock', 'America/Sitka', 'America/St_Barthelemy', 'America/St_Johns', 'America/St_Kitts', 'America/St_Lucia', 'America/St_Thomas', 'America/St_Vincent', 'America/Swift_Current', 'America/Tegucigalpa', 'America/Thule', 'America/Thunder_Bay', 'America/Tijuana', 'America/Toronto', 'America/Tortola', 'America/Vancouver', 'America/Virgin', 'America/Whitehorse', 'America/Winnipeg', 'America/Yakutat', 'America/Yellowknife', 'Antarctica/Casey', 'Antarctica/Davis', 'Antarctica/DumontDUrville', 'Antarctica/Macquarie', 'Antarctica/Mawson', 'Antarctica/McMurdo', 'Antarctica/Palmer', 'Antarctica/Rothera', 'Antarctica/South_Pole', 'Antarctica/Syowa', 'Antarctica/Troll', 'Antarctica/Vostok', 'Arctic/Longyearbyen', 'Asia/Aden', 'Asia/Almaty', 'Asia/Amman', 'Asia/Anadyr', 'Asia/Aqtau', 'Asia/Aqtobe', 'Asia/Ashgabat', 'Asia/Ashkhabad', 'Asia/Atyrau', 'Asia/Baghdad', 'Asia/Bahrain', 'Asia/Baku', 'Asia/Bangkok', 'Asia/Barnaul', 'Asia/Beirut', 'Asia/Bishkek', 'Asia/Brunei', 'Asia/Calcutta', 'Asia/Chita', 'Asia/Choibalsan', 'Asia/Chongqing', 'Asia/Chungking', 'Asia/Colombo', 'Asia/Dacca', 'Asia/Damascus', 'Asia/Dhaka', 'Asia/Dili', 'Asia/Dubai', 'Asia/Dushanbe', 'Asia/Famagusta', 'Asia/Gaza', 'Asia/Harbin', 'Asia/Hebron', 'Asia/Ho_Chi_Minh', 'Asia/Hong_Kong', 'Asia/Hovd', 'Asia/Irkutsk', 'Asia/Istanbul', 'Asia/Jakarta', 'Asia/Jayapura', 'Asia/Jerusalem', 'Asia/Kabul', 'Asia/Kamchatka', 'Asia/Karachi', 'Asia/Kashgar', 'Asia/Kathmandu', 'Asia/Katmandu', 'Asia/Khandyga', 'Asia/Kolkata', 'Asia/Krasnoyarsk', 'Asia/Kuala_Lumpur', 'Asia/Kuching', 'Asia/Kuwait', 'Asia/Macao', 'Asia/Macau', 'Asia/Magadan', 'Asia/Makassar', 'Asia/Manila', 'Asia/Muscat', 'Asia/Nicosia', 'Asia/Novokuznetsk', 'Asia/Novosibirsk', 'Asia/Omsk', 'Asia/Oral', 'Asia/Phnom_Penh', 'Asia/Pontianak', 'Asia/Pyongyang', 'Asia/Qatar', 'Asia/Qostanay', 'Asia/Qyzylorda', 'Asia/Rangoon', 'Asia/Riyadh', 'Asia/Saigon', 'Asia/Sakhalin', 'Asia/Samarkand', 'Asia/Seoul', 'Asia/Shanghai', 'Asia/Singapore', 'Asia/Srednekolymsk', 'Asia/Taipei', 'Asia/Tashkent', 'Asia/Tbilisi', 'Asia/Tehran', 'Asia/Tel_Aviv', 'Asia/Thimbu', 'Asia/Thimphu', 'Asia/Tokyo', 'Asia/Tomsk', 'Asia/Ujung_Pandang', 'Asia/Ulaanbaatar', 'Asia/Ulan_Bator', 'Asia/Urumqi', 'Asia/Ust-Nera', 'Asia/Vientiane', 'Asia/Vladivostok', 'Asia/Yakutsk', 'Asia/Yangon', 'Asia/Yekaterinburg', 'Asia/Yerevan', 'Atlantic/Azores', 'Atlantic/Bermuda', 'Atlantic/Canary', 'Atlantic/Cape_Verde', 'Atlantic/Faeroe', 'Atlantic/Faroe', 'Atlantic/Jan_Mayen', 'Atlantic/Madeira', 'Atlantic/Reykjavik', 'Atlantic/South_Georgia', 'Atlantic/St_Helena', 'Atlantic/Stanley', 'Australia/ACT', 'Australia/Adelaide', 'Australia/Brisbane', 'Australia/Broken_Hill', 'Australia/Canberra', 'Australia/Currie', 'Australia/Darwin', 'Australia/Eucla', 'Australia/Hobart', 'Australia/LHI', 'Australia/Lindeman', 'Australia/Lord_Howe', 'Australia/Melbourne', 'Australia/NSW', 'Australia/North', 'Australia/Perth', 'Australia/Queensland', 'Australia/South', 'Australia/Sydney', 'Australia/Tasmania', 'Australia/Victoria', 'Australia/West', 'Australia/Yancowinna', 'Brazil/Acre', 'Brazil/DeNoronha', 'Brazil/East', 'Brazil/West', 'CET', 'CST6CDT', 'Canada/Atlantic', 'Canada/Central', 'Canada/Eastern', 'Canada/Mountain', 'Canada/Newfoundland', 'Canada/Pacific', 'Canada/Saskatchewan', 'Canada/Yukon', 'Chile/Continental', 'Chile/EasterIsland', 'Cuba', 'EET', 'EST', 'EST5EDT', 'Egypt', 'Eire', 'Etc/GMT', 'Etc/GMT+0', 'Etc/GMT+1', 'Etc/GMT+10', 'Etc/GMT+11', 'Etc/GMT+12', 'Etc/GMT+2', 'Etc/GMT+3', 'Etc/GMT+4', 'Etc/GMT+5', 'Etc/GMT+6', 'Etc/GMT+7', 'Etc/GMT+8', 'Etc/GMT+9', 'Etc/GMT-0', 'Etc/GMT-1', 'Etc/GMT-10', 'Etc/GMT-11', 'Etc/GMT-12', 'Etc/GMT-13', 'Etc/GMT-14', 'Etc/GMT-2', 'Etc/GMT-3', 'Etc/GMT-4', 'Etc/GMT-5', 'Etc/GMT-6', 'Etc/GMT-7', 'Etc/GMT-8', 'Etc/GMT-9', 'Etc/GMT0', 'Etc/Greenwich', 'Etc/UCT', 'Etc/UTC', 'Etc/Universal', 'Etc/Zulu', 'Europe/Amsterdam', 'Europe/Andorra', 'Europe/Astrakhan', 'Europe/Athens', 'Europe/Belfast', 'Europe/Belgrade', 'Europe/Berlin', 'Europe/Bratislava', 'Europe/Brussels', 'Europe/Bucharest', 'Europe/Budapest', 'Europe/Busingen', 'Europe/Chisinau', 'Europe/Copenhagen', 'Europe/Dublin', 'Europe/Gibraltar', 'Europe/Guernsey', 'Europe/Helsinki', 'Europe/Isle_of_Man', 'Europe/Istanbul', 'Europe/Jersey', 'Europe/Kaliningrad', 'Europe/Kiev', 'Europe/Kirov', 'Europe/Lisbon', 'Europe/Ljubljana', 'Europe/London', 'Europe/Luxembourg', 'Europe/Madrid', 'Europe/Malta', 'Europe/Mariehamn', 'Europe/Minsk', 'Europe/Monaco', 'Europe/Moscow', 'Europe/Nicosia', 'Europe/Oslo', 'Europe/Paris', 'Europe/Podgorica', 'Europe/Prague', 'Europe/Riga', 'Europe/Rome', 'Europe/Samara', 'Europe/San_Marino', 'Europe/Sarajevo', 'Europe/Saratov', 'Europe/Simferopol', 'Europe/Skopje', 'Europe/Sofia', 'Europe/Stockholm', 'Europe/Tallinn', 'Europe/Tirane', 'Europe/Tiraspol', 'Europe/Ulyanovsk', 'Europe/Uzhgorod', 'Europe/Vaduz', 'Europe/Vatican', 'Europe/Vienna', 'Europe/Vilnius', 'Europe/Volgograd', 'Europe/Warsaw', 'Europe/Zagreb', 'Europe/Zaporozhye', 'Europe/Zurich', 'GB', 'GB-Eire', 'GMT', 'GMT+0', 'GMT-0', 'GMT0', 'Greenwich', 'HST', 'Hongkong', 'Iceland', 'Indian/Antananarivo', 'Indian/Chagos', 'Indian/Christmas', 'Indian/Cocos', 'Indian/Comoro', 'Indian/Kerguelen', 'Indian/Mahe', 'Indian/Maldives', 'Indian/Mauritius', 'Indian/Mayotte', 'Indian/Reunion', 'Iran', 'Israel', 'Jamaica', 'Japan', 'Kwajalein', 'Libya', 'MET', 'MST', 'MST7MDT', 'Mexico/BajaNorte', 'Mexico/BajaSur', 'Mexico/General', 'NZ', 'NZ-CHAT', 'Navajo', 'PRC', 'PST8PDT', 'Pacific/Apia', 'Pacific/Auckland', 'Pacific/Bougainville', 'Pacific/Chatham', 'Pacific/Chuuk', 'Pacific/Easter', 'Pacific/Efate', 'Pacific/Enderbury', 'Pacific/Fakaofo', 'Pacific/Fiji', 'Pacific/Funafuti', 'Pacific/Galapagos', 'Pacific/Gambier', 'Pacific/Guadalcanal', 'Pacific/Guam', 'Pacific/Honolulu', 'Pacific/Johnston', 'Pacific/Kiritimati', 'Pacific/Kosrae', 'Pacific/Kwajalein', 'Pacific/Majuro', 'Pacific/Marquesas', 'Pacific/Midway', 'Pacific/Nauru', 'Pacific/Niue', 'Pacific/Norfolk', 'Pacific/Noumea', 'Pacific/Pago_Pago', 'Pacific/Palau', 'Pacific/Pitcairn', 'Pacific/Pohnpei', 'Pacific/Ponape', 'Pacific/Port_Moresby', 'Pacific/Rarotonga', 'Pacific/Saipan', 'Pacific/Samoa', 'Pacific/Tahiti', 'Pacific/Tarawa', 'Pacific/Tongatapu', 'Pacific/Truk', 'Pacific/Wake', 'Pacific/Wallis', 'Pacific/Yap', 'Poland', 'Portugal', 'ROC', 'ROK', 'Singapore', 'Turkey', 'UCT', 'US/Alaska', 'US/Aleutian', 'US/Arizona', 'US/Central', 'US/East-Indiana', 'US/Eastern', 'US/Hawaii', 'US/Indiana-Starke', 'US/Michigan', 'US/Mountain', 'US/Pacific', 'US/Samoa', 'UTC', 'Universal', 'W-SU', 'WET', 'Zulu']



# Selecting Dates and Tines


```python
# Load library 
import pandas as pd

# Create data frame 
dataframe = pd.DataFrame()

# Create datetimes 
dataframe['date'] = pd.date_range('1/1/2001', periods=100000, freq='H')

# Select observations between two datetimes 
dataframe[(dataframe['date'] > '2002-1-1 01:00:00') & (dataframe['date'] <= '2002-1-1 04:00:00')]
```

## set the date column as the DataFrame’s index
- 인덱스로 설정


```python
# Set index 
dataframe = dataframe.set_index(dataframe['date'])
```

## Select observations between two datetimes
- 참조하기 와 Slicing


```python
# Select observations between two datetimes 
dataframe.loc['2002-1-1 01:00:00':'2002-1-1 04:00:00']
```

## 기준 날짜를 기준으로 데이터 나누기  truncate
- ts.truncate(before=None, after=None, axis=None, copy: bool = True)


```python
ts.truncate(after='1/9/2011')
```

## Breaking Up Date Data into Multiple Features
- 시계열 컬럼을 여러개로 쪼개기  년, 월, 일, 시간, 분 등


```python
# Load library 
import pandas as pd

# Create data frame 
dataframe = pd.DataFrame()

# Create five dates 
dataframe['date'] = pd.date_range('1/1/2001', periods=150, freq='W')

# Create features for year, month, day, hour, and minute 
dataframe['year'] = dataframe['date'].dt.year 
dataframe['month'] = dataframe['date'].dt.month 
dataframe['day'] = dataframe['date'].dt.day 
dataframe['hour'] = dataframe['date'].dt.hour 
dataframe['minute'] = dataframe['date'].dt.minute

# Show three rows 
dataframe.head(3)
```

## Calculating the Difference Between Dates
- 시간 차이 구하기


```python
# Load library 
import pandas as pd

# Create data frame 
dataframe = pd.DataFrame()

# Create two datetime features 
dataframe['Arrived'] = [pd.Timestamp('01-01-2017'), pd.Timestamp('01-04-2017')] 
dataframe['Left'] = [pd.Timestamp('01-01-2017'), pd.Timestamp('01-06-2017')]

# Calculate duration between features 
dataframe['Left'] - dataframe['Arrived']
```




    0   0 days
    1   2 days
    dtype: timedelta64[ns]




```python
# 뒤에 'days' 안나오게
pd.Series(delta.days for delta in (dataframe['Left'] - dataframe['Arrived']))
```

# Encoding Days of the Week
- know the day of the week for each date. 

- pandas’ Series.dt property weekday_name:



```python
# Load library 
import pandas as pd

# Create dates 
dates = pd.Series(pd.date_range("2/2/2002", periods=3, freq="M"))

# Show days of the week 
dates.dt.weekday
```




    0    3
    1    6
    2    1
    dtype: int64



# Creating a Lagged Feature ... shift
- create a feature that is lagged n time periods

  과거 데이터를 표현하는 속성을 만들 수 있다


```python
# Load library 
import pandas as pd

# Create data frame 
dataframe = pd.DataFrame()

# Create data 
dataframe["dates"] = pd.date_range("1/1/2001", periods=5, freq="D") 
dataframe["stock_price"] = [1.1,2.2,3.3,4.4,5.5]

# Lagged values by one row 
dataframe["previous_days_stock_price"] = dataframe["stock_price"].shift(1)

# Show data frame 
dataframe
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
      <th>dates</th>
      <th>stock_price</th>
      <th>previous_days_stock_price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2001-01-01</td>
      <td>1.1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2001-01-02</td>
      <td>2.2</td>
      <td>1.1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2001-01-03</td>
      <td>3.3</td>
      <td>2.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2001-01-04</td>
      <td>4.4</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2001-01-05</td>
      <td>5.5</td>
      <td>4.4</td>
    </tr>
  </tbody>
</table>
</div>



# Using Rolling Time Windows
 calculate some statistic for a rolling time

 rolling 은 moving 이라고도 불린다

* including - max(), mean(), count(), corr()


```python
# Load library 
import pandas as pd

# Create datetimes 
time_index = pd.date_range("01/01/2010", periods=5, freq="M")

# Create data frame, set index 
dataframe = pd.DataFrame(index=time_index)

# Create feature 
dataframe["Stock_Price"] = [1,2,3,4,5]

# Calculate rolling mean 
dataframe.rolling(window=2).mean()
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
      <th>Stock_Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010-01-31</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2010-02-28</th>
      <td>1.5</td>
    </tr>
    <tr>
      <th>2010-03-31</th>
      <td>2.5</td>
    </tr>
    <tr>
      <th>2010-04-30</th>
      <td>3.5</td>
    </tr>
    <tr>
      <th>2010-05-31</th>
      <td>4.5</td>
    </tr>
  </tbody>
</table>
</div>



# Handling Missing Data in Time Series

- use interpolation to fill in gaps caused by missing values


```python
# Load libraries 
import pandas as pd 
import numpy as np

# Create date 
time_index = pd.date_range("01/01/2010", periods=5, freq="M")

# Create data frame, set index
dataframe = pd.DataFrame(index=time_index)

# Create feature with a gap of missing values 
dataframe["Sales"] = [1.0,2.0,np.nan,np.nan,5.0]

# Interpolate missing values 
dataframe.interpolate()
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
      <th>Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010-01-31</th>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2010-02-28</th>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2010-03-31</th>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2010-04-30</th>
      <td>4.0</td>
    </tr>
    <tr>
      <th>2010-05-31</th>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



- method="quadratic"


```python
# Interpolate missing values method="quadratic
dataframe.interpolate(")
```

- 모든 값을 채우지 않고 채우는 값의 limit 설정



```python
dataframe.interpolate(limit=1, limit_direction="forward")
```

- df.ffill(), df.bfill() 을 활용하여 직전이나 직후의 데이터로 채우기


```python

```


```python
# Forward-fill 
dataframe.ffill()

# Back-fill 
dataframe.bfill()
```


```python

```

# 시계열 데이터의 sampling

시계열 데이터는 랜덤샘플링을 하면 안된다

train 에는 과거 데이터
test 에는 미래 데이터가 있어야 한다


```python
def split_train_test_time(df, date):  # df 는 시계열 데이터 프레임, date 는 기준 날짜
    train_df = df[df['시계열컬럼'] < date]
    test_df = df[df['시계열컬럼'] >= date]
    return train_df, test_df
```

.pop 을 이용해서 다시 train_x, train_y, test_x, test_y  로 나눠라
