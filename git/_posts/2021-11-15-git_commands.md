---
layout: post
title: git 명령어 정리
categories: git
tags: [git]
date: 2021-11-15 01:54 +0800
# toc: true
---

## git commit 취소

    // [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
    git reset --soft HEAD^
    // [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
    git reset --mixed HEAD^ // 기본 옵션
    git reset HEAD^ // 위와 동일
    git reset HEAD~2 // 마지막 2개의 commit을 취소 및 삭제
    // [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
    git reset --hard HEAD^
    // 원격 저장소에 커밋내역 push
    git push -f origin [브랜치 이름] 브랜치가 main이면 대괄호 없이 씀

## git push 취소

    // commit 취소 후 다시 commit 한다.
    git commit -m "Write commit messages"
    // 원격 저장소에 push한다.
    git push origin [branch name] -f
    // 또는
    git push origin +[branch name]

## git history 확인

    // 커밋로그 확인
    git log
    // 특정 커밋의 descriptsion과 수정된 파일, 수정 내용 확인
    git show [commit id]
    // 특정 커밋의 수정 파일 목록만 보고싶다면
    git show [commit id] --name-only
    // --name-only 옵션. 수정된 파일 목록 보기
    git log --name-only
    // -p 옵션. 수정된 파일 목록과 수정내용 모두 확인
    git log -p

## git reflog

git rebase 또는 git reset 등으로 커밋이 삭제하는 경우에도 git 이력은 보관되며 이 이력을 볼 수 있다.

![](/assets/img/git_reflog.png)

    // commit 복구하기
    git reset --hard <커밋해시id>
    // branch 복구하기
    // git reflog 또는 git reflog |grep 브랜치명 으로 log확인
    git checkout -b <삭제한 브랜치명> <커밋해시id>

## git branch 삭제

    //원격저장소 브랜치 삭제
    git push origin --delete [브랜치이름]
    //로컬저장소 브랜치 삭제
    git branch -d [브랜치이름]

## git branch 업데이트

    // 원격의 브랜치를 찾지 못해서 발생하는 오류 발생 시 git remote 갱신.
    git remote update

## 원격 저장소 branch 확인

    //원격 저장소의 branch 리스트 확인
    git branch -r
    //로컬, 원격 모든 저장소의 branch 리스트 확인
    git branch -a

## 원격 저장소 branch 가져오기

    //로컬의 동일한 이름의 branch를 생성하여 해당 branch로 checkout
    git checkout -t [브랜치이름]
    //branch이름 바꿔서 가져오기
    git checkout -b [생성할 branch 이름] [원격 저장소의 branch 이름]
