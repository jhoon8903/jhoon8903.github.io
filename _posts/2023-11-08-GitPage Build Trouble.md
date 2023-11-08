---
layout: post
title: Git Page Build 의존성 문제
subtitle: Git Page 의존성 문제 해결을 통한 빌드
author: Daniel
categories: 문제해결
tags: 
 - Github
 - GitPage
banner: 
  image: https://i.imgur.com/7pagCRT.jpg
---

GItHub Page Build 문제 해결
--

> Github Page Build 의존성 문제로 인한 정상적인 페이지 빌드의 버그 발생

## Warning Log

```text
Run actions/jekyll-build-pages@v1

[9](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:10)/usr/bin/docker run --name ghcrioactionsjekyllbuildpagesv109_bd7f43 --label 715fc6 --workdir /github/workspace --rm -e "INPUT_SOURCE" -e "INPUT_DESTINATION" -e "INPUT_TOKEN" -e "INPUT_FUTURE" -e "INPUT_BUILD_REVISION" -e "INPUT_VERBOSE" -e "HOME" -e "GITHUB_JOB" -e "GITHUB_REF" -e "GITHUB_SHA" -e "GITHUB_REPOSITORY" -e "GITHUB_REPOSITORY_OWNER" -e "GITHUB_REPOSITORY_OWNER_ID" -e "GITHUB_RUN_ID" -e "GITHUB_RUN_NUMBER" -e "GITHUB_RETENTION_DAYS" -e "GITHUB_RUN_ATTEMPT" -e "GITHUB_REPOSITORY_ID" -e "GITHUB_ACTOR_ID" -e "GITHUB_ACTOR" -e "GITHUB_TRIGGERING_ACTOR" -e "GITHUB_WORKFLOW" -e "GITHUB_HEAD_REF" -e "GITHUB_BASE_REF" -e "GITHUB_EVENT_NAME" -e "GITHUB_SERVER_URL" -e "GITHUB_API_URL" -e "GITHUB_GRAPHQL_URL" -e "GITHUB_REF_NAME" -e "GITHUB_REF_PROTECTED" -e "GITHUB_REF_TYPE" -e "GITHUB_WORKFLOW_REF" -e "GITHUB_WORKFLOW_SHA" -e "GITHUB_WORKSPACE" -e "GITHUB_ACTION" -e "GITHUB_EVENT_PATH" -e "GITHUB_ACTION_REPOSITORY" -e "GITHUB_ACTION_REF" -e "GITHUB_PATH" -e "GITHUB_ENV" -e "GITHUB_STEP_SUMMARY" -e "GITHUB_STATE" -e "GITHUB_OUTPUT" -e "RUNNER_OS" -e "RUNNER_ARCH" -e "RUNNER_NAME" -e "RUNNER_ENVIRONMENT" -e "RUNNER_TOOL_CACHE" -e "RUNNER_TEMP" -e "RUNNER_WORKSPACE" -e "ACTIONS_RUNTIME_URL" -e "ACTIONS_RUNTIME_TOKEN" -e "ACTIONS_CACHE_URL" -e "ACTIONS_ID_TOKEN_REQUEST_URL" -e "ACTIONS_ID_TOKEN_REQUEST_TOKEN" -e "ACTIONS_RESULTS_URL" -e GITHUB_ACTIONS=true -e CI=true -v "/var/run/docker.sock":"/var/run/docker.sock" -v "/home/runner/work/_temp/_github_home":"/github/home" -v "/home/runner/work/_temp/_github_workflow":"/github/workflow" -v "/home/runner/work/_temp/_runner_file_commands":"/github/file_commands" -v "/home/runner/work/jhoon8903.github.io/jhoon8903.github.io":"/github/workspace" ghcr.io/actions/jekyll-build-pages:v1.0.9

[10](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:11)fatal: detected dubious ownership in repository at '/github/workspace'

[11](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:12)To add an exception for this directory, call:

[12](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:13)

[13](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:14) git config --global --add safe.directory /github/workspace

[14](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:15)Warning: the running version of Bundler (2.1.4) is older than the version that created the lockfile (2.4.21). We suggest you to upgrade to the version that created the lockfile by running `gem install bundler:2.4.21`.

[15](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:16)Resolving dependencies...

[16](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:17)The following gems are missing

[17](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:18) * rake (12.3.3)

[18](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:19) * public_suffix (5.0.3)

[19](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:20) * google-protobuf (3.25.0)

[20](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:21) * sass-embedded (1.69.5)

[21](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:22) * jekyll-sass-converter (3.0.0)

[22](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:23) * kramdown (2.4.0)

[23](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:24) * mercenary (0.4.0)

[24](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:25) * rouge (4.2.0)

[25](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:26) * unicode-display_width (2.5.0)

[26](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:27) * terminal-table (3.0.2)

[27](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:28) * webrick (1.8.1)

[28](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:29) * jekyll (4.3.2)

[29](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:30) * jekyll-feed (0.17.0)

[30](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:31) * racc (1.7.3)

[31](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:32) * rainbow (3.1.1)

[32](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:33) * jekyll-spaceship (0.10.2)

[33](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:34)Install missing gems with `bundle install`

[34](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:35)Warning: github-pages can't satisfy your Gemfile's dependencies.

[35](https://github.com/jhoon8903/jhoon8903.github.io/actions/runs/6794109890/job/18470010385#step:4:36)To use retry middleware with Faraday v2.0+, install `faraday-retry` gem
```

### 문제점 

```text
Warning: the running version of Bundler (2.1.4) is older than the version that created the lockfile (2.4.21). We suggest you to upgrade to the version that created the lockfile by running `gem install bundler:2.4.21`.
```

해당 Bundler의 옵션이 맞지 않아 필요한 의존 gems 패키지가 정상적으로 설치 되지 않는 문제<br>
해당 문제는 페이지 자체적 빌드는 문제가 없지만 내부의 패키지의 의존하는 몇몇 프로그램
- jekyll-spaceship (0.10.2)
- 셀병합
- PlantUML
- Mermaid    
해당 패키지가 정상적으로 로드 되지 않아 Page의 스크립트가 정상적으로 로드 되지 않음


## 해결방법

> 1. `_config.yml` 수정
> 2. git workflows 작업
> 3. git pages Deploy Option 변경

1번과 2번의 문제를 해결하였을 때도 정상적으로 로드 되지 않는 문제가 발생
이때 발견한 문제는 WorkFlows와 같이 기본 `pages build and deployment`   
2개의 빌드가 동시에 작동하는 문제를 발견하였음

![](https://i.imgur.com/oV2XeYK.jpg)

이 부분에서 Source가 기본으로 되어 main 브랜치에 적용되면 바로 진행되는 문제가 발생

![](https://i.imgur.com/yNJlesR.jpg)

Source 액션을 변경하여 다시 Deploy 진행하여 문제 해결<br>이제 해당 GitPage에서도 정상적으로 랜딩 되는 것을 확인 

![](https://i.imgur.com/A2tNS8H.jpg)

![](https://i.imgur.com/iRoKxxe.jpg)

