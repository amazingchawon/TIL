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
| s.replace(old, new, [count]) | 바꿀 대상 글자를 새로운 글자로 바꿔서 반환 |
| s.strip([chars]) | 문자열의 시작과 끝에 있는 공백이나 지정한 문자를 제거 *중간에 있는 문자는 제거 불가 |
| s.split(sep=None, maxsplit=-1) | 공백이나 특정 문자를 기준으로 분리하여 문자열 리스트로 반환 |
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