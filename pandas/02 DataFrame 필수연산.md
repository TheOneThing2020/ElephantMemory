### 데이터 프레임  열 선택  
1. 인덱싱 연산자
2. 메서드 사용
   1. select_dtypes(include = [ ], exclude = [ ])
      include=['int64', 'float64', 'bool'], 모든 수치열 ['number']
      get_dtype_counts()  >> 특정 데이터 형식의 열 개수를 반환
   2. filter(items =  , like = , regex= , axis= )
      items = ['열이름', '열이름'] >> 정확한 열이름을 매개변수 받음, 틀려도 오류 발생 x
      like = '문자열' >> 문자열을 포함하고 있는 모든 열을 반환
      regex= '\d' , 숫자 포함 열 >> 정규식을 사용해 열을 검색

### 열이름 정렬하기
나름의 일관된 가이드 라인을 설정하는 것이 바람직함
1. 각 열을 이산이나 연속에 따라 분류
2. 이산 열과 연속 열 내에서 공통적인 열은 그룹으로 분류
3. 그룹 내 가장 중요한 열을 제일 먼저 나오게 하고 범주형 열을 연속형보다 먼저 나오게 함
    df.columns >> 가이드 라인에 맞게 열을 리스트로 정리 >> df = df[정리된 리스트]

### 전체 데이터프레임에 대한 연산

df.shape >> (전체행 개수, 전체열 개수) 
df.size    >> 전체행 개수 * 전체열개수
df.ndim  >> 차원반환( ser 1차원, df 2차원 )
len(df)    >> 전체행 개수
df.count() >>  각 열에 누락값을 제외한 개수를 ser 반환(열이름이 인덱스)
min, max, mean, median, std, etc >> 열이름이 인텍스, 계산 결과 값
:: 수치 열은 누락값은 그냥 지나침(skipna = True : default)
describe(percentiles = [ ]) >> 기술통계량, 분위수 계산

### 메서드 체인
df.isnull() >> df : 블리언 변경
df.isnull().sum() >> ser : 열 인덱스, 결과값 값
df.isnull().sum().sum() >> scalar : 전체 누락값
any() >> 값 중 True 값이 있는지를 반환 
all() >> 값 전체가 True 값이 있는지를 반환

### 데이터프레임에서 연산자 이용

데이터프레임이 산수나 비교 연산자 중 하나와 직접 연결되면 각 열의 모든 값에 연산이 적용(데이터가 동일한 형태이어야 함)
컴퓨터의 소수점 연산의 부정확으로 인해 0.0001을 미리 더하고 연산을 시작(중간값인 경우 짝수로 맞춤 : 동률일 경우 짝수 기법)
df.round(2)

### 누락값 비교

pandas는 Numpy의 NaN(np.nan) 객체를 사용해 누락값을 표시, 이 객체는 스스로가 자신과 같다는 등식이 성립하지 않음(예외 : 같이 않음만 성립) 
np.nan == np.nan >> False
None == None     >> True
np.nan > 5 >> False, np.nan < 5 >> False, np.nan != 5 >> True

---------

Series & DataFrame은 == 연산자를 사용해 원소끼리 비교를 수행한 후 동일 크기의 객체를 반환
원소끼리 비교할 때는 원소 중 NaN 있는 경우 위 설명에 있듯이 예기치 않은 결과가 나옴
누락값을 세는 가장 기초적인 방법은 isnull() 메서드 사용이며, 두 객체(DataFrame) 전체를 비교하는 정확한 방법은 == 연산자를 사용하는 것이 아니라 equals() 메서드를 사용하는 것임

### 데이터 프레임 연산의 방향 바꾸기

많은 데이터프레임 메서드는 axis 매개변수를 가지고 있으며, 연산이 일어나는 방향을 결정함
axis >> 0 : index > default , 1 : columns 두 가지 값만 가짐
연산의 방향은 항상 헷갈리는 부분 중 하나이며,  0(index), 1(columns)를 방향으로 이해하여 1(columns) 경우 열 방향으로 연산이 일어나다고 이해(결국 행이 연산됨)

### 대학 캠퍼스의 다양성 지수 발견

college.isnull().sum(axis=1).sort_values(ascending=False)
college.dropna(how='all')
df.dropna(axis = 0, how = 'any', thresh = None, subset = None, inplace = False)

1. how >> 'any' > 하나라도 NaN 있으면 삭제,  'all' > 모두 NaN 이면 삭제
2. thresh : int    > 열 또는 행 중 NaN 값이 int 값만큼 되는 행은 나두고 나머지는 삭제
3. subset : 빈 값을 검사하는 필드 이름 지정

df.value_counts(normalize=False, sort=True, ascending=False, bins = None, dropna=True)

