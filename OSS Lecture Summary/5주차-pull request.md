# 📌 깃허브 연동 및 원격 등록
## 로컬 저장소를 원격 저장소에 연결하기
새로운 로컬 저장소를 생성하고 원격 저장소를 연결하여 보자.

우선 새 로컬 저장소를 생성한 후 README.md 파일을 만들어 추적 등록, 커밋한다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w
$ git init
Initialized empty Git repository in C:/OSS/git/05w/.git/

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ echo "# gitstudy05" >> README.md

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git add README.md

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git commit -m "first commit"
[main (root-commit) aa746c8] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```
<br/>

이제 원격 저장소와 연결해 보겠다.  
원격 저장소와 연결하려면 다음과 같이 **add 옵션**을 사용해야 한다.  

**$ git remote add 원격저장소별칭 원격저장소URL**

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git remote add origin https://github.com/kanujoa/gitstudy05_test.git     # 자신의 서버 주소를 입력한다.(origin을 별칭으로 설정함.)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git remote -v     # 원격 저장소 목록 확인
origin   https://github.com/kanujoa/gitstudy05_test.git (fetch)
origin   https://github.com/kanujoa/gitstudy05_test.git (push)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git remote show origin     # 'git remote show 원격저장소별칭'은 원격 저장소의 상세한 정보를 출력시켜준다.
* remote origin
  Fetch URL: https://github.com/kanujoa/gitstudy05_test.git
  Push  URL: https://github.com/kanujoa/gitstudy05_test.git
  HEAD branch: main
  Remote branch:
    main new (next fetch will store in remotes/origin)
  Local ref configured for 'git push':
    main pushes to main (local out of date)
```
<br/><br/>


# 📌 서버 전송
## push: 서버에 전송
✔️ **push**: 원격 저장소로 커밋된 파일들을 업로드하는 동작이다.  

▶️ 자신의 로컬 저장소를 '백업'하는 용도로 원격 저장소를 사용 가능하다.
<br/><br/>

git remote -v 명령어를 이용해 업로드가 가능한 원격 저장소가 등록되어 있는 것을 확인한다.   
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git remote -v
origin  https://github.com/kanujoa/gitstudy05_test.git (fetch)
origin  https://github.com/kanujoa/gitstudy05_test.git (push)
```
<br/>

push 명령어를 사용해 현재 main 브랜치를 origin 서버로 전송한다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git push origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 223 bytes | 55.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/kanujoa/gitstudy05_test.git
 * [new branch]       main -> main
```
<br/>

업로드 결과를 서버 저장소에서 확인한다. 다음과 같이 gitstudy05.md가 업데이트 되어 있는 것을 확인 가능하다.
<br/><br/><br/>


# 자동으로 내려받기
## clone: 복제
✔️ **clone**: 기존 저장소를 이용하여 새로운 저장소를 생성하는 방법 중 하나

▶️ clone 명령어를 사용하면 초기화 init 명령어 외에 원격 서버 접속에 필요한 추가 설정을 자동으로 수행하고 서버의 연결 설정을 마친 후 서버 안에 있는 모든 커밋된 코드 이력들을 
   한 번에 내려받는다.
<br/><br/>

새로운 폴더를 만들어 아까 gitstudy05_test 저장소를 clone해 보겠다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w_clone
$ git clone https://github.com/kanujoa/gitstudy05_test.git     # clone 명령어와 저장소 주소를 같이 적어주면 된다.
Cloning into 'gitstudy05_test'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```
<br/><br/>

## pull: 서버에서 내려받기
✔️ **pull**: 원격 저장소의 갱신된 내용을 추가로 내려받기 위하여 사용하는 명령어이다.

▶️ pull 명령어를 주기적으로 사용하면 최신 커밋 정보로 로컬 저장소를 유지할 수 있다.
<br/><br/>

실습을 위해 원본 로컬 저장소로 이동한다.
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w_clone
$ cd ..

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git
$ cd 05w

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ code server.htm     # server.htm이라는 파일을 만들어 VSCode로 편집하기
```
<br/>

다음과 같이 코드 내용을 작성한다.
![image](https://user-images.githubusercontent.com/99963066/204382871-2136ac1a-b690-4414-a8c6-e6dd153a2dac.png)
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git add server.htm     # server.htm을 SA에 올리기

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git commit -m "good day"     # 커밋하기
[main 8fd17da] good day
 1 file changed, 2 insertions(+)
 create mode 100644 server.htm

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git push origin main      # 서버로 push 하기(이로써 gitstudy05_test에 server.htm 파일이 추가된다.)
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 340 bytes | 113.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/kanujoa/gitstudy05_test.git
   aa746c8..8fd17da  main -> main
```
<br/>

다시 복제 저장소로 가보자.  
복제 저장소에는 아직 first commit 이 커밋 하나만 있는 것을 확인할 수 있다.  
이번에는 원격 저장소에서 갱신된 새 커밋 정보를 가지고 온다.
<br/>

**'git pull'** 명령어를 사용하면 원격 저장소에 갱신된 로컬 저장소의 커밋 정보와 비교하여 갱신한다.  
다시 복제된 저장소에서 log 명령어를 실행하면 first commit, good day 이렇게 2개의 커밋이 뜨는 것을 확인할 수 있다.
<br/><br/><br/>


# 📌 수동으로 내려받기
## fetch
✔️ **fetch(페치)**: 원격 저장소에서 코드를 수동으로 내려받는 작업을 한다. 다음과 같은 코드를 작성하여 실행할 수 있다.  
▶️ **git fetch 원격저장소URL**
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ code server.htm     # VSCODE를 켜서 아래 사진과 같이 코드를 수정한다.
```
<br/>

![image](https://user-images.githubusercontent.com/99963066/204474732-b4404e52-b721-462f-a7dd-54be752e8693.png)
<br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git commit -am "look sky"     # 수정된 파일을 스테이지 영역에 등록과 동시에 커밋한다.
[main 420e411] look sky
 1 file changed, 2 insertions(+), 1 deletion(-)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ git push origin main     # push 명령어를 이용해 커밋된 수정 내용을 원격 저장소로 전송한다.
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 366 bytes | 91.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/kanujoa/gitstudy05_test.git
   8fd17da..420e411  main -> main
```
<br/>

다시
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w (main)
$ cd ..     # 폴더를 이동한다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git
$ cd 05w_clone     # 05w_clone 저장소(복제 폴더)로 이동한다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w_clone (main)
$ git fetch     # fetch 명령어를 이용해 커밋을 내려받는다.
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 346 bytes | 16.00 KiB/s, done.
From https://github.com/kanujoa/gitstudy05_test
   8fd17da..420e411  main       -> origin/main
```
<br/>

페치 후 원격 저장소의 커밋 내용을 살펴보면 커밋 로그가 나오는 것을 확인할 수 있다.  
pull 명령어와 다르게 fetch 명령어를 실행한 후에는 커밋이 추가된 것을 확인할 수 있다.
<br/><br/><br/>


# 📌 merge 명령어로 수동 병합
페치는 데이터를 내려받기만 할 뿐 자동 병합하지 않는다. 내려받은 커밋을 로컬 저장소에 적용하려면 병합 명령을 실행해야 하는데, 이때 merge 명령어를 사용한다.  
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w_clone (main)
$ git merge origin/main     # 임시 원격 브랜치 병합

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/05w_clone (main)
$ git log     # 로그를 확인하여 다시 로컬 저장소의 로그 기록을 확인한다.
commit 420e411b83781c643ee7bd85229136c790821aef (HEAD -> main, origin/main)
Author: yubeenso <youbin0105@gmail.com>
Date:   Tue Nov 29 17:19:13 2022 +0900

    look sky     

commit 8fd17da66b985e1bb3d3e5e4aa899a8b73ab8945
Author: yubeenso <youbin0105@gmail.com>
Date:   Tue Nov 29 06:18:34 2022 +0900

    good day

commit aa746c8ad10d909137794a55da2495905b0f8aa4
Author: yubeenso <youbin0105@gmail.com>
Date:   Tue Nov 29 00:23:30 2022 +0900

    first commit
```
