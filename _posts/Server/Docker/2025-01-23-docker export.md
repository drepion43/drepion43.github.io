---
title:  "[Docker]Image/Container Export"
categories: Docker
tag: [linux, Docker]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: jupyter notebook
comments: true
date: 2025-01-23
toc_sticky: true
---

# 개요
보통 docker build나 commit으로 만들어진 이미지는 docker hub와 같은 registry에 push하고 pull하여 사용합니다. 하지만 이런 경우에는 docker hub와 통신이 가능한 경우이며, 폐쇄망인 경우에는 docker conatiner나 image는 직접 파일을 옮겨줘야합니다. 이런 경우를 위해 docker에서는 export, import, save, load를 지원해줍니다. 이번절에서는 이에 대해 살펴보겠습니다.   
여기서 export-import, save-load가 한 쌍이라고 생각하시면됩니다.

## docker save(docker image $\rightarrow$ tar)
docker save는 docker image를 tar파일로 만드러 외부로 노출시킬 수 있게 해줍니다. 이 경우, docker image를 build한 후, 본인의 환경에 맞게 customization을 한 후, 이 container를 다시 image로 남겨두고 싶어 image로 만든 후, 그 image를 외부로 노출시킬 때 유용합니다. 그럼 우선 customization을 한 conatiner를 다시 image로 commit하는 cli에 대햏 살펴보겠습니다.   
```bash
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```
상기의 cli에서 OPTIONS에는 이미지의 작성자를 나타내는 -a 옵션을 줄 수도, 커밋 메세지를 나타내는 -m 옵션을 주어 메타데이터를 포함시킬 수 있습니다. cli를 작성하시면 뒤에 REPOSITORY[:TAG]의 이름으로 Docker Image가 생성됩니다. 그럼 이렇게 만든 Image를 외부로 노출시킬 수 있는 save cli사용법을 살펴보겠습니다.   
```bash
docker save [OPTIONS] tar File [IMAGE]
docker save -o test.tar test_image:latest
``` 

상기와같이 작성을 해주시면 Docker Image를 담고 있는 test.tar파일 결과를 얻을 수 있습니다. 이를 원하는 서버로 이동시켜 다시 이미지화해주시면 외부에 동일한 환경의 Docker Image가 생성하실 수 있습니다.   

## docker load(tar $\rightarrow$ docker image)
```bash
docker load -i tar File
```
tar파일로 만들어진 이미지를 다시 docker image로 되돌릴 수 있습니다.   

## docker export(docker container $\rightarrow$ tar)
이번에는 Container 자체를 이동시킬 때 사용하는 cli입니다. export는 container를 실행할 때 필요한 모든 파일들이 압축이됩니다. 즉, root 파일시스텤이 모두 압축이되게 되며, 용량이 매우 커질 것입니다.   
```bash
docker export [Container] > tar File
```

## docker import(tar $\rightarrow$ docker image)
import는 cli는 export로 만든 tar파일을 다시 image로 생성하는 cli입니다.    
```bash
docker import tar File
```



