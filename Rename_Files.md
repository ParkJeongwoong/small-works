# Rename Files

> 어떤 폴더의 파일 이름이 모두 'First name' + 'Last name' + '-' + '파일명' 순으로 되어 있다고 가정
>
> 이 때 모든 파일을 전부 '파일명'만 남기려고 한다면??



## 파일 예시

![image-20210408103451029](Rename_Files.assets/image-20210408103451029.png)



## 코드

```python
import os

print('경로를 입력하세요. (.: 현재폴더, \\: 폴더 들어가기) ex [.\\\\test]')
file_path = input('주소: ')
# file_path = '.\\test' # 테스트용 path
file_names = os.listdir(file_path)

# 이름 변경
for name in file_names:
    src = os.path.join(file_path, name) # 파일 이름을 주소와 연결
    dst = list(name.split('-'))[1] # 변경할 이름
    dst = os.path.join(file_path, dst) # 변경할 이름을 주소와 연결
    os.rename(src, dst) # src에서 dst로 이름 변경
```



## 결과

![image-20210408103545113](Rename_Files.assets/image-20210408103545113.png)



코드 출처: https://hogni.tistory.com/35
