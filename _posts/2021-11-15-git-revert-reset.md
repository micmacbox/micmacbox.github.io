---
layout: post
title: git revert vs reset
categories: git
tags: [git]
date: 2022-03-02 01:54 +0800
# toc: true
---

## 이전 커밋으로 되돌리기

reset과 revert 사용법과 차이점을 정리해봅니다.

## git revert와 reset의 차이점

원격 저장소에서 다른 사람과의 협업 유(revert)무(reset)에 따라서 달라진다.

reset: 되돌리고 싶은 커밋의 시점으로 이동하는것 (HEAD의 위치 변경)

revert: 원하는 커밋을 되돌리며 로그를 남기는 것 (커밋을 되돌리는 커밋을 만듦)

## reset

나 혼자 쓰는 브랜치라면 reset을 써도 무방하다.

    // 커밋해시를 이용해 되돌리기
    git reset [해시id]
    // 바로 이전 커밋으로 되돌리기
    git reset HEAD^
    // 마지막 2개의 commit을 취소 및 삭제
    git reset HEAD~2

    // --soft 옵션: 변경 이력 삭제 및 변경 내용은 stage 상태
    git reset --soft [해시id]
    // --mixed 옵션: 변경 이력 삭제 및 변경 내용은 unstage 상태
    git reset --mixed [해시id]
    // --hard 옵션: 변경 이력 삭제 및 커밋id 이후의 변경 내용까지 삭제
    git reset --hard [해시id]

    // --force or -f 옵션: reset 후 원격 저장소에 push (혼자 쓸 경우에만 쓰기)
    git push -f origin [브랜치명]

## revert

revert 명령어는 이미 서버에 push를 해버렸을 경우 주로 사용되며 revert에 대한 커밋 메시지가 추가된다.

    // 해시id에 해당하는 내용만 삭제되며 커밋메시지에 해당 커밋이 삭제된 이력이 남는다
    git revert [해시id]
    // --no-commit 옵션: revert한 결과를 커밋않고 stage 상태로 유지.
    git revert --no-commit [해시id]
    // 여러개 커밋 되돌리기
    git revert [해시id]..[해시id]
