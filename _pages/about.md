---
layout: page
title: About
permalink: /about/
---

## 익숙한 공간에서 내 로그 작성하기
>블로그 공간을 제공하는 사이트도 많고, googledoc을 활용해도 되긴 하지만,
>언제나 고민되는 것은 돈, 접근성, 데이터의 이식성이다.
>
>광고 배너없는 순수 무료 호스팅을 이용하며, \
>쉽게 호스팅을 닫고 컨텐츠만 로컬에 저장한다던가, \
>사이트를 변경하고 싶었다.
>
>AWS에서 workpress 등을 이용하여 직접 관리할 수도 있지만, AWS는 관리 소홀로 해킹을 당해 금전적 손해를 겪을 수도 있다고 했다.

### 환경 검토
- 무료 호스팅 사이트
    - github, gitlab
- SSG [Static Site Generator](https://en.wikipedia.org/wiki/Static_site_generator)
    - markdown같은 텍스트 파일을 이용해서 static site를 생성해주는 엔진
        - static site : <-> dynamic site. 
            - 항상 동일한 컨텐츠를 렌더링
            - 사용자 입력이나 스크립트 등에 영향을 받지 않음
    - jekyll(github 1위, 대용량 사이트 성능 이슈 있음), Hugo(Go, 성능 좋다고 알려짐), Next.js 등

### 비교해보기
1. gitlab with hugo
    - 사이트 작성 : https://gohugo.io/getting-started/quick-start/
        - hugo.toml 작성(config.toml is obsolte)
        - theme : archie
        ```
        # build sites
        hugo server
        ## server address: http://127.0.0.1:1313/blog/

        # for pulish
        hugo
        ```
    - publish : 
        - https://gohugo.io/hosting-and-deployment/hosting-on-gitlab/
        - gitlab - create blank project
            - Visibility -> public
            - Deployment target -> Gitlab Pages
        - ~~https://beanfly80.gitlab.io/log~~ (Closed)
 
    <img src="/assets/gitlab.png" width="90%" height="90%" title="gitlab">

1. github with jekyll
    - 사이트 작성
        - https://jekyllrb.com/docs/github-pages/
        - theme : ~~default(minima)~~ reverie
        ```
        bundle exec jekyll serve
        ## server address: http://127.0.0.1:4000/
        ```
    - publish
        - https://pages.github.com/
        - https://beanfly80.github.io

### 결과
1. github with jekyll
    - 매우 간단! 코드 푸쉬되면, 자동으로 호스팅 시작
    - hugo에 비해 빌드 & 적용이 느린 게 느껴지나, 아직 포스트가 몇 개 안되므로.
    - host url도 간단
2. gitlab with hugo
    - go기반으로 프레임워크가 깔끔한 느낌
    - 빠름
    - custom css 수정 쉽다
    - repo 이름이 추가되어, url 이 길다
    - deplyoment(.gitlab-ci.yml) 작성 필요

----
> github에서 jekyll을 이용해서 간단하게 시작하기로 한다.