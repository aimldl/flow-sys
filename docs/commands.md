# 명령어 레퍼런스

## 핵심 명령어

| 명령어 | 별칭 | 설명 |
|--------|------|------|
| `go` | `fgo` | resume.md의 첫 번째 경로로 이동하여 작업 재개 |
| `fmemo <텍스트>` | | '잡'생각을 memo.md에 기록 (멀티라인 입력 지원) |
| `q` | `quit`, `fquit` | 표준 종료: git push → resume 저장 → 검수 → 터미널 닫기 |
| `qq` | `fqq`, `quickquit` | 빠른 종료: git push → resume 저장 → 즉시 터미널 닫기 |
| `cleanmemo` | `fcleanmemo`, `clean-memo` | memo.md → `v-memo-YYYY-MM.md` prepend, 초기화 |
| `cleantodo` | `fcleantodo`, `clean-todo` | `[v]` → `v-todo-YYYY-MM.md`, `[->]` → `next.md`, `[]` 유지, 빈 슬롯 3개 채움 |
| `cleannext` | `fcleannext`, `clean-next` | next.md → `v-next-YYYY-MM.md` prepend, 초기화 |

---

## memo 출력 옵션

memo는 필요에 따라 작업 context를 함께 기록할 수 있습니다.

```bash
FLOW_MEMO_VIEW=off | layer | full_path
```

- off (기본): context 없이 메모만 기록
- layer: Flow OS 구조 포함 (예: my/quests, .flow, .flow-os)
- full_path: 전체 경로 포함

예:

```
12:10  my/quests    | API 수정
12:10  .flow        | 상태 정리
12:10  ~/my/...     | 디버깅 로그
```

## 파일 편집

| 명령어     | 별칭                    | 대상 파일                                   |
| ---------- | ----------------------- | ------------------------------------------- |
| `todo`     | `etodo`, `edittodo`     | `~/my/.flow/todo.md`                        |
| `resume`   | `eresume`, `editresume` | `~/my/.flow/resume.md`                      |
| `memo`     | `ememo`, `editmemo`     | `~/my/.flow/memo.md`                        |
| `next`     | `enext`, `editnext`     | `~/my/.flow/next.md`                        |
| `fcmd`     |                         | `~/.flow-os/docs/commands.md`               |
| `e <파일>` | `edit`                  | 지정한 파일 (macOS: TextEdit / Linux: nano) |

## 디렉토리 이동

| 명령어   | 별칭                                                                            | 대상          |
| -------- | ------------------------------------------------------------------------------- | ------------- |
| `fos`    | `flowos`                                                                         | `~/.flow-os`  |
| `flow`   | `f`, `myflow`                                                                    | `~/my/.flow`  |
| `docs`   | `doc`                                                                            | `~/.flow-os/docs` |
| `my`     |                                                                                   | `~/my`        |
| `craft`  | `myc`, `mycraft`, `my-craft`, `mcraft`                                           | `~/my/craft`  |
| `forge`  | `myf`, `myforge`, `my-forge`, `mforge`                                           | `~/my/forge`  |
| `quests` | `quest`, `myq`, `myquests`, `myquest`, `my-quests`, `my-quest`, `mquests`, `mquest` | `~/my/quests` |
| `temp`   | `tmp`, `myt`, `mytmp`, `mytemp`, `my-temp`                                       | `~/my/temp`   |
| `vault`  | `myv`, `myvault`, `my-vault`, `mvault`                                           | `~/my/vault`  |

## Git 자동화

| 명령어    | 별칭 | 설명                                     |
| --------- | ---- | ---------------------------------------- |
| `gpull`   |      | 설정된 모든 repo를 pull                  |
| `gpush`   |      | 설정된 모든 repo를 auto-commit & push    |
| `gstatus` |      | 설정된 모든 repo의 dirty/clean 상태 표시 |
| `ginit`   |      | git-target-repos.cfg의 repo들을 clone    |
| `gsetup`  |      | repo 디렉토리 생성 및 bin 실행 권한 부여 |

대상 저장소는 `~/.flow-os/bin/git-target-repos.cfg`에서 관리합니다. 자세한 내용은 [git-sync.md](git-sync.md)를 참고하세요.

## Shell 설정 편집

| 명령어 | 별칭                | 대상 파일                        |
| ------ | ------------------- | -------------------------------- |
| `fa`   | `falias`            | `~/.flow-os/shell/zsh/alias`     |
| `fc`   | `fconf`, `fconfig`  | `~/.flow-os/shell/zsh/config`    |
| `ff`   | `ffn`, `ffunctions` | `~/.flow-os/shell/zsh/functions` |
| `fr`   | `frc`               | `~/.flow-os/shell/zsh/rc`        |
| `za`   | `zalias`            | `~/.zshalias`                    |
| `zc`   | `zconfig`           | `~/.zshconfig`                   |
| `zr`   | `zrc`               | `~/.zshrc`                       |
| `z`    |                     | zsh 재시작 (`exec zsh`)          |
| `s`    |                     | `.zshrc` 재로드 (`source`)       |

## 시스템 유틸리티

| 명령어                   | 원래 명령                               |
| ------------------------ | --------------------------------------- |
| `c` / `cc`               | `cd` / `cd ..`                          |
| `cl`                     | `clear`                                 |
| `cx`                     | `chmod +x`                              |
| `fn` / `fd`              | `find . -name` / `find . -type d -name` |
| `g`                      | `egrep -n`                              |
| `l` / `ll` / `la`        | `ls -GF` / `ls -alF` / `ls -A`          |
| `last`                   | 최근 수정된 파일 20개                   |
| `p`                      | 현재 경로 (HOME 축약)                   |
| `t` / `td` / `t1` / `t2` | `tree` 변형                             |
| `search`                 | `grep -Rin`                             |

## Python 가상환경

| 명령어 | 별칭 | 설명                              |
| ------ | ---- | --------------------------------- |
| `vc`   |      | `python -m venv .venv`            |
| `va`   |      | `source .venv/bin/activate`       |
| `vd`   |      | `deactivate`                      |
