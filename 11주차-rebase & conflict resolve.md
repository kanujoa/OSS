# 📌 충돌

## 충돌이 생기는 상황
✔️ 같은 위치의 코드를 동시에 다르게 수정할 때 충돌이 생김. (대부분의 충돌 원인)
▶️ 같은 위치를 동시에 수정하면 두 수정 중 어떤 것이 맞는지 깃에서 자동으로 알 수 없기 때문에 이때 깃은 충돌 오류라고 알려 주고 개발자에게 직접 수정하여 충돌을 해결하라고 요청한다.
<br/><br/><br/>

# 📌 리베이스
## 베이스(base)
✔️ **베이스**: 새로운 브랜치가 파생되는 커밋을 의미한다. 아래 그림에서는 커밋 2가 base에 해당된다.   

![image](https://user-images.githubusercontent.com/99963066/204229237-237b4f9c-f191-4ed5-8e25-3277d6c27a6a.png)
<br/><br/>

## 베이스 변경
✔️ **리베이스**: 파생된 브랜치의 기준이 되는 베이스 커밋을 변경하는 것  
<br/>

### 브랜치의 베이스를 변경하는 이유
▶️ 복잡한 브랜치의 수많은 경로는 코드의 진행 상황을 한눈에 파악하기 어렵게 하므로 커밋의 진행 모습을 단순화하기 위해 사용  
<br/>

리베이스를 사용하면 다음 그림과 같이 브랜치 A의 공통된 조상인 커밋2를 master 브랜치의 마지막 커밋6으로 변경할 수 있게 된다.   
그리고 모든 브랜치의 커밋들을 리베이스된 커밋6 이후로 재정렬한다.

![image](https://user-images.githubusercontent.com/99963066/204231296-021dd82d-4cf5-4815-929d-3d8905be273f.png)
<br/><br/>

## 리베이스 vs 병합
✔️ 병합
- 공통 조상 커밋을 찾으면 서로 다르게 커밋이 진행된 두 브랜치를 3-way 방식으로 병합할 수 있다.  
- 병합하는 두 브랜치는 순차적으로 커밋을 비교하면서 마지막 최종 커밋을 생성한다.

![image](https://user-images.githubusercontent.com/99963066/204232606-8b585ef9-f738-4c08-949c-bc9d6e589dc5.png)
<br/>

✔️ **리베이스**
- 두 브랜치를 서로 비교하지 않고 순차적으로 커밋 병합을 시도한다.  
- 리베이스를 하면 먼저 공통 조상 커밋을 찾고, 베이스 커밋을 변경하여 두 브랜치의 커밋 위치를 바꿈. 그리고 파생된 브랜치의 diff를 임시 공간에 잠시 보관한다.  
- 기존 베이스 커밋2 에서 커밋6으로 베이스 기준점을 변경한다. 변경하는 기준 브랜치의 마지막 커밋에서 차례로 임시 공간에 저장한 diff를 하나씩 적용한다.
- 새로운 베이스 기준점을 기반으로 한 브랜치에서 커밋3 -> 커밋4를 커밋 6에서 연장하여 수정 재배치한다.

![image](https://user-images.githubusercontent.com/99963066/204234774-fad9727f-30d4-4391-9a5b-e4355d158ccc.png)
<br/>

▶️ 3-way 병합은 병합 커밋이 있지만 리베이스는 병합 커밋이 없다.
▶️ 브랜치의 마지막을 가리키는 커밋 위치가 다르다. 브랜치 A는 커밋 4를 가리키지만 main 브랜치는 아직 커밋 6을 가리킨다.
<br/><br/><br/>


# 실습 1
1. 저장소 pbase 생성 후 브랜치 main에서 1개의 커밋(111 한 줄로 파일 h.txt 생성) 먼저 생성
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주
$ git init pbase     # pbase 저장소 생성
Initialized empty Git repository in C:/OSS/git/11주/pbase/.git/

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주
$ cd pbase     # pbase 저장소로 이동

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ echo 111 > h.txt     # 111 내용이 담긴 h.txt 파일 새로 생성

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git add .     # 모든 내용을 SA에 올린다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git commit -m 1     # 커밋하기
[main (root-commit) fc5a616] 1
 1 file changed, 1 insertion(+)
 create mode 100644 h.txt

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git log --oneline --graph --all
* fc5a616 (HEAD -> main) 1
```
<br/>

2. 브랜치 topic 생성 후 이동해서 파일 h.py 수정(두 줄 추가) 후 커밋
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git checkout -b topic     # topic 브랜치 생성 후 이동
Switched to a new branch 'topic'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ echo 222 >> h.txt     # h.txt에 222 내용 추가

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ echo AAA >> h.txt     # 연속으로 h.txt에 AAA 내용 추가

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ cat h.txt     # h.txt 내용 확인
111
222
AAA

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git commit -am A     # 커밋하기
[topic 89572ff] A
 1 file changed, 2 insertions(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git log --oneline --graph --all
* 89572ff (HEAD -> topic) A
* fc5a616 (main) 1
```
<br/>

3. 다시 브랜치 main에서 파일 h.txt 수정(222 내용 추가) 후 커밋
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git checkout -     # 이전 브랜치로 이동
Switched to branch 'main'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ echo 222 >> h.txt     # h.txt에 내용 추가

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git commit -am 2     # 커밋하기
[main 12ad34e] 2
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git log --oneline --graph --all
* 12ad34e (HEAD -> main) 2
| * 89572ff (topic) A
|/
* fc5a616 1
```
