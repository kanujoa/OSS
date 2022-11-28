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
<br/><br/>

# 실습 2
1. 브랜치 topic에서 main을 rebase 병합  
- 파일 내부를 수정하고 add, rebase, --continue 함으로써 충돌 해결하기  
- 메시지 작성하고 저장 후 닫으면 rebase 성공
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git rebase main     # topic 브랜치에서 main 브랜치를 rebase 명령어를 사용하여 rebase 병합시킴.
Auto-merging h.txt
CONFLICT (content): Merge conflict in h.txt     # 충돌 발생
error: could not apply 89572ff... A
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 89572ff... A

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic|REBASE 1/1)     # 충돌 표시
$ git rebase --abort     # --abort 옵션을 사용하여 리베이스를 취소할 수 있다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git rebase main     # 다시 리베이스 시도
Auto-merging h.txt
CONFLICT (content): Merge conflict in h.txt
error: could not apply 89572ff... A
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 89572ff... A

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic|REBASE 1/1)
$ code h.txt     # VSCode를 열어 h.txt의 코드를 아래와 같이 수정하여 충돌을 해결한다.
```
<br/>

![image](https://user-images.githubusercontent.com/99963066/204292997-46feb650-502a-469e-9a84-8e2c763c84bc.png)
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic|REBASE 1/1)
$ git add h.txt     # 수정된 h.txt를 SA에 올린다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic|REBASE 1/1)
$ git rebase --continue      # --continue 옵션을 사용하면 병합을 계속 진행할 수 있다.
hint: Waiting for your editor to close the file...
```
<br/>
 
 ![image](https://user-images.githubusercontent.com/99963066/204297075-82dcc452-22b3-4735-afed-b48cb75b3860.png)
<br/>

git rebase --continue 후에 위와 같이 커밋 메시지를 작성하고 저장해 주면 rebase 커밋이 완료된다.  
(리베이스를 이용하여 병합 시에는 충돌된 부분들을 한 단계씩 해결해 나가면서 병합이 가능하다.)
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git log --oneline --graph --all
* 15cc027 (HEAD -> topic) A rebase      # topic 브랜치에서 rebase가 이루어진 것이 커밋되어 있는 것을 확인할 수 있다.
* 12ad34e (main) 2
* fc5a616 1
```
<br/>

2. 다시 main 브랜치에서 fast-forward(빨리감기) 병합 후 브랜치 topic을 삭제한다.

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (topic)
$ git checkout -     # 이전 브랜치인 main 브랜치로 이동
Switched to branch 'main'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git merge topic     # main 브랜치에서 topic 브랜치를 병합함.
Updating 12ad34e..15cc027
Fast-forward
 h.txt | 1 +
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git log --oneline --graph --all
* 15cc027 (HEAD -> main, topic) A rebase     # main과 topic 브랜치 모두 가장 최근 커밋인 A rebase를 가리키고 있다.
* 12ad34e 2
* fc5a616 1

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git branch -d topic     # topic 브랜치 삭제
Deleted branch topic (was 15cc027).

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git log --oneline --graph --all     
* 15cc027 (HEAD -> main) A rebase     # topic 브랜치가 삭제되고 main 브랜치만 남음.
* 12ad34e 2
* fc5a616 1
```
<br/><br/>

# 실습 3
다시 main 브랜치에서 commit --amend로 마지막 커밋을 다음으로 수정한다.

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ code h.txt
```
<br/>

아래와 같은 코드를  
![image](https://user-images.githubusercontent.com/99963066/204304792-a87f7df4-6084-40e0-a0ee-91a81088a1c9.png)

아래와 같이 수정한다.  
![image](https://user-images.githubusercontent.com/99963066/204305579-a352531f-2e18-4589-a033-5712e1e61a98.png)
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git add h.txt     # 수정된 h.txt 파일을 SA에 올린다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git commit --amend     # commit 명령어와 --amend 옵션을 사용하여 VSCode를 사용하여 커밋 메시지를 수정한다.
[main 1f58690] 3
 Date: Mon Nov 28 18:11:26 2022 +0900
 1 file changed, 1 insertion(+)
```
<br/>

아래와 같이 수정한다.  
![image](https://user-images.githubusercontent.com/99963066/204306485-1598f174-2258-47f8-a42f-f7bb4d290d03.png)
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/11주/pbase (main)
$ git log --oneline --graph --all
* 1f58690 (HEAD -> main) 3     # 커밋 메시지가 'A rebase'에서 '3'으로 바뀐 것을 확인 가능
* 12ad34e 2
* fc5a616 1
```




