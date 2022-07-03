---
title:  "Ubuntu 간단한 패키지 설치"
categories: test
tag: [linux, ubuntu]
toc: true
author_profile: false
sidebar:
    nav: "docs"
---

```python
VIM INSTALL
sudo apt-get update
sudo apt-get install vim

TERMINATOR INSTALL
sudo apt install terminator

VSCODE INSTALL
sudo apt-get install curl
sudo sh -c 'curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/microsoft.gpg'
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt-get update
sudo apt install code

CHROME INSTALL
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb

```
