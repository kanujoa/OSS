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
