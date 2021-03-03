# offline_meeting_under_five_people

5인 이상 집합 금지 규정를 지키며 모임을 만들 수 있도록 돕는 코드 (잠복기를 고려해 2주 마다 사용하는 것을 권장)

---

## Code

```python
## skin
string_to_num = {'금':'0', '토':'1', '일':'2'}


## engine
def find_best_day(mixed,decided, days): # mixed를 넣으면 가장 좋은 날을 찾아주는 함수
    priorities = []
    for day in days:
        if len(mixed[day]) > 8:
            priorities.append([priority_over8[len(mixed[day])%4],day])
        else:
            priorities.append([priority_under9[len(mixed[day])%8],day])

        if len(decided[day]) == 0:
            priorities[-1][0] -= 4

    priorities.sort(key=lambda x : (-x[0],x[1])) # 우선순위가 높을수록, 같다면 날짜가 빠를수록
    print('pr', priorities) ## 디버깅 용도
    return priorities[0][1] # 가장 좋은 날짜 반환
    

expected = {'0':set(), '1':set(), '2':set()} # 후보 인원
decided = {'0':set(), '1':set(), '2':set()} # 결정된 인원

priority_over8 = {0:1, 1:4, 2:3, 3:4}
priority_under9 = {0:5, 1:1, 2:3, 3:7, 4:4, 5:2, 6:8, 7:6}

N = int(input('인원 : '))
print('이름과 요일을 입력하세요.')
print('ex) 박정웅 금 토 일')
members = []
for pn in range(N):
    person = list(input('이름 요일 : ').split())
    person = [person[0]] + list(string_to_num[s] for s in person[1:])

    members.append((person[0],person[1:])) # 이름과 날짜를 member 변수에 저장

    for i in person[1:]:
        expected[i].add(person[0])

members.sort(key= lambda x : len(x[1])) # 가능한 날짜 순으로 정렬

for p in range(N):
    mixed = {} # expected와 decided를 섞은 자료
    for day in expected:
        mixed[day] = expected[day] | decided[day]
    print(mixed) ## 디버깅 용도
    person = members[p] # 사람을 불러냄
    print(person[0], person[1]) ## 디버깅 용도
    your_day = find_best_day(mixed,decided,person[1]) # 날짜 결정

    decided[your_day].add(person[0])

    for day in person[1]:
        expected[day].discard(person[0])

    print('ex', expected) ## 디버깅 용도
    print('_________________________________________________________') ## 디버깅 용도
    print('de', decided) ## 디버깅 용도
    print() ## 디버깅 용도

print(decided) ## 디버깅 용도

####################################

when = ['금요일','토요일','일요일']
print('\n\n\n=============오프라인 모임=============')
print('===================================')
for day, people in decided.items():
    print(when[int(day)])
    num = 1
    while people:
        team = []
        if len(people)< 5:
            while people:
                team.append(people.pop())
        elif len(people)%4:
            for i in range(3):
                team.append(people.pop())
        else:
            for i in range(4):
                team.append(people.pop())
        print('team{} :'.format(num),' '.join(team))
        num += 1
    print('\n===================================')
```

### 설명

|      | (dicided 기준) | (mixed 기준) | under 9 | over 8 |
| ---- | -------------- | ------------ | ------- | ------ |
| 인원 | 나누는 인원    | 우선순위     |         |        |
| 1    | 1              | 1            | 1       |        |
| 2    | 2              | 3            | 3       |        |
| 3    | 3              | 7            | 7       |        |
| 4    | 4              | 4            | 4       |        |
| 5    | 2 3            | 2            | 2       |        |
| 6    | 3 3            | 8            | 8       |        |
| 7    | 3 4            | 6            | 6       |        |
| 8    | 4 4            | 5            | 5       |        |
| 9    | 3 3 3          | 4            |         | 4      |
| 10   | 3 3 4          | 3            |         | 3      |
| 11   | 3 4 4          | 4            |         | 4      |
| 12   | 4 4 4          | 1            |         | 1      |
| 13   | 3 3 3 4        | 4            |         |        |
| 14   | 3 3 4 4        | 3            |         |        |
| 15   | 3 4 4 4        | 4            |         |        |
| 16   | 4 4 4 4        | 1            |         |        |
| 17   | 3 3 3 4 4      | 4            |         |        |
| 18   | 3 3 4 4 4      | 3            |         |        |
| 19   | 3 4 4 4 4      | 4            |         |        |
| 20   | 4 4 4 4 4      | 1            |         |        |
| 21   | 3 3 3 4 4 4    | 4            |         |        |
| 22   | 3 3 4 4 4 4    | 3            |         |        |
| 23   | 3 4 4 4 4 4    | 4            |         |        |
| 24   | 4 4 4 4 4 4    | 1            |         |        |
| 25   | 3 3 3 4 4 4 4  | 4            |         |        |

- 각 요일마다 가능한 사람들이 중복으로 들어있는 expected
- 이미 결정된 사람들이 모여있는 decided
- 그리고 이 둘을 섞은 mixed



- mixed에 있는 각 요일 별 가능한 사람 수가 위의 표와 같을 때, 우선순위를 부여하고 가장 우선순위가 큰 요일에 배정



## Test Samples

```
10
a 금 토
b 토 일
c 토 일
d 금 일
e 금 토 일
f 금 토
g 금
h 일
i 금 토 일
j 금 토

====
9
b 토 일
c 토 일
d 금 일
e 금 토 일
f 금 토
g 금
h 일
i 금 토 일
j 금 토

====
9
b 토 일
c 토 일
d 토 일
e 토 일
f 토 일
g 토 일
h 토 일
i 토 일
j 토 일

====
20
a 토 일
b 금 일
c 금 토
d 금 토
e 금 토 일
f 금 일
as 일
d 토
v 금 토
ea 토 일
sf 금 토 일
q 금 일
lf 금
pe 토 일
ee 토 일
wv 토
bt 금
hy 금
uy 금 토
tt 금 토 일

====
26
a 금 토 일
b 금 토 일
c 금
d 금
e 금 토
f 토 일
g 토 일
h 토 일
i 토 일
j 금 토 일
k 금 일
l 금 일
m 토 일
n 금 토
o 금 토
p 토
q 일
r 일
s 금
t 금 토
u 금 일
v 금 토 일
w 금 일
x 토 일
y 금 토
z 토
```
