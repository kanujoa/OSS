# 📌 병합
**- 병합(합치기): 분리된 브랜치를 한 브랜치로 합치는 작업**

✔️ 수동 병합: 양쪽 파일을 일일이 비교하며 바뀐 점을 찾아서 적용해야 한다.
<br/>

✔️ **깃으로 자동 병합**: 원본을 기준으로 두 파일의 변경 이력을 비교한다. 
- 깃의 병합은 브랜치를 기준으로 한다. 분리된 각각의 브랜치에서 수정된 사항을 하나의 브랜치로 병합하므로 병합하고자 하는 브랜치는 같은 로컬 저장소에 있어야 한다.
- 깃이 모든 코드의 병합을 완벽하게 처리할 수는 없는데 '충돌' 상황에서는 코드의 병합을 자동으로 반영하지 못한다.
<br/>

✔️ **Fast-Forward 병합**   
- 빨리 감기 병합은 일반적으로 혼자 개발할 때 사용!
- 혼자 개발 시에는 브랜치가 생성된 커밋에 따라 순차적으로 분기되고 코드 수정도 순차적으로 할 때가 많음. 이러한 순차적 커밋에 맞추어 병합을 처리하는 방법이 Fast-Forward
  병합임!
<br/>

✔️ **3-way 병합**
- 여러 개발자와 협업으로 작업하는 경우 대부분 3-way 병합을 사용한다.
- 두 브랜치에서 공통 조상 커밋을 자동으로 찾아 주며 공통 조상 커밋을 기준으로 브랜치를 병합한다. 
- 병합을 성공적으로 완료한 후에는 새로운 커밋인 병합 커밋을 추가로 하나 생성한다.
<br/><br/><br/>


# 실습 1
1. 저장소 mgct를 생성하여 브랜치 main에서 1개의 커밋(aaa 한 줄로 파일 h.py 생성) 먼저 생성  
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주
$ git init mgct     # 저장소 mgct 생성
Initialized empty Git repository in C:/OSS/git/10주/mgct/.git/

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주
$ cd mgct     # 저장소 mgct로 이동

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ echo aaa > h.py     

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git add .

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git commit -m 'Add aaa'     # 커밋
[main (root-commit) 13395b8] Add aaa
 1 file changed, 1 insertion(+)
 create mode 100644 h.py
```
<br/>

2. 브랜치 devp 생성 후 이동해서 파일 h.py 수정(111 추가) 후 커밋
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git checkout -b devp     # 브랜치 devp를 생성 후 이동
Switched to a new branch 'devp'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (devp)     # 브랜치 devp로 이동함.
$ echo 111 >> h.py     # 파일 h.py에 내용 추가

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (devp)
$ git commit -am 'Add 111'     # 커밋
[devp 33f9db7] Add 111
 1 file changed, 1 insertion(+)
```
<br/>

3. 다시 브랜치 main에서 파일 h.py 수정(bbb 추가) 후 커밋
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (devp)
$ git checkout main     # main 브랜치로 이동한다.
Switched to branch 'main'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ echo bbb >> h.py      # 파일 h.py에 내용 추가

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ cat h.py     # h.py의 파일 내용 확인
aaa
bbb

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all     # 모든 저장소에서의 커밋 이력 한 줄로 보기
* 33f9db7 (devp) Add 111
* 13395b8 (HEAD -> main) Add aaa

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git commit -am 'Add bbb'     # 커밋
[main a288fec] Add bbb
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all     
* a288fec (HEAD -> main) Add bbb     # main 브랜치에서 커밋을 진행함으로써 devp에서 진행한 커밋보다 앞서나가 있는 것을 확인
| * 33f9db7 (devp) Add 111
|/
* 13395b8 Add aaa
```
<br/><br/>


# 실습 2
1. main 브랜치에서 devp를 3-way 병합, 충돌 발생 확인
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git merge devp     # merge 명령어를 사용해 main 브랜치에서 devp를 병합하려고 함.
Auto-merging h.py
CONFLICT (content): Merge conflict in h.py
Automatic merge failed; fix conflicts and then commit the result.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main|MERGING)
$ cat h.py     # h.py 파일 내용을 확인해 보면 main 브랜치에서의 bbb라는 내용과 devp 브랜치에서의 111 내용에서
aaa            # 충돌이 발생한 것을 확인할 수 있다.
<<<<<<< HEAD
bbb
=======
111
>>>>>>> devp
```
<br/>

2. 병합 시 충돌한 내용인 111과 bbb를 모두 파일 내용으로 반영시킨다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main|MERGING)
$ code h.py     # VSCode를 열어 어떻게 병합할 건지를 결정한다.
```

![image](https://user-images.githubusercontent.com/99963066/204138502-9a6bf55e-1c92-4670-83af-fa1044c1d295.png)

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main|MERGING)
$ git commit -am '3-way merge'     # 3-way 병합을 하였으므로 병합 커밋 메시지를 작성하고 커밋해 준다.
[main 08f3d40] 3-way merge

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all     # 커밋 이력을 확인하여 병합이 제대로 되었는지 확인한다.
*   08f3d40 (HEAD -> main) 3-way merge
|\
| * 33f9db7 (devp) Add 111     # bbb와 111이 모두 반영이 되어 병합이 된 것을 확인할 수 있다.
* | a288fec Add bbb
|/
* 13395b8 Add aaa
```
<br/><br/>

# 실습 3
다시 main 브랜치에서 작업을 한다.

1. 브랜치 devp가 merged인지를 확인 후 브랜치 devp를 삭제
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git branch
  devp
* main     # 현재 브랜치는 main이다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git branch --merged      # 병합된 브랜치 확인
  devp     # devp 브랜치는 병합 상태이다.
* main     # 현재 브랜치는 main이다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git branch -d devp      # -d 옵션을 사용하여 devp 브랜치를 삭제한다.
Deleted branch devp (was 33f9db7).
```
<br/>

2. 새로운 브랜치 hotfix를 만들어 이동 후 파일 h.py 수정(222 내용 추가) 후 커밋
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git switch -c hotfix     # -c 옵션을 사용하여 hotfix 브랜치 생성 후 이동
Switched to a new branch 'hotfix'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (hotfix)
$ cat h.py     # hotfix 브랜치에서의 초기 h.py 파일 내용 확인
aaa
bbb
111

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (hotfix)
$ echo 222 >> h.py     # h.py에 내용 추가하기

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (hotfix)
$ git commit -am 'Add 222'     # 커밋 메시지 작성 후 커밋
[hotfix 8f79694] Add 222
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (hotfix)
$ git log --oneline --graph --all     # 커밋 이력 확인
* 8f79694 (HEAD -> hotfix) Add 222     # 새로 생성한 hotfix 브랜치에서 Add 222 커밋이 발생한 것을 확인할 수 있다.
*   08f3d40 (main) 3-way merge
|\
| * 33f9db7 Add 111
* | a288fec Add bbb
|/
* 13395b8 Add aaa
```
<br/>

3. 다시 main 브랜치에서 브랜치 hotfix를 빠른 ff로 병합한다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (hotfix)
$ git checkout main     # main 브랜치로 이동한다.
Switched to branch 'main'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all     # 커밋 이력을 본다.
* 8f79694 (hotfix) Add 222
*   08f3d40 (HEAD -> main) 3-way merge
|\
| * 33f9db7 Add 111
* | a288fec Add bbb
|/
* 13395b8 Add aaa

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git merge hotfix     # hotfix 브랜치를 병합한다. 여기에서는 Fast-Forward 병합이 된다.
Updating 08f3d40..8f79694
Fast-forward
 h.py | 1 +
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all      # 커밋 이력을 확인해 병합이 제대로 되었는지를 확인한다.
* 8f79694 (HEAD -> main, hotfix) Add 222     # 병합이 되어 main 브랜치와 hotfix 브랜치가 같은 위치에 있는 것을 확인할 수 있다.
*   08f3d40 3-way merge
|\
| * 33f9db7 Add 111
* | a288fec Add bbb
|/
* 13395b8 Add aaa

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ cat h.py
aaa
bbb
111
222     # main 브랜치에서도 222 내용이 추가되어 있는 것을 볼 수 있다.
```
<br/>

# 실습 4
1. main 브랜치에서 새로운 브랜치인 feature를 만들어 파일 h.py를 수정(333 내용 추가) 후 커밋한다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git checkout -b feature     # feature 브랜치 생성 후 이동한다.

Switched to a new branch 'feature'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (feature)
$ echo 333 >> h.py      # h.py에 내용을 추가한다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (feature)
$ git commit -am 'Add 333'     # 커밋을 진행한다.
[feature 7a40c7c] Add 333
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (feature)
$ git log --oneline --graph --all     # 커밋 이력을 확인한다.
* 7a40c7c (HEAD -> feature) Add 333     # feature 브랜치에서 Add 333 내용의 커밋이 이루어진 것을 확인할 수 있다.
* 8f79694 (main, hotfix) Add 222
*   08f3d40 3-way merge
|\
| * 33f9db7 Add 111
* | a288fec Add bbb
|/
* 13395b8 Add aaa

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (feature)
$ git branch      # 브랜치 목록 확인
* feature
  hotfix
  main

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (feature)
$ git branch --merged     # 병합된 브랜치를 확인
* feature
  hotfix
  main
```
<br/>

2. 다시 main 브랜치에서 병합된 브랜치와 병합되지 않은 브랜치를 확인
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (feature)
$ git checkout -     # main 브랜치로 이동한다.
Switched to branch 'main'

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git branch --no-merged     # main 브랜치에서 병합이 되지 않은 브랜치는 feature 브랜치임을 확인 가능
  feature
```
<br/>

3. main에서 브랜치 feature를 3-way 병합한다.
- 병합 시 옵션 **--no-ff**를 반드시 사용해야 한다.
- 병합 전에 반드시 VSCode에서 병합 커밋 메시지를 작성해야 한다.

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git merge feature --no-ff
hint: Waiting for your editor to close the file...
```

![image](https://user-images.githubusercontent.com/99963066/204141585-d0681280-3a41-4a3a-9db8-4a88c8df8799.png)

위와 같이 병합 커밋 메시지 작성 후 저장한 후 실행해야 한다.

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git merge feature --no-ff     
Merge made by the 'ort' strategy.
 h.py | 1 +
 1 file changed, 1 insertion(+)     # 커밋 메시지 작성이 완료 후 VSCode를 닫으면 옆과 같은 메시지가 뜬다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all     
*   dc9f828 (HEAD -> main) Merge branch 'feature' by 3-way merge     # 3-way 병합이 완료되었고 내가 작성한 병합 커밋 메시지도 뜨는 것을 볼 수 있다.
|\
| * 7a40c7c (feature) Add 333
|/
* 8f79694 (hotfix) Add 222
*   08f3d40 3-way merge
|\
| * 33f9db7 Add 111
* | a288fec Add bbb
|/
* 13395b8 Add aaa
```
<br/>

4. main 브랜치에서 merge된 브랜치와 merge되지 않은 브랜치를 확인 후 병합된 hotfix 브랜치를 삭제한다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git branch --merged     # 모든 브랜치가 병합되어 있음을 확인할 수 있다.
  feature
  hotfix
* main

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git branch -d hotfix     # hotfix 브랜치를 삭제하였다.
Deleted branch hotfix (was 8f79694).

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/10주/mgct (main)
$ git log --oneline --graph --all
*   dc9f828 (HEAD -> main) Merge branch 'feature' by 3-way merge
|\
| * 7a40c7c (feature) Add 333
|/
* 8f79694 Add 222
*   08f3d40 3-way merge      # hotfix 브랜치가 삭제된 것을 볼 수 있다.
|\
| * 33f9db7 Add 111
* | a288fec Add bbb
|/
* 13395b8 Add aaa
```
<br/><br/>

# 실습 5
소스트리를 열어서 브랜치 확인 후 병합된 브랜치인 feature를 삭제한다.

![image](https://user-images.githubusercontent.com/99963066/204142285-43d8b313-3e66-4c66-8cc3-68fe5ad22595.png)

최종 결과는 다음과 같다.
![image](https://user-images.githubusercontent.com/99963066/204142311-f659cd84-dfea-4265-ae1c-f7e3acc74645.png)
