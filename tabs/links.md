---
layout: links
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: id_links

# publish date (used for seo)
# if not specified, site.time will be used.
#date: 2022-03-03 12:32:00 +0000

# for override items in _data/lang/[language].yml
#title: My title
#button_name: "My button"
# for override side_and_top_nav_buttons in _data/conf/main.yml
#icon: "fa fa-bath"

# seo
# if not specified, date will be used.
#meta_modify_date: 2022-03-03 12:32:00 +0000
# check the meta_common_description in _data/owner/[language].yml
#meta_description: ""

# optional
# please use the "image_viewer_on" below to enable image viewer for individual pages or posts (_posts/ or [language]/_posts folders).
# image viewer can be enabled or disabled for all posts using the "image_viewer_posts: true" setting in _data/conf/main.yml.
#image_viewer_on: true
# please use the "image_lazy_loader_on" below to enable image lazy loader for individual pages or posts (_posts/ or [language]/_posts folders).
# image lazy loader can be enabled or disabled for all posts using the "image_lazy_loader_posts: true" setting in _data/conf/main.yml.
#image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false

# you can always move this content to _data/content/ folder
# just create new file at _data/content/links/[language].yml and move content below.
###########################################################
#                Links Page Data
###########################################################
page_data:
  main:
    header: "Links"
    info: "참고하는 링크들 좋은 채널 또는 페이지가 있다면 계속 Update"

  # To change order of the Categories, simply change order. (you don't need to change list order.)
  category:
    - title: "Github"
      type: id_github
      color: "#181717"
    - title: "Project Tools"
      type: id_project
      color: "#7952B3"
    - title: "Blog"
      type: id_blog
      color: "#62b462"
    - title: "Flutter"
      type: id_flutter
      color: "#0175C2"
    - title: "Unity"
      type: id_unity
      color: "#334455"
    - title: "Programming"
      type: id_programming
      color: "#F58025"

  list:
    -
    # Programming Category
    - type: id_programming
      title: "Stack OverFlow"
      url: "https://stackoverflow.com/"
      info: "우리의 선생님 갓 스택 플로우"
    - type: id_programming
      title: "naver d2"
      url: "https://www.youtube.com/@naverd2848"
      info: "Naver d2 공식 Youtube 채널 - 업로드되는 DEVIEW의 내용들이 너무 좋다 추천합니다."
    - type: id_programming
      title: "코딩애플"
      url: "https://www.youtube.com/@codingapple"
      info: "코딩애플 공식 Youtube 채널 - 정말 쉽게 설명해주는 채널 재미있다."
    - type: id_programming
      title: "우아한테크"
      url: https://www.youtube.com/@woowatech
      info: "우테크 채널 아키텍처 및 서비스개선등 시간이 지나도 참고할만한 정보가 너무나 많이 있다."

    # Github Category
    - type: id_github
      title: "Shields IO"
      url: "https://shields.io/"
      info: "깃헙 꾸미기에서 뱃지 만드는 페이지"
    - type: id_github
      title: "Simple Icons"
      url: "https://simpleicons.org/"
      info: "깃헙을 꾸미거나 각 아이콘의 컬러를 알고 싶을때 이용하면 좋은 페이지"
    - type: id_github
      title: "anuranghazra Github"
      url: "https://github.com/anuraghazra/github-readme-stats"
      info: "깃헙 Readme를 꾸미기 위한 위젯 설명이 잘 되어 있다."
    - type: id_github
      title: "Gitmoji 사용법"
      url: "https://inpa.tistory.com/entry/GIT-%E2%9A%A1%EF%B8%8F-Gitmoji-%EC%82%AC%EC%9A%A9%EB%B2%95-Gitmoji-cli"
      info: "갓 파 인파의 블로그 중 Gitmoji 사용법이 잘 정리되어 있다."
    - type: id_github
      title: "Markdown Guide"
      url: "https://www.markdownguide.org/"
      info: "마크다운 언어 공식 가이드 Readme를 작성하거나 Notion Page등 MK를 사용할 줄 알면 쉽게 꾸밀 수 있다."

    # Project Tools
    - type: id_project
      title: "ERD Cloud"
      url: "https://www.erdcloud.com"
      info: "ERD 작성시에 꼭 필요한 ERD Cloud"
    - type: id_project
      title: "Excalidraw"
      url: "https://excalidraw.com"
      info: "이미지를 그리거나 그린 이미지를 공유_협업이 가능한 Excalidraw"
    - type: id_project
      title: "이모티콘모음"
      url: "https://kr.piliapp.com/facebook-symbols"
      info: "각종 이모티콘 모음 페이지 웹에서 공통적으로 사용가능한 이모티콘이 모여있다."
    - type: id_project
      title: "New Pen"
      url: "https://codepen.io/pen/?editors=0010"
      info: "간단한 코드 작성시 html css 등 바로 적용되는 모습을 볼 수 있다."
    - type: id_project
      title: "Mockaroo"
      url: "https://www.mockaroo.com"
      info: "Mock Data 생성시에 다양한 카테고리로 만들 수 있다 10000개 생성은 무료"

    #Blog
    - type: id_blog
      title: "인파 블로그"
      url: "https://inpa.tistory.com"
      info: "갓파의 블로그 왠만한 자료는 다 있다."

    #Flutter
    - type: id_flutter
      title: "Coding Papa"
      url: "https://www.youtube.com/@TheCodingPapa"
      info: "더 코딩파파 youtube 공식 채널 - Flutter 정보를 얻기좋다"

    #Unity
    - type: id_unity
      title: "Unity 공식 채널"
      url: "https://www.youtube.com/@UnityKoreaHi"
      info: "유니티 youtube공식 채널"
---
