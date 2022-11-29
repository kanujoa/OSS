# 실습 1
## SA(staging area)와 WD(working directory)에서의 파일 차이 보기(diff)
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w
$ git init gdiff     # 07w 폴더 하부에 gdiff 저장소 생성
Initialized empty Git repository in C:/OSS/git/07w/gdiff/.git/

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w
$ cd gdiff     # gdiff 저장소로 이동

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ touch dtype.py     # touch 명령어를 이용해 dtype.py라는 빈 파일을 생성한다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git st     # 상태 보기
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        dtype.py     # dtype.py 파일은 현재 untracked file이고 working directory에 있다.

nothing added to commit but untracked files present (use "git add" to track)     # staging area에 올려 파일이 추적되게 하려면 git add를 하라는 의미이다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git sts     # 상태 한 줄로 보기
?? dtype.py     # ?? 로 보아 untracked file임을 알 수 있다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ cat dtype.py     # dtype.py는 touch 명령어로 빈 파일로 만들었으므로 내용이 없다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # 'git diff'는 SA와 WD의 파일 차이를 비교하게 해 주는 명령어이다. SA가 현재 비어있으므로 출력 결과가 없다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ code dtype.py     # VScode가 열리고 dtype.py 파일을 편집할 수 있다. print('python') 을 적고 저장시킨다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ cat dtype.py     # dtype.py 파일의 내용을 다시 살펴본다.
print('python')     # 새로 작성한 내용이 들어있는 것을 볼 수 있다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git sts
?? dtype.py     # 아직 dtype.py는 working directory에 있는 상태이다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git add .     # 현재 디렉토리에 있는 모든 파일 변경 사항들을 staging area에 올린다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git sts
A  dtype.py     # dtype.py 파일이 add되었음을 확인할 수 있다. 현재 dtype.py 파일은 staging area에 있다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git st     
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   dtype.py     # dtype.py는 커밋될 수 있는 파일이 되었다.
        
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # SA와 WD에서의 파일 내용 차이가 없으므로(똑같이 print('python') 내용이 들어 있음.) 결과는 출력되지 않는다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ code dtype.py     # 'print(list('python'))' 이라는 코드 한 줄을 더 추가한다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git st     
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   dtype.py     # 아직 커밋하지 않고 staging area에 위치해 있는 dtype.py 파일이다. 'print('python')' 까지의 내용이다.     

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   dtype.py      # staging area에 있는 파일을 수정한 상태이다. 'print(list('python'))' 코드가 추가된 것에 대한 것이다.
        
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git sts
AM dtype.py     # dtype.py가 add와 modified 상태에 있음을 확인할 수 있다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # git diff를 입력하면 WD와 SA의 차이점을 보여준다.
diff --git a/dtype.py b/dtype.py
index da5bf37..63c2376 100644
--- a/dtype.py     # a는 이전 버전(SA 파일)을 의미한다.
+++ b/dtype.py     # b는 이후 버전(WD 파일)을 의미한다.
@@ -1 +1,2 @@     # -는 이전 버전, +는 이후 버전, 그 뒤의 숫자는 라인을 의미한다.
 print('python')
+print(list('python'))     # WD에 코드 한 줄이 추가되었음을 확인할 수 있다.
\ No newline at end of file
```
- git diff : SA --> WD 파일 비교
- git diff --cached : GR(Git Repository) --> SA 파일 비교
- git diff HEAD : GR --> WD 파일 비교
<br/><br/>

# 실습 2
## diff 더 자세히 알아보기
- diff의 비교 기준(from) : 커밋된 파일
- diff의 비교의 대상(to) : stage(index) 영역 파일
- diff의 옵션 --cached의 의미 : 비교의 대상이 stage(index) 영역

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff --cached     # diff --cached를 통해 커밋을 한 번도 하지 않은 상태에서 커밋과 SA를 비교한 것이다. 
diff --git a/dtype.py b/dtype.py
new file mode 100644
index 0000000..da5bf37
--- /dev/null     # 커밋한 것은 없다.(저장소에 들어간 파일은 없다.)
@@ -0,0 +1 @@
+print('python')     # SA에 있는 코드 내용

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff HEAD     # 커밋을 한 번도 하지 않았으므로 HEAD를 작성하면 오류가 발생한다.
fatal: ambiguous argument 'HEAD': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
  
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git commit -m 'add string type'     # 첫 커밋을 한다. print('python') 코드에 대한 커밋이다.
[main (root-commit) c644ac4] add string type
 1 file changed, 1 insertion(+)
 create mode 100644 dtype.py

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff --cached     # GR과 WD는 차이가 없으므로 출력 결과가 없다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff HEAD     # 다시 diff HEAD를 입력하여 GR과 WD 사이의 차이를 살펴본다.
diff --git a/dtype.py b/dtype.py
index da5bf37..63c2376 100644
--- a/dtype.py     # 이전 버전 파일(GR 파일)을 의미
+++ b/dtype.py     # 이후 버전 파일(WD 파일)을 의미
@@ -1 +1,2 @@
 print('python')     
+print(list('python'))     # 이후 버전 파일(WD 파일)에만 있는 추가된 코드를 볼 수 있다.
\ No newline at end of file
```
<br/><br/>

## GR, SA, WD를 모두 다르게 해 보기
```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git sts     
 M dtype.py     # dtype.py는 앞서 코드 한 줄을 더 추가하면서 수정되었지만 add를 하지는 않은 상태이다.
 
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git add .     # 수정한 dtype.py를(코드 2줄) staging area로 올린다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git sts
M  dtype.py

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ code dtype.py     # dtype.py를 VScode에서 수정한다. print({i:ord(i) for i in 'python'})을 추가한다. 이제 WD에는 코드가 3줄이 존재하게 된다.
```
GR에는 코드 1줄, SA에는 코드 2줄, WD에는 코드 3줄이 있는 dtype.py가 존재한다는 것을 염두해 두고 diff 명령어를 사용해 보자.

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # git diff 명령어를 이용해 SA와 WD에서의 dtype.py 파일의 내용을 비교해 본다.
diff --git a/dtype.py b/dtype.py
index 63c2376..ad457fd 100644
--- a/dtype.py     # 이전 버전 파일(SA)
+++ b/dtype.py     # 이후 버전 파일(WD)
@@ -1,2 +1,3 @@    
 print('python')
-print(list('python'))     # 이전 버전 파일이 있는 SA에 print(list('python')) 코드가 있음을 알 수 있다.
\ No newline at end of file
+print(list('python'))     # 이후 버전 파일이 있는 WD에 옆 2개의 코드가 있음을 알 수 있다.
+print({i:ord(i) for i in 'python'})
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff --cached     # git diff --cached 명령어를 이용해 GR과 SA에서의 dtype.py 파일의 내용을 비교해 본다.
diff --git a/dtype.py b/dtype.py
index da5bf37..63c2376 100644
--- a/dtype.py     # 이전 버전 파일(GR)
+++ b/dtype.py     # 이후 버전 파일(SA)
@@ -1 +1,2 @@
 print('python')     
+print(list('python'))     # SA에 옆의 한 줄의 코드가 더 있음을 알 수 있다.  
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff head     # git diff head 명령어를 이용해 GR과 WD에서의 dtype.py 파일 내용을 비교해 본다.
diff --git a/dtype.py b/dtype.py
index da5bf37..ad457fd 100644
--- a/dtype.py     # 이전 버전 파일(GR)
+++ b/dtype.py     # 이후 버전 파일(WD)
@@ -1 +1,3 @@
 print('python')
+print(list('python'))     # WD에 옆의 두 줄의 코드가 더 작성되어 있음을 확인할 수 있다.
+print({i:ord(i) for i in 'python'})
\ No newline at end of file
```
<br/><br/>

# 실습 3
## 커밋 총 2번 하고 git diff의 5개 버전을 모두 수행하기
- git diff head1 head2 : 커밋 이력 간, 즉 head1에서 head2의 파일 비교(from to를 잘 구분하는 것이 중요)
- HEAD : 가장 최근 커밋
- HEAD~ : 가장 최근 커밋 바로 이전 커밋
- HEAD^ : 가장 최근 커밋 바로 이전 커밋

![image](https://user-images.githubusercontent.com/99963066/197385906-12779c41-1e2c-4f43-8e79-37d4074f265e.png)
<br/><br/>

```bash
뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git commit -m 'add list type'     # 커밋을 함으로써 GR에 있는 dtype.py 내용 추가하기(GR과 SA에서의 dtype.py 파일 내용이 같아짐.)   
[main b4e4e2d] add list type
 1 file changed, 1 insertion(+)

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git log1     # 커밋 이력 확인
b4e4e2d (HEAD -> main) add list type     # 아까 한 커밋
c644ac4 add string type

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff head~ head     # GR에서 가장 최근 커밋 바로 이전 커밋(print('python'))으로부터 가장 최근 커밋(print(list('python')))이 어떤 부분이 다른지 알 수 있다.
diff --git a/dtype.py b/dtype.py
index da5bf37..63c2376 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1 +1,2 @@
 print('python')
+print(list('python'))     # 가장 최근 커밋에는 옆의 코드가 추가되어 있는 것이 다른 점이라는 것을 알 수 있다.
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff head head^     # 가장 최근 커밋(print(list('python')))으로부터 가장 최근 커밋 바로 이전 커밋(print('python'))이 어떤 부분이 다른지 알 수 있다.
diff --git a/dtype.py b/dtype.py
index 63c2376..da5bf37 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1,2 +1 @@
 print('python')
-print(list('python'))     # 가장 최근 커밋 바로 이전 커밋에는 옆의 코드가 추가되어 있지 않다는 것이 다른 점이라는 것을 알 수 있다.
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # git diff 명령으로 SA로부터 WD가 무엇이 다른지 살펴본다.
diff --git a/dtype.py b/dtype.py
index 63c2376..ad457fd 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1,2 +1,3 @@
 print('python')
-print(list('python'))     # 옆 코드 내용이 SA와 비교하여 WD에는 없다.
\ No newline at end of file
+print(list('python'))     # 옆의 2개 코드 내용이 SA와 비교하여 WD에 존재한다.
+print({i:ord(i) for i in 'python'})
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git add dtype.py     # print({i:ord(i) for i in 'python'}까지의 코드 내용이 들어 있는 dtype.py를 staging area에 올린다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # 위에서 git add를 함으로써 SA와 WD의 코드 내용은 3줄로 같아졌으므로 git diff 명령어를 입력하였을 때 출력값이 없다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ code dtype.py     # print({i for i in range(5)}) 코드를 추가한다.

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff     # SA를 기준으로 WD가 다른 부분이 무엇인지를 git diff를 통해 살펴본다.
diff --git a/dtype.py b/dtype.py
index ad457fd..a496c0c 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1,3 +1,4 @@
 print('python')
 print(list('python'))
-print({i:ord(i) for i in 'python'})     # 옆 코드의 내용이 WD에서는 빠지고
\ No newline at end of file
+print({i:ord(i) for i in 'python'})     # 옆 2줄의 코드 내용이 WD에 추가되어 있는 것을 볼 수 있다.
+print({i for i in range(5)})
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff --staged     # git diff --staged를 입력하면 commited와 staged의 비교가 가능하다.
diff --git a/dtype.py b/dtype.py
index 63c2376..ad457fd 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1,2 +1,3 @@
 print('python')
-print(list('python'))     # staging area에는 옆의 내용이 commited와 비교해서 없고
\ No newline at end of file
+print(list('python'))     # 옆의 2줄이 commited와 비교하여 존재한다.
+print({i:ord(i) for i in 'python'})
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff head     # git diff head는 HEAD가 가리키는 가장 최근 커밋과 WD 사이의 차이를 보여준다.
diff --git a/dtype.py b/dtype.py
index 63c2376..a496c0c 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1,2 +1,4 @@
 print('python')
-print(list('python'))     # WD는 GR과 비교하여 옆과 같은 코드가 없다.
\ No newline at end of file
+print(list('python'))     # WD는 GR과 비교하여 옆과 같은 3줄의 코드가 더 존재한다.
+print({i:ord(i) for i in 'python'})
+print({i for i in range(5)})
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff --cached head~     # GR 기준으로 SA가 어떤 차이가 있는지를 볼 수 있다. 그런데 head~를 적어주었으므로 GR 안에는 print('python') 한 줄만 있어야 한다.
diff --git a/dtype.py b/dtype.py     # 즉, 가장 최근 커밋에서 바로 이전 커밋까지를 GR에 있는 것으로 보는 것이다.
index da5bf37..ad457fd 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1 +1,3 @@
 print('python')
+print(list('python'))     # 옆의 두 줄의 코드가 GR에는 없고 SA에 있는 코드임을 확인할 수 있다.
+print({i:ord(i) for i in 'python'})
\ No newline at end of file

뚜비@DESKTOP-SKBKL14 MINGW64 /c/OSS/git/07w/gdiff (main)
$ git diff head~     # GR 기준으로 WD에서의 dtype.py가 어떻게 다른지를 비교할 수 있다.
diff --git a/dtype.py b/dtype.py
index da5bf37..a496c0c 100644
--- a/dtype.py
+++ b/dtype.py
@@ -1 +1,4 @@
 print('python')
+print(list('python'))
+print({i:ord(i) for i in 'python'})
+print({i for i in range(5)})
\ No newline at end of file
```
- 커밋 간의 파일 비교 : git diff 커밋아이디1 커밋아이디2
- 이전 커밋과 최종 커밋의 파일 비교 : git diff HEAD^ HEAD
<br/><br/>

# 실습 5
## 소스트리에서 저장소 gdiff 살펴보기
![image](https://user-images.githubusercontent.com/99963066/197390612-2e204328-a527-4987-946f-77087406fc01.png)
<br/>

![image](https://user-images.githubusercontent.com/99963066/197390633-30889e14-a704-48d4-911e-d34f1b558139.png)
