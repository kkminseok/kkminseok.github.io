---
title: 내 blog 버전별 업데이트 로그 정리 ~Version 3까지
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-02 01:00:00 +0800
categories: [Blogging,Tutorial]
tags: [start git blog]
math: true
mermaid: true
image: 
  path: /assets/img/sample/logo.jpg
comments : true
---

내가 기존에 썼던 버전은 3.1.0 언저리였다. 그 기준으로 업데이트 로그를 작성하겠다.

> 원본은 <https://github.com/cotes2020/jekyll-theme-chirpy/releases?page=2>에 있습니다.!!
{: .prompt-tip }

# v3.1.0

## New
- post preview image 설정에 `alt`기능을 추가하였습니다.
- site 설정 부분에 `lang`을 추가하였습니다. -> 방문자 유입 국적에 따라 언어가 바뀌게 보임.

## Improved
- _config.yml에서 breadcrumb?를 삭제하여 유연하게 breadcrumb을 생성할 수 있게 하였습니다.
- `/tabs/`를 tabs에서 제거하였습니다.
- `gemspec` 파일을 output에서 제거하였습니다.
- `Gemfile`의 중복을 제거하였습니다.

## Fixed
- latest/oldest post의 `nav button`을 복구하였습니다.
- post의 image bottom margin을 고쳤습니다.

-----

# v3.2.0

## Improved
- 코드블록을 최적화하였습니다.
- 사이트 제외 목록에 정규식을 사용하였습니다.
- `travis job`을 도입하였습니다.
- 검색 결과 항목의 최소 너비를 향상시켰습니다.
- 문서에 분기를 추가하였습니다.

## Fixed
- 가이드라인을 수정하였습니다.

-----

# v3.2.1

## Hotfix
- 게시공유 옵션 버튼을 복구하였습니다.

-----

# v3.2.2

## Hotfix
- 코너에 있는 latest/oldest post의 `navigation buttons`을 고쳤습니다.
- list속에 있는 image의 넓이를 수정하였습니다.

-----

# v3.3.0

## New
- math에서 `TeX`와`LaTex`단위를 추가하였습니다.

## Improved
- 깃허브 액션에서 `ruby setup`부분을 업데이트 하였습니다.
- site.author를 제거하였습니다.
- `timeago`를 개선하고, 포스트 정렬 알고리즘을 개선하였습니다.
- `bump tool`을 향상시켰습니다.

## Fixed
- 서비스 제공자가 Post로 요청온 값을 캐시 키로 저장하여 발생한 PWA 에러를 수정하였습니다.

-----

# v3.3.1

## Hotfix
- math에서 `Tex`와`LaTex`설정이 배포버전에서 잘못되어 수정하였습니다.(오역 가능성)

-----

# v3.3.2

## Hotfix
- 로컬 캐시가 없는 경우 PV 결과를 failure로 나타내는 경우를 수정하였습니다.
- 검색 결과의 `link color`를 수정하였습니다.

