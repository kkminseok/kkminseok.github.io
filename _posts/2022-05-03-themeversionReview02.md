---
title: 내 blog 버전별 업데이트 로그 정리 ~Version 4까지
author: 
    name: minseok
    link: https://github.com/kkminseok
date: 2022-05-04 01:00:00 +0800
categories: [Blogging,Tutorial]
tags: [start git blog]
math: true
mermaid: true
image: 
  path: /assets/img/sample/logo.jpg
comments : true
---


> 원본은 <https://github.com/cotes2020/jekyll-theme-chirpy/releases?page=2>에 있습니다.!!
{: .prompt-tip }

# v4.0.0

## New

- local에서 배포할때의 `pageview`를 지원하게 되었습니다.
- `shadow`옵션을 이미지 첨부할 때 추가할 수 있도록 하였습니다.
- 팝업 이미지를 추가하였습니다.

## Improved
- PWA cache storage를 줄였습니다.
- `gem installation`에서 요구하는 외부 파일들을 줄였습니다.
- `favicon`을 간단하게 하였습니다. > 정확히는 모르겠습니다.
- `new tablets`에서의 디자인 반응을 개선하였습니다.
- `ios`에서 `safari` 브라우저로 들어왔을때 검색창을 확대할 수 있는것을 방지하였습니다.
- `panel`의 텍스트컬러를 통합하였습니다.
- `sidebar`의 스크롤바를 숨겼습니다.

## Fixed
- 사이트를 업데이트한 후 `Service Worker`가 `event error`를 가져옵니다.
- 몇몇 `CDN URL`이 헤더에 포함되지 않는 것을 고쳤습니다.
- `RSS` 템플릿의 `author`를 수정할 수 없던 것을 고쳤습니다.
  
-----

# v4.0.1

## Hotfix
- gem에 있는 몇몇 의존성 파일들이 누락되었던 것을 고쳤습니다.

-----

# v4.0.2

## Hotfix
- `Ruby`의 양립할 수 없는 공용버전 범위를 제한하였습니다. 
  
-----

# v4.1.0

## New
- `localization` 레이아웃을 지원합니다. > 이해못했스빈다.
- hook에 `SCSS variables`을 커스텀할 수 있게 추가하였습니다.
- 모바일환경에서 `paginator`를 재디자인 하였습니다. (페이지 1,2,3...12 이런식으로 넘어가는 부분)

## Improved
- 이미지나 `post`가 로딩중일때 레이아웃이 옮겨지는 것을 방지하였습니다.
- 로딩중일때 `code language badge`가 꺼지는 현상을 방지하였습니다.
- `lazy-loaded`를 선택한 이미지인 경우 서서히 사라지는것을 추가하였습니다.

## Fixed
- `pagination`의 `bread-crumb`이 정확하지 않은것을 수정하였습니다.
- The items of .gitignore > ????
- 몇몇 `docs`에서 `Typo`에 실수를 했던것을 고쳤습니다. > ..?
  
-----

# v4.1.1

## Hotfix
- 새로운 탭 페이지의 `default name`이 부정확한것을 수정하였습니다.
- `site config`에서 불필요한 변수는 제거하였습니다.
- 몇몇 문서의 `markdown` 문법이 에러가 있는것을 고쳤습니다.

-----

# v4.2.0

## New
- `Disqus` 테마가 자동으로 바뀌게 설정하였습니다.
- `macOs` 스타일의 스크롤바를 생성하였습니다.
- code block
  - `title option`을 제작할 수 있게 하였습니다.
  - `line number`를 숨길 수 있게 하였습니다.
  - 클립보드에 복사하는 버튼을 추가하였습니다.

## Improved
- `full-text` 검색을 가능하게 하였습니다.
- `Locale stuff` > ??

## Fixed
- 몇몇 경우 Tab의 이름이 부적절한 것을 수정하였습니다.

-----

# v4.2.1

## Hotfix
- 실수로 삭제된 `post` 복사링크를 복구하였습니다.
- Safari에서 `code block`의 `line number`가 복사되는 것을 방지하였습니다.

-----

# v4.3.0

## New
- 사이드바의 디자인을 수정하였습니다.
- `code block`의 상단 아이콘을 추가하였습니다.
- `syntax language`의 별칭을 공식 이름으로 수정하였습니다. ? > Convert the alias of the syntax language to the official name
- Ruby 3.0 의존성을 추가하였습니다.

## Improved
- `color scheme`의 디자인을 개선하였습니다.
- `tips`의 `code button`을 숨깁니다.
- `code block`의 스크롤바를 자동으로 숨김처리 하였습니다.
- `code block`버튼의 웹 접근성을 강화하였습니다.
- 깃허브 액션이 자동으로 테마를 업그레이드하는것을 방지하였습니다.
- 자동배포에 필요한 파일을 줄였습니다.
  
## Fixed
- `comment block`이 로딩시간동안 jumping..?넘어가는것을 방지하였습니다.

-----

# v4.3.4

## Hotfix
- `NPM`의 `lock-file`이 웹사이트 결과값에 포함되어 복사되는 것을 수정하였습니다.
- `json`,`plaintext` 등 몇몇 언어들이 `code block`에서 제대로 바뀌지 않는 것을 수정하였습니다.
- 다수의 `footnote`가 서서히 사라지는것을 수정하였습니다. > Multiple reverse footnote will overlap (#439)
  


> 영어공부 열심히 해야겠다..

