# 🚀 Flow OS 설치 가이드
- Flow OS와 작업 대상(Data)을 명확히 분리하는 독립성 철학을 가집니다.
- 따라서 본 저장소를 홈 디렉토리 `~`에 hidden으로 직접 배치합니다.

설치가 완료되면 홈 디렉토리 `~` 아래에 다음과 같은 구조가 자동으로 생성됩니다.

```bash
~/
├── (OS의 기본 디렉토리)
├── .flow-os/      # OS 레이어
└── my/            # 데이터 레이어
    ├── .flow/     # 상태 레이어
    └── (purpose)  # 의도 레이어  (craft, forge, quests, ...)
```

사용자 입장에서 사실상 하나의 작업 공간만 보입니다. 

```bash
~/
├── (OS의 기본 디렉토리)
└── my
```

예를 들어 macOS에서 다음과 같이 `my`가 하나의 작업 공간으로 자연스럽게 인식됩니다.

```bash
~/
├── Applications
├── Desktop
├── Documents
├── Downloads
├── Library
├── Movies
├── Music
├── my
├── Pictures
└── Public
```

보다 자세한 내용은 [설계 철학](docs/philosophy.md)를 참고하세요.
------------------------------
## 1. 새 SSH 키를 생성하여 깃허브에 추가
- SSH 키는 '공개키(GitHub에 등록됨)'와 '개인키(로컬 기기에 저장됨)' 한 쌍으로 이루어져 있습니다.
- 보안상 개인키는 여러 기기에서 공유하지 않는 것이 원칙이므로, 기기마다 별도의 키를 등록합니다.

터미널(예: Linux 환경)에서 새로운 키를 생성하고 등록하는 방법은 다음과 같습니다.

1.터미널에서 새 SSH 키 생성
로컬 기기 (예: 맥북, 크롬북)의 리눅스 터미널을 열고 아래 명령어를 입력합니다.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

이메일 부분은 본인의 깃허브 계정 이메일로 변경하세요. 예:aimldl.tkim@gmail.com

명령어를 입력하면 파일을 저장할 위치나 비밀번호(passphrase)를 묻는데, 모두 엔터(Enter)를 쳐서 기본값으로 넘어갑니다.

```bash
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/thekim/.ssh/id_ed25519): 
Enter passphrase for "/home/thekim/.ssh/id_ed25519" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thekim/.ssh/id_ed25519
Your public key has been saved in /home/thekim/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:******************************************* [REDACTED_EMAIL]
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|                 |
|                 |
|                 |
|   [REDACTED]    |
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

2.SSH 에이전트 실행 및 키 추가

생성한 키를 시스템이 인식할 수 있도록 등록합니다. 터미널에 아래 두 줄을 차례대로 입력하세요.

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
예

```bash
$ eval "$(ssh-agent -s)"
Agent pid 18402
$ ssh-add ~/.ssh/id_ed25519
Identity added: /home/thekim/.ssh/id_ed25519 (aimldl.tkim@gmail.com)
$
```

3.생성된 공개키(Public Key) 확인 및 복사
이제 깃허브에 등록할 '공개키' 내용을 터미널 화면에 출력합니다.

```bash
cat ~/.ssh/id_ed25519.pub
```

예
```bash
$ cat ~/.ssh/id_ed25519.pub
ssh-ed25519 ******************************************* aimldl.tkim@gmail.com
```
ssh-ed25519 ... 로 시작해서 본인 이메일로 끝나는 긴 텍스트가 나오면, 텍스트 전체를 마우스로 드래그하여 복사합니다.


4.깃허브에 새 키 등록
브라우저에서 GitHub SSH settings 페이지로 이동합니다.

우측 상단의 New SSH key 초록색 버튼을 클릭합니다.

* Title에는 알아보기 쉽게 "크롬북 터미널의 퍼블릭키" 등으로 적습니다.
* Key type은 Authentication Key 그대로 둡니다.
* Key 입력란에 방금 복사한 텍스트를 붙여넣기 하고 Add SSH key 버튼을 누릅니다.

이렇게 설정하고 나면 깃허브 SSH keys 목록에 
- "크롬북 터미널의 퍼블릭키"
처럼 등록된 키가 나란히 등록됩니다.

5. 연결이 잘 되었는지 테스트해보기
- 등록한 기기 모두에서 안전하게 깃허브를 사용할 수 있게 됩니다.
- 터미널에서 깃허브와 SSH 연결이 완벽하게 되었는지 확인하는 방법은 간단합니다.


```bash
ssh -T git@github.com
```

성공 메세지 (연결 완료)
- `Hi (본인의 깃허브 아이디)! You've successfully authenticated, but GitHub does not provide shell access.`
- 정상적으로 연결이 되었다면 위와 같은 환영 메세지가 출력됩니다.
- 이제 에러 없이 마음껏 git clone을 진행하실 수 있습니다!

예
```bash
$ ssh -T git@github.com
Hi aimldl! You've successfully authenticated, but GitHub does not provide shell access.
$
```

만약 연결 확인 메세지가 나올 경우
  - `Are you sure you want to continue connecting (yes/no/[fingerprint])?`라는 메세지가 다시 나온다면,
  - `yes`를 입력하고 엔터를 누르시면 됩니다.

------------------------------
## 2. 저장소 클론 (Clone)

* 원격 저장소 주소: https://github.com/aimldl/flow-os

홈 디렉토리(~)에서 소스코드를 클론합니다.

🚨 중요: 디렉토리 이름 앞에 점(.)을 포함해서 숨김 폴더 형태로 생성합니다.

* ~/.flow-os (O)
* ~/flow-os (X)

### **SSH 방식 (권장)**

```bash
git clone git@github.com:aimldl/flow-os.git ~/.flow-os
```

예
```bash
$ git clone git@github.com:aimldl/flow-os.git ~/.flow-os
'/home/thekim/.flow-os'에 복제합니다...
remote: Enumerating objects: 359, done.
remote: Counting objects: 100% (153/153), done.
remote: Compressing objects: 100% (121/121), done.
remote: Total 359 (delta 72), reused 81 (delta 31), pack-reused 206 (from 1)
오브젝트를 받는 중: 100% (359/359), 149.55 KiB | 365.00 KiB/s, 완료.
델타를 알아내는 중: 100% (167/167), 완료.
$
```

💡 만약 이미 `~/flow-os`로 잘못 클론했다면?
아래 명령어를 통해 디렉토리 이름을 간단히 변경할 수 있습니다.

```bash
mv ~/flow-os ~/.flow-os
```
또는 홈 디렉토리 이동 후 변경
```bash
cd ~ && mv flow-os .flow-os
```

> 💡 Flow OS 관점
>
> SSH 설정은 단순한 인증 방식 변경이 아니라, “작업 흐름을 끊지 않기 위한 환경 설계”입니다

자세한 내용은 [부록. GitHub SSH 설정 가이드]() 을 참고하세요.

## 3. 시스템 셋업

클론이 완료되면 해당 디렉토리로 이동하여 설치 스크립트를 실행합니다.

**1. 설치 디렉토리로 이동**
```bash
cd ~/.flow-os
```
**2. 스크립트 실행 권한 부여 및 설치 진행**
```bash
chmod +x install.sh
./install.sh
```

예
```bash
~$ cd .flow-os/
~/.flow-os$ ls
CHANGELOG.md  INSTALL.md    README.ja.md  TODO.md  docs        shell
CLAUDE.md     README.en.md  README.md     bin      install.sh
~/.flow-os$ chmod +x install.sh
~/.flow-os$ ./install.sh 
🚀 Installing Flow OS...
  ✅ [OS]    .zshrc updated
  ✅ [OS]    .bashrc updated
  ✅ [data]  ~/my created
  ✅ [state] ~/my/.flow created
  ✅ [intent] purpose dirs ready (craft forge quests temp vault)
  ✅ [state] memo.md created
  ✅ [state] next.md created
  ✅ [state] resume.md created
  ✅ [state] todo.md created
  ✅ [env]   bin/ scripts executable

✅ Done. Restart terminal or run: exec zsh  (zsh)
                              or run: exec bash (bash)
~/.flow-os$ 

```

설치 완료 후 터미널을 재시작하거나 
- zshell은 `exec zsh`를 
- bash는 `exec bash`를
실행합니다.

> 💡 환경 호환성 안내
> 설치 스크립트는 Linux의 Bash 환경과 macOS의 Zsh 환경을 자동 감지하고 지원하도록 설계되었습니다.
>
> 참고: macOS Catalina(10.15) 버전부터 터미널의 기본 쉘이 Bash에서 Zsh로 변경됐습니다.

## 4. 첫 실행 시 

```
🔄 Syncing repos...
   Flow OS
🎯 목표: 기억하지 않고, 망설이지 않고, 바로 실행하는 상태 만들기
👉 목적: 기억 복원, 현재 상태 즉시 인지, 작업 진입 마찰 제거
🔄 흐름: 시작 → 복귀 → 유지 → 종료

   터미널 → todo → go → 작업1 → 작업2 → 작업3 → clean → quit
                         (🧠 → memo → todo)
                         💭 잡생각→흘려보냄

✨ my            의도  인지 상태
   ├── craft     유지  이미 익숙한 기술 유지
   ├── forge     학습  배우고 성장
   ├── quests    실행  실행 단위 작업
   ├── temp      임시  임시 작업/파일
   └── vault     보호  비공개 데이터 (private)

📝 TODO

🚀 Run 'go' to resume at 
~ $
```

위처럼 `resume at` 다음이 비어있습니다. 

`todo` 명령어를 실행해서 입력합니다.

```bash
~ $ todo
```

그만 두기 위해 `q` 명령어를 실행해서
```bash
~ $ q
📦 sync repos
✅ resume location saved: ~
👀 resume.md를 열어 검수합니다...
✅ RESUME 검수 후 종료하려면 Enter (취소: Ctrl+C): 
👋 Closing session...
~ $
```

터미널을 재시작합니다.

```bash
✅ Sync skipped (sync'd 17 min ago)
   Flow OS
🎯 목표: 기억하지 않고, 망설이지 않고, 바로 실행하는 상태 만들기
👉 목적: 기억 복원, 현재 상태 즉시 인지, 작업 진입 마찰 제거
🔄 흐름: 시작 → 복귀 → 유지 → 종료

   터미널 → todo → go → 작업1 → 작업2 → 작업3 → clean → quit
                         (🧠 → memo → todo)
                         💭 잡생각→흘려보냄

✨ my            의도  인지 상태
   ├── craft     유지  이미 익숙한 기술 유지
   ├── forge     학습  배우고 성장
   ├── quests    실행  실행 단위 작업
   ├── temp      임시  임시 작업/파일
   └── vault     보호  비공개 데이터 (private)

📝 TODO
[] 두 개의 다른 기기에서 동작 여부 테스트

🚀 Run 'go' to resume at ~
~ $

```
위처럼 `~` 위치가 저장되었습니다.

그리고 `TODO` 하단에 입력한 해야할 일을 볼 수 있습니다.

`my` 명령어 테스트
```bash
  ...
🚀 Run 'go' to resume at ~
~ $
```

`my`를 입력해서 `~/my` 디렉토리로 이동한 뒤, `t` (`tree` 명령어의 alias)를 실행하면
```bash
~ $ my
my $ t
.
├── craft
├── forge
├── quests
├── temp
└── vault

6 directories, 0 files
my $
```

의도 레이어의 폴더들이 생성됐음을 알 수 있습니다.

`~/my/.flow` 디렉토리는 `la` 명령어로 확인할 수 있습니다.
```bash
my $ la
.flow  craft  forge  quests  temp  vault
my $
```

## 5. 디렉토리 구조 확인
설치가 완료되면 홈 디렉토리 `~` 아래에 다음과 같은 구조가 자동으로 생성됩니다.

```bash
~/
├── .flow-os/              # OS 레이어 (이 저장소)
└── my/                    # 데이터 레이어
    ├── .flow/             # 상태 레이어
    │   ├── memo.md
    │   ├── next.md
    │   ├── resume.md
    │   ├── todo.md
    │   └── v-*.md         # 월별 아카이브 (v는 완료 상태를 의미함)
    ├── craft/             # 의도 레이어
    ├── forge/
    ├── quests/
    ├── temp/
    └── vault/
```

## 6. (레이어 기준) 설치 과정

설치 스크립트는 **Flow OS의 추상화 계층 구조를** 

```
~/
├── (OS의 기본 디렉토리)
├── .flow-os/      # OS 레이어
└── my/            # 데이터 레이어
    ├── .flow/     # 상태 레이어
    └── (purpose)  # 의도 레이어 (craft, forge, quests, ...)
```

아래 순서로 구성합니다.

| 단계 | 레이어 | 작업        | 설정 내용 요약                                                                                          |
| ---- | ------ | ----------- | --------------------------------------------------------------------------------------------------------- |
| 0    |        | 선택 도구   | Python3, Docker, Jupyter Lab, VS Code가 없으면 설치할지 확인 후 설치                                    |
| 1    | OS     | 연결/활성화 | `~/.zshconfig`, `~/.zshalias` 생성 후 `~/.zshrc`에 로더 추가                                            |
| 2    | 데이터 | 생성        | `~/my` 작업 공간 생성. `bin/git-target-repos.cfg`에 등록된 개인 저장소가 있으면 클론, 없으면 빈 디렉토리 생성 |
| 3    | 상태   | 생성        | `~/my/.flow`  생성                                                                                       |
| 4    | 의도   | 생성        | `craft`, `forge`, `quests`, `temp`, `vault` 생성                                                        |
| 5    | 상태   | 초기화      | `~/my/.flow`에 핵심 상태 파일 (`memo.md`, `next.md`, `resume.md`, `todo.md`) 생성 (2단계에서 클론됐다면 건너뜀) |
| 6    |        | 실행 환경   | `bin/` 스크립트에 실행 권한 부여                                                                         |

**🧠 왜 이런 순서를 따르는가?**

Flow OS는 **상태를 기반으로 행동하는 시스템** 이므로 설치 과정도 이와 같은 흐름을 따릅니다.

> 환경 연결 → 작업 공간 생성 → 상태 구조 정의 → 상태 초기화



## 📎 부록. Lite Mode: SSH 없이 사용하기

SSH 설정 없이도 Flow OS는 정상적으로 사용할 수 있지만 일부 Git 기반 자동화 기능은 제한됩니다.

> - **Lite Mode (HTTPS)** → 빠른 시작, 최소 설정
>
> - **SSH Mode** → 지속적 작업, 흐름 유지
>
>   👉 그래서 Flow OS에서는 SSH 방식을 권장합니다.

### 왜 SSH 방식을 권장하는가?

Flow OS는 GitHub와의 작업을 **흐름(flow)을 유지하는 방식**으로 설계되어 있습니다.

SSH가 아닌 HTTPS 방식으로도 저장소를 클론할 수 있습니다.

```bash
git clone https://github.com/aimldl/flow-os.git ~/.flow-os
```

> **⚠️ 제한 사항**
>
> HTTPS 방식으로 설치와 기본적인 사용에는 문제가 없지만, 다음과 같은 불편이 발생합니다.
>
> - `git push`, `git pull` 시 인증 요구 (ID / 토큰 입력)
> - 자동화된 워크플로우에서 흐름이 끊김

> **💡 Flow OS 관점**
>
> Lite Mode는 “빠른 시작”을 위한 선택이며,
>  SSH Mode는 “지속 가능한 흐름”을 위한 환경입니다.

### 언제 Lite Mode를 사용하는가?

- SSH 설정이 번거로운 경우
- 로컬 환경에서만 작업하는 경우
- GitHub 연동이 필수적이지 않은 경우

### 언제 SSH로 전환해야 하는가?

- Git 작업 빈도가 증가할 때
- 자동화 스크립트를 활용할 때
- 작업 흐름(flow)을 유지하고 싶을 때

## 📎 부록. GitHub SSH 설정 가이드

GitHub와 상호작용의 **흐름이 끊기 않도록 자동화**하기 위해 HTTPS 대신 **SSH 방식 사용을 권장**합니다.

**왜 SSH를 사용하는가?**

HTTPS 방식은 `git push`, `git pull` 할 때 ID/토큰 입력이 필요하므로 작업 흐름(flow)이 끊깁니다.

👉 SSH는 한 번 설정 후 인증 없이 계속 사용 가능합니다.

**SSH 설정: 세 단계**

1. SSH 키 생성
2. GitHub에 키 등록
3. 연결 테스트

------

### 1. SSH 키 생성

터미널에서 아래 명령어를 실행합니다:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com'
```

- 이메일은 GitHub 계정 이메일을 사용
- 파일 경로는 기본값(Enter) 그대로 사용
- 패스프레이즈는 선택 (비워도 됨)

### 2. SSH Agent 실행 및 키 등록

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 3. GitHub에 SSH 키 등록

**3-1. 공개키 복사**

```bash
cat ~/.ssh/id_ed25519.pub
```

출력된 내용을 전체 복사합니다.

**3-2. GitHub에 등록**

1. GitHub 접속
2. Settings → **SSH and GPG keys**
3. **New SSH key** 클릭
4. 복사한 키 붙여넣기

### 4. 연결 테스트

```bash
ssh -T git@github.com
```

정상이라면 다음과 같은 메시지가 출력됩니다:

```
Hi <username>! You've successfully authenticated...
```

------

### 5. SSH 방식으로 클론

아래 방식으로 클론할 수 있는지 확인합니다.

```bash
git clone git@github.com:aimldl/flow-os.git ~/.flow-os
```

