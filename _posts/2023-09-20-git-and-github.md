---
layout: post
title: Git & Github 톺아보기
---

톺아보기는 ___구석구석 꼼꼼히 살펴보기___라는 뜻이다. 본 뜻을 찾아보면, '톺다'라는 뜻은 _틈이 있는 곳마다 모조리 더듬어 뒤지면서 찾다_라는 뜻이고, '톺아보기'는 _샅샅이 높아 나가면서 살피다_라는 뜻이다.

## Git이란 무엇인가?

- 리누스 토발즈가 리눅스 커널 개발에 사용할 관리 도구 용도로 개발
- 분산 버전 관리 시스템으로 지역 저장소(Local Repository)와 원격 저장소(Remote Repository)가 존재
- 여러 사람이 같은 코드를 수정하고 공유하며, 하나의 파일로 모든 버전을 관리할 수 있다. 또한 동시에 같은 부분을 수정했을 때 생기는 문제들을 해결할 수 있다.

`git`을 사용하는 가장 큰 이유는 버전관리와 협업이다.  
`git`은 지역 저장소에서 버전 관리가 수행된다. 그렇기 때문에 원격 저장소나 네트워크에 문제가 있어도 작업이 가능하다.  
원격 저장소는 여러 사람들이 협업을 위해 버전이 공동으로 관리되는 곳이다. 자신의 버전 내역을 반영하거나, 다른 개발자의 반영된 버전 내역을 가져올 때 사용한다.

## Github는 무엇인가?

- `git` 저장소 호스팅을 지원하는 웹 서비스

`git`을 사용하는 프로젝트를 지원하는 웹 호스팅 서비스이기 때문에, `git`을 사용하는 프로젝트를 모두 `github`에서 관리할 수 있다. `gitlab`처럼 다른 호스팅 서비스를 이용할 수도 있다.

## Git 사용법

### 계정 정보 입력

```bash
git config --global user.email "email"
git config --global user.name "username"
```

### git status

`git`으로 관리되고 있는 파일의 상태를 확인할 수 있다.

### git의 3가지 상태

#### modified

- 파일을 수정하면 그 파일은 `modified` 상태가 된다.

#### staged

- 변경된 파일을 `git add` 명령어를 통해서 `stage area`로 올리면 `staged` 상태가 된다.

#### committed

- `staged` 파일을 `git commit -m "message"`를 통해서 해당 파일을 `committed` 상태로 바꿀 수 있다.
- 여기까지 진행하고 `git push`가 일어나야 원격 저장소에 해당 내용이 반영된다.

#### untracked files

- 아직 `git`이 관리하지 않는 파일
- 보안에 위협이 되는 파일은 `git`으로 관리하지 않는 것을 권장

### git add <filename>

- `modified` 상태의 파일을 `staged` 상태로 변경
- 해당 파일을 `stage`에 올림
- `git add .` 명령어를 통해서 `stage`에 올라가지 않은 파일 모두를 `stage`에 올릴 수 있다

### git restore <filename>

- `modified` 상태의 파일을 수정 이전의 상태로 변경
- 모든 파일을 초기화하려면 `git restore .` 명령어 사용

### git restore --staged <filename>

- `staged` 상태의 파일을 `modified` 상태로 변경
- 새로 추가된 파일의 경우에는 `untracked file` 로 변경됨
- 모든 파일을 초기화하려면 `git restore --staged .` 명령어 사용

### git rm

```bash
git rm <filename>
```
- 원격 저장소와 로컬 저장소 파일을 모두 삭제

```bash
git rm --cached <filename>
```
- 로컬 저장소의 파일은 보존하고, 원격 저장소에 있는 파일만 삭제

### .gitignore

- 해당 파일에 작성된 파일, 폴더는 `git`에서 관리하지 않음
- 매번 `git rm` 관련 명령어를 쓰지 않아도 된다

### git commit

- 해당 명령어만 입력하면 `vim`을 통해서 커밋 메시지 작성 가능

```bash
git commit -m "Commit Message"
```
- 보편적으로 사용하는 커밋 명령어

```bash
git commit -am "Commit Message"
```
- `git add`와 `git commit` 명령어를 한번에 사용 가능

### git log

- 커밋의 목록을 볼 수 있음

`--oneline` : 간략하게

### git diff

- 모든 코드의 수정 내용을 알고 싶을 때 사용

`git`입장에서는 코드가 수정되면 기존 코드가 삭제되고 새로운 코드가 추가되는 개념이다. 따라서 수정됐다고 생각하지 않는다.

## git으로 협업하기

현재까지의 명령어는 지역 저장소에서 사용 가능하다. 협업을 위해서는 `github`까지 사용해야한다.

### 원격 저장소 연결

```bash
git remote add <name> <url>
```
- `name`이라는 이름으로 `url`을 연결 (`name`은 `url` 별칭을 의미한다)
- `name`과 `url`은 필수

### 원격 저장소에 버전 내역 반영 (git push)

```bash
git push <remote> <branch>
```

- 지역 저장소의 `branch`브랜치의 버전 내역을 원격 저장소의 `branch` 브랜치에 반영한다
- `git remote`를 하고나면 기본적으로 `master`라는 브랜치가 생성됨
- `git push --help`를 입력했을 때 대괄호에 나오는 명령어는 옵션

### 원격 저장소에서 내용 불러오기 (git pull)

```bash
git pull <remote> <branch>
```
- 협업 시 충돌을 막기 위해 작업 전에 `git pull` 명령어로 시작

### git reset

- 커밋을 초기화시키고 싶을때 사용

```bash
git reset HEAD~1
git reset --mixed HEAD~1
```
`HEAD`는 현재 커밋의 위치를 나타낸다.  
위의 명령어는 현재 커밋의 위치에서 한개의 커밋 내용을 뒤로 돌리는 의미를 가지고 있다. 두 명령어 모두 같은 기능을 수행한다.

커밋 이후 해당 명령어를 사용하게 되면 파일들은 `modified`상태로 변경된다.(`committed` → `modified`)

```bash
git reset --soft HEAD~~1
```
`--soft`라는 옵션을 추가하게 되면 파일들이 `staged` 상태로 변경된다.(`committed` → `staged`)

```bash
git reset --hard <commitId>
```
`<commitId>` 이후 작성한 모든 커밋 내용을 삭제한다.

```bash
git reset --hard HEAD~1
```
- `committed` 상태를 수정 이전 혹은 생성 이전 상태로 변경 (`commited` → 기존 상태)
- `git reset HEAD~1` → `git restore <fileName>` 과정과 동일한 명령어 기능을 가지고 있다

이미 `github`에 올라간 내용을 되돌리기 위해선 어떻게 해야 할까?

`git reset` 명령어를 사용하게 되면 커밋 자체가 사라지기 때문에 되돌리려고 하는 내용을 다른 작업자가 알 수 없다. 그래서 되돌리는 내용도 표시할 수 있도록 `git revert` 명령어를 사용한다.

### git revert

- 되돌리려고 하는 커밋을 그래도 유지하고, 새로운 커밋을 통해서 되돌리는 기능
- 보통 협업할 때 `revert`명령어를 주로 사용한다
- 되돌리려고 하는 내용을 공유하기 위해 사용한다

```bash
git revert HEAD
git revert <commitId>
```
> `revert` 명령어를 취소한느 것은 커밋을 되돌리는 것과 동일하다
> `git reset --hard HEAD~1` 명령어를 사용하면 된다

### git branch

- 보편적으로 `master` 브랜치가 전체 프로젝트의 중심이 된다
- 신기능 등을 개발할 때 새로운 브랜치를 이용해서 신기능을 개발할 수 있다

`git branch`를 입력하면 현재 존재하는 브랜치의 목록을 나타낸다  
`-r`옵션을 통해 remote branch를 확인할 수 있다.  
`-a`옵션을 통해 local과 remote branch 모두를 확인할 수 있다.

#### 브랜치 생성

```bash
git branch <branch>
```

#### 브랜치 삭제

```bash
git branch -d <branch>
```

#### 브랜치 이동

```bash
git checkout <branch>
```

```bash
git switch <branch>
```
기존에 `checkout`을 많이 사용하였다. 그러나 최근에는 `switch`를 많이 사용한다.  
Git은 2019년 8월 16일 `2.23.0`으로 업데이트하면서 `checkout`의 기능을 `switch`와 `restore`을 통해 각각 분리하였다. 기존의 `checkout`은 브랜치를 생성하거나 이동, 복원하는 용도로 사용되었다.

__switch: 브랜치를 변경한다__

`switch` 명령어를 통해 브랜치를 변경할 수 있다. 기존에 존재하지 않는 브랜치를 생성하고 변경하고자 한다면 `-c` 옵션을 사용하면 된다.

__restore: 작업중인 파일을 되돌린다(복원)__

작업중인 파일 중 기존 마지막 커밋의 상태로 되돌리고자 할 때는 `restore`를 사용한다.

#### 브랜치를 나눠서 사용하는 이유

신기능을 개발하다가 운영 이슈 혹은 버그가 발생하게 되면, 브랜치를 나누지 않았을 경우 개발하던 신기능을 모두 되돌려야 한다.  
하지만 브랜치를 나눠서 사용하게 되면 운영에서 사용하고 있는 브랜치와 신기능을 개발하는 브랜치를 따로 생성해서 운영 이슈가 발생했을 때 브랜치 이동을 통해서 서로 코드의 간섭을 받지 않게 된다  
그렇기 때문에 항상 작업을 시작할 때는 `git branch` 명령어를 통해서 현재 브랜치의 위치를 확인하는 습관을 들이는 것이 좋다.  
추가적으로 `git pull origin <branch>` 명령어를 이용해서 다른 작업자들이 작업한 내용을 동기화하고 작업을 진행하는 것이 좋다.

### git merge

- 현재 브랜치에서 다른 브랜치의 변경 내역을 가져온다

```bash
git merge <branch>
```

만약 `develop` 브랜치에서 `master` 브랜치의 변경 내역을 가져오고 싶다면, `git checkout develop` → `git merge master` 순서로 명령어를 입력하면 된다.  
`merge` 과정을 포기하고 싶다면 `git merge --abort` 명령어를 입력하면 된다.

### git rebase

- 현재 브랜치에 다른 브랜치의 변경 내역을 가져오면서 `base`를 재설정한다.

```bash
git rebase <branch>
```

`git merge`의 경우에는 현재 브랜치에 다른 브랜치의 변경 내역을 가져오면서 새로운 커밋이 발생하게 되는데, `git rebase`의 경우에는 현재 브랜치와 다른 브랜치의 변경 내역을 시간 순서대로 반영하고, 새로운 커밋이 발생하지 않는다.

하지만 많은 작업자들이 사용하는 브랜치의 경우 `rebase` 명령어를 사용하는 것은 위험하다.

기존 작업자들의 커밋 히스토리가 변경되는 경우에는 각 작업자들은 때에 따라 자신의 커밋을 다시 반영하거나 재작업을 해야할 수도 있다.

`git rebase` 진행 중 `conflict`가 발생하는 경우, `git rebase --continue` 명령어를 입력하면 된다.

`rebase` 과정을 포기하고 싶다면 `git rebase --abort` 명령어를 입력하면 된다.

#### conflict가 발생했을 경우

`git merge`나 `git pull`, `git rebase`와 같은 명령어를 사용하다보면 `conflict`가 발생할 수 있다.

다른 작업자와 같은 코드를 수정하게 돼서 나타나는 "__충돌__"이다.

이러한 경우 우선적으로 해당 코드 내용을 수정한다.

수정하지 않고 파일을 추가, 커밋, 푸시하게 되면 `conflict`가 발생한 부분은 각 브랜치에서 들어온 내용을 모두 반영하게 된다.

수정이 완료 됐다면 해당 파일을 `add` → `commit` → `push` 한다.

### git cherry-pick

- 다른 브랜치의 변경 내역을 가져오고 싶은 경우에 대부분 `rebase`, `merge` 명령어를 사용한다.
- 특정 변경 내역을 가져오고 싶을 때 `cherry-pick` 명령어를 사용한다.

### git stash

- 어느 브랜치에서 수정 작업을 진행하던 중 다른 브랜치로 이동하고 싶을 때 사용한다.
- 보통 작업을 진행하던 중 다른 브랜치로 이동하게 되면 에러가 발생하게 된다.
- 임시 저장소를 생성해서 해당 작업 내용을 저장한 뒤 브랜치를 이동한다.

#### 임시 저장소 생성

```bash
git stash
```

- 작업 중인 내용이 임시 저장소에 저장이 되고, 현재 브랜치에는 수정 사항이 없어지게 된다.
- 이후 `git checkout` 명령어를 사용해서 다른 브랜치로 이동할 수 있다.

#### 임시 저장소 목록

```bash
git stash list
```

- 저장된 내용을 목록으로 확인할 수 있다. 최근에 추가된 내용이 0번으로 추가된다.

#### 저장 내용 반영

```bash
git stash apply
```

- 저장 내용을 현재 브랜치에 반영한다.

#### 임시 저장소 삭제

```bash
git stash drop
```

- `git stash apply` 명령어를 사용하게 되는 경우 저장 내용은 반영하지만, 임시 저장소에 해당 내용이 삭제되지는 않는다.
- 저장 내용 반영과 저장소 삭제를 동시에 진행하고 싶다면, `git stash pop` 명령어를 사용하면된다.

`git stash` 명령어를 유용하게 사용하는 경우는 브랜치를 착각했을 때이다.

`develop` 브랜치에서 작업해야 되는 기능을 브랜치를 확인하지 않고 `master` 브랜치에서 작업했다고 가정하자.  
이러한 경우에는 작업한 내용이 크지 않다면, 수정 사항을 `git restore` 명령어를 통해서 되돌리고 브랜치를 이동할 수도 있다. 하지만 작업 내용이 많다면 쉽지 않다.
```bash
git stash
git checkout develop
git stash pop
```
위와 같이 명령어를 입력하게 되면 모든 수정 사항이 `develop` 브랜치로 이동된다.

`git stash pop` 명령어를 사용했을 때, `conflict`가 발생하게 될 수 있다. 이러한 경우에는 동일하게 수정하고 반영하면 된다.