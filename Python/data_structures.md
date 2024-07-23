# Data Structure 데이터 구조
1. 정의 : 여러 데이터를 효과적으로 사용, 관리하기 위한 구조 (str, list, dict 등)
2. 활용방법 : 각 데이터 구조의 **메서드** 를 호출하여 다양한 기능을 활용하기

## Method
1. 정의 : 객체에 속한 함수 → 객체의 상태를 조작하거나 동작을 수행
2. 호출 방법 : 데이터 타입 객체.메서드()

# 시퀀스 데이터 구조

## 문자열

### 문자열 조회/탐색 및 검증 메서드

| 메서드 | 설명 |
| --- | --- |
| s.find(x) | x의 첫 번째 위치를 반환. 없으면 -1을 반환 |
| s.index(x) | x의 첫 번째 위치를 반환. 없으면 오류 발생 |
| s.isupper() | 대문자 여부 (boolean) |
| s.islower() | 소문자 여부 (boolean) |
| s.isalpha() | 알파벳 문자 여부 *단순 알파벳이 아닌 유니코드 상 Letter (한국어도 포함) (boolean) |

*참고 : 메서드 이름이 is로 시작하면, return 값은 boolean

### 문자열 조작 메서드 (새 문자열 반환)

| 메서드 | 설명 |
| --- | --- |
| s.replace(old, new [,count]) | 바꿀 대상 글자를 새로운 글자로 바꿔서 반환 |
| s.strip([chars]) | 문자열의 **시작과 끝**에 있는 공백이나 지정한 문자를 제거 *중간에 있는 문자는 제거 불가 |
| s.split(sep=None, maxsplit=-1) | 공백이나 특정 문자를 기준으로 분리하여 문자열 **리스트**로 반환 |
| ‘separator’.join(iterable) | 구분자로 iterable의 문자열을 연결한 문자열 반환 |
| s.capitalize() | 문자열의 첫글자는 대문자로, 나머지는 소문자로 바꿔서 반환 |
| s.title() | 문자열에 공백 다음 글자를 대문자로 바꿔서 반환 |
| s.upper() | 문자열 전체를 대문자로 바꿔서 반환 |
| s.lower() | 문자열 전체를 소문자로 바꿔서 반환 |
| s.swapcase() | 대문자는 소문자로, 소문자는 대문자로 바꿔서 반환 |

*메서드는 이어서 사용 가능

```python
text = 'heLLo, woRLD!'
new_text = txt.swapcase().replace('l','z')
print(new_text) # HEllO, WOrld!
# 주의
# 반환 type이 일치해야함
```

## 리스트

### 리스트 값 추가 및 삭제 메서드

| 메서드 | 설명 |
| --- | --- |
| L.append(x) | 리스트 마지막에 항목 x를 추가 |
| L.extend(m) | Iterable m의 모든 항목들을 리스트 끝에 추가(+=과 같은 기능) |
| L.insert(i, m) | 리스트 인덱스 i에 항목 x를 삽입 |
| L.remove(x) | 리스트 가장 왼쪽에 있는 항목(첫번째) x를 제거 항목이 존재하지 않을 경우, ValueError |
| L.pop() | 리스트 가장 오른쪽에 있는 항목(마지막)을 반환 후 제거 |
| L.pop(i) | 리스트의 인덱스 i에 있는 항목을 반환 후 제거 |
| L.clear() | 리스트의 모든 항목 삭제 |

*리스트는 가변이 가능하기 때문에 원본 자체를 수정하는 것

```python
my_list = [1, 2, 3]
my_list.extend([4, 5, 6])
print(my_list) # [1, 2, 3, 4, 5, 6]

my_list.extend(5)
print(my_list) # TypeError : 'int' object is not iterable
```

### 리스트 탐색 및 정렬 메서드
| 메서드 | 설명 |
| --- | --- |
| L.index(x) | 리스트에서 첫 번째로 일치하는 항목 x의 인덱스를 반환 |
| L.count(x) | 리스트에서 항목 x의 개수를 반환 |
| L.reverse() | 리스트의 순서를 역순으로 변경(정렬 X) |
| L.sort() / L.sort(reverse = True) | 리스트를 정렬(매개변수 이용 가능) |

<br/>

# 비시퀀스 데이터 구조
## Dictionary 딕셔너리

고유한 항목들의 정렬되지 않은 컬렉션, 키 :  값 구조

### 딕셔너리 메서드

| 메서드 | 설명 |
| --- | --- |
| D.clear() | 딕셔너리의 모든 키/값 쌍을 제거 |
| D.get(key[,default]) | 키에 연결된 값을 반환하거나 키가 없으면 None 혹은 기본 값(설정한 Default 값)을 반환 <br/>cf ) dict[key]와 차이 : 에러 유무 (dict[key]는 KeyError 발생) |
| D.keys() | 딕셔너리 key를 모은 객체를 반환 (return 데이터 타입 : dict_keys([key1, key2, … ]) |
| D.values() | 딕셔너리 value를 모은 객체를 반환 (return 데이터 타입 : dict_values([value1, value 2, … ]) |
| D.items() | 딕셔너리 key, value 쌍을 모은 객체를 반환 (return 데이터 타입 : dict_items([(key1, value1), (key2, value2), …]) |
| D.pop(key[,default]) | 딕셔너리에서 key를 제거하고 연결됐던 value를 반환 (없으면 error 혹은 default를 반환) |
| D.setdefault(key[,default]) | 딕셔너리 key와 연결된 값을 반환, key가 없다면 default와 연결한 key를 딕셔너리에 추가하고 default를 반환 |
| D.update([other]) | other가 제공하는 key/value 쌍으로 딕셔너리를 갱신, 기존 key는 덮어씀 <br/>cf ) other는 보통 여러 개의 인자를 넣을 수 있다는 의미 |
1. D.items() 예시
    
    ```python
    person = {'name': 'Alice', 'age': 25}
    print(person.items()) # dict_items([('name, 'Alice'), ('age', 25)])
    for item in person.items():
        print(item) # ('name', 'Alice') ('age', 25)
    for key, value in person.items() :
        print(key)
        print(value) # name Alice age 25
    ```
    
2. D.pop() 예시
    
    ```python
    person = {'name': 'Alice', 'age': 25}
    print(person.pop('age')) # 25
    print(person) # {'name': 'Alice'}
    print(person.pop('country')) # KeyError : 'country'
    print(person.pop('country', None)) # None
    ```
    
3. D.setdefault() 예시
    
    ```python
    person = {'name': 'Alice', 'age': 25}
    print(person.setdefault('country', 'KOREA')) # KOREA
    print(person) # {'name': 'Alice', 'age': 25, 'country' : KOREA}
    ```
    
4. D.update() 예시
    
    ```python
    person = {'name': 'Alice', 'age': 25}
    other_person = {'name': 'Jane', 'gender': 'Female'}
    
    person.update(other_person)
    print(person) # {'name': 'Jane', 'age': 25, 'gender': 'Female'}
    
    person.update(age=50, country='KOREA')
    print(person) # {'name': 'Jane', 'age': 50, 'gender': 'Female', 'country' : 'KOREA'}
    ```
    

## Set 세트
고유한 항목들이 정렬되지 않은 컬렉션 (중복 X, 순서 X)

### 세트 메서드

| 메서드 | 설명 |
| --- | --- |
| s.add(x) | 세트에 x를 추가 |
| s.clear() | 세트의 모든 항목을 제거 (출력은 {} 이 아닌, set()로 표기됨) |
| s.remove(x) | 세트에서 항목 x를 제거, 제거하고자 하는 x가 없다면 KeyError 발생 |
| s.pop() | 세트에서 임의의 요소를 제거하고 반환 |
| s.discard(x) | 세트에서 항목 x를 제거, remove와 달리 error 없음 (return 값 None) |
| s.update(iterable) | 세트에 다른 iterable 요소를 추가 |
1. s.remove(x)
    
    ```python
    my_set.remove(2)
    print(my_set) # {'a', 'b', 'c', 1, 3}
    my_set.remove(10)
    print(my_set) # KeyError: 10
    ```
    
2. s.pop()
    
    ```python
    my_set = {'a', 'b', 'c', 1, 2, 3}
    element = my_set.pop()
    
    print(element) # 1 랜덤한 값이 빠짐!
    print(my_set) # {'b', 'c', 2, 3, 'a'}
    ```
    
3. s.update(*iterable*)
    
    ```python
    my_set = {'a', 'b', 'c', 1, 2, 3}
    my_set.update([1, 4, 5])
    print(my_set) # {'c', 4, 'a', 'b', 1, 5, 2, 3}
    ```
    

### 세트의 집합 메서드
| 메서드 | 설명 | 연산자 |
| --- | --- | --- |
| set1.difference | set1에는 들어있지만 set2에는 없는 항목으로 세트를 생성 후 반환 | set1 - set2 |
| set1.intersection(set2) | set1과 set2 모두 들어있는 항목으로 세트를 생성 후 반환  | set1 & set2 |
| set1.issubset(set2) | set1의 항목이 모두 set2에 들어있으면 True를 반환  | set1 ≤ set2 |
| set1.issuperset(set2) | set1가 set2의 항목을 모두 포함하면 True를 반환 | set1 ≥ set2 |
| set1.union(set2) | set1 또는 set2에(혹은 둘다) 들어있는 항목으로 세트를 생성 후 반환 | set1 | set2 |