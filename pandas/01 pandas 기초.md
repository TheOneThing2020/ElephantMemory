### 데이타프레임 해부

**주요 구성 요소**
1. 인덱스 : index
2. 열        : columns
3. 값        : values
인덱스 레이블과 열 이름은 각각 개별적인 인덱스나 열의 구성원을 말함
인덱스란 전체 인덱스 레이블을 통칭, 열이란 전체 열 이름을 통칭
인덱스와 열을  통틀어 축(axes)이라 부르며 axis = 0(index), axis = 1(columns)

### 데이터프레임 주요 구성 요소 이용

index = df.index            >> Index 객체 반환    >> index.values       >> ndarray 객체 반환
columns = df.columns   >> Index 객체 반환    >> columns.values  >> ndarray 객체 반환
data = df.values            >> ndarray 객체 반환 

Index 객체는 빠른 선택과 정렬을 위해 해시 테이블을 사용해 구현되어 있으며, 교집합, 합집합 같은 연산을 지원한다는 점에서 파이썬의 집합과 유사하지만, 중복이 허용되고 순서가 있다는 점에서는 구분됨

### 데이터 형식 이해하기

**pandas 문자열 이름**
bool / int / float / O, object / datetime64 / timedelta64 / category
**pandas/numpy 객체**
np.bool / np.int / np.float / np.object / np.datetime64, pd.Timestamp / np.timedelta64, pd.Timedelta / pd.Categorical

df.dtypes >> 각 열의 데이터 형식
df.get_type_counts() >> 각 데이터 형식별 개수

### 데이터 단일 열을 Series 선택하기

1. 인덱싱 연산자(indexing operator) >> 단일 문자열 지정하면 Series 반환
2. 점 연산자(dot operator) >> 특수문자, 공백 검색 X >> 장점 : 메서드 체인이 가능

ser.to_frame() >> Series를 단일 열을 가진 DataFrame 변환

### Series 메서드 호출

dir 함수 >> 객체의 모든 속성과 메서드들을 볼수 있음 : 리스트로 반환
set(dir(pd.Series)) & set(dir(pd(pd.DataFrame)))

ser.value_counts(normalize=False, sort=True, ascending=False, bins = None, dropna=True)
normalize = True >> 상대 빈도가 반환
ser.size
ser.shape
len(ser)
ser.count() >> 누락되지 않은 값의 개수를 반환

min(axis=None, skipna=None, level=None, numeric_only=None, kwargs)
max(axis=None, skipna=None, level=None, numeric_only=None, kwargs)
mean(axis=None, skipna=None, level=None, numeric_only=None, kwargs)
median(axis=None, skipna=None, level=None, numeric_only=None, kwargs)
std(axis=None, skipna=None, level=None, ddof=1, numeric_only=None, kwargs)
sum(axis=None, skipna=None, level=None, numeric_only=None, min_count=0, kwargs)

describe(percentiles=None, include=None, exclude=None) >> 수치과 객체 형식 열은 다른 출력
quantile(q=0.5, interpolation='linear')
isnull() >> 특정 개별값이 누락값인지를 같은 크기의 블리언 Series 반환
notnull() 
hasnans >> 누락값이 있는지를 True, False 반환
fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None, kwargs)
dropna(axis=0, inplace=False, kwargs)

### Series 연산자 사용하기

사측연산은 Series 각 값에 동일하게 적용됨
// >> 몫만 반환하는 나눗셈
% >> 나머지만 반환하는 나누셈

비교연산자의 크다(>), 작다(<), 크거나 같다(>=), 작거나 같다(<=), 같다(==), 다르다(!=)는 조견 결과에 따라 Series 각 값을 True, False 변환하여 반환

위 연산자는 동일한 기능을 하는 메서드가 존재하는데, 디폴트 행동을 제어할 수 있는 매개변수를 가지고 있음

add(other, level=None, fill_value=None, axis=0)
mul(other, level=None, fill_value=None, axis=0)
div(other, level=None, fill_value=None, axis=0)
floordiv(other, level=None, fill_value=None, axis=0)
mod(other, level=None, fill_value=None, axis=0)

lt(other, level=None, fill_value=None, axis=0)    >> Less Than
gt(other, level=None, fill_value=None, axis=0)   >> Greater Than
le(other, level=None, fill_value=None, axis=0)   >> Less Equal
ge(other, level=None, fill_value=None, axis=0)  >> Greater Equal
eq(other, level=None, fill_value=None, axis=0)  >> Equal
ne(other, level=None, fill_value=None, axis=0)  >> Not Equal

### Series 메서드를 함께 사용하기

메서드 체인(method chaining) : 점 표기법을 사용해서 메서드를 연속적으로 호출하는 것을 말함
모든 파이썬 객체는 메서드 체인이 가능하며, 각 객체의 메서드는 객체를 반환하고 반환된 객체는 그 자체로 또 다른 메서드를 갖기 때문(메서드가 반드시 동일한 데이터 형식을 반환해야 하는 건 아님)
파이썬은 보통 하나의 표현식을 여러 줄로 쓸 수 있도록 허영하지 않기 때문에 백슬래시 기호(\)를 사용해 글자를 연결

ser.value_counts().head()
ser.isnull().sum()
ser.fillna(0).\
  .astype(int64)\
  .head()
ser.isnull().mean() >> 누락된 값의 퍼센티지

### 인덱스를 의미 있게 만들기

set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False)
reset_index(level=None, drop=False, inplace=False, col_level=0, col_fill='')
read_csv(index_col=None, parse_dates=False, etc)

### 열과 행 이름 다시 짓기

열 이름을 잘 만들면 그 자체로 설명적이며 간결하고 대소 문자, 공백, 밑줄 등의 여러 특징에 있어 공통된 관행을 따르게 됨

rename(mapper=None, index=None, columns=None, axis=None, copy=True, inplace=False, level=None)
rename 메서드는 예전 값을 새로운 값으로 매핑하는 딕셔너리를 입력으로 받음
renameColumns = {'oldColumn01':'newColumn01', 'oldColumn01':'newColumn02'}
rename(columns=renameColumns)

행과 열 이름을 바꿀 수 있는 여러 방법이 있는데 열과 행의 속성을 파이썬 리스트로 바로 지정할 수도 있으며 이때는 리스트가 열과 행의 레이블과 동일한 개수의 원소를 갖고 있을 때 작동됨
new_index = df.index.tolist() 
new_index[0] = 'newRowName'
df.index = new_index

### 열의 생성과 삭제

열을 생성하는 간단한 방법은 스칼라 값을 할당해 생성
df['new Column'] = 0
df['new Column'] = df['old Column01'] + df['old Column02']

drop(labels=None, axis=0, index=None, columns=None, level=None, inplace=False, errors='raise')
del df['삭제 열']

insert(loc, column, value, allow_duplicates=False)
loc >> 삽입 정수 위치, column >> 새 열의 이름, value >> 새 열의 값






