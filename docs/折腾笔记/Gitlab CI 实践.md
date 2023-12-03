#CI #Gitlab

# 安装

[文档](https://docs.gitlab.com/runner/install/docker.html)

# 项目 CI 配置

```yml
image: inovex/gitlab-ci-android  
  
stages:  
  - build  
  
before_script:  
  - export GRADLE_USER_HOME=$(pwd)/.gradle  
  - chmod +x ./gradlew  
  
cache:  
  key: ${CI_PROJECT_ID}  
  paths:  
    - .gradle/  
  
build:  
  stage: build  
  script:  
    - ./gradlew assembleDebug  
  artifacts:  
    name: demo  
    paths:  
      - app/build/outputs/apk/debug/*.apk
```

# 防止 gitlab-runner 每次 pull 镜像

[官方文档](https://docs.gitlab.com/runner/executors/docker.html#how-pull-policies-work)

需要在 runner 配置的 `runners.docker` 节点中配置 `pull_policy = ["if-not-present"]`

![[Pasted image 20221115223342.png]]

# 获取最新 Artifact

```shell
curl -L -sS --header "PRIVATE-TOKEN: glpat-td2V1mard4YutRj_PNn_" https://gitlab.com/api/v4/projects/<project-id>/jobs/artifacts/<branch>/download?job=<job-name> -o ss.zip
```

需要先创建一个 Token
