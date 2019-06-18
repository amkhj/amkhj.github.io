---
layout: post
title: 전원 연결시 AMK 단말 버튼으로 바로 실행하기 편
date: 2019-06-18 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: amk_button.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [kt amk, raspberry pi, auto booting]
---
본 포스트는 AMK 단말에 전원을 연결한 후 지정한 파이썬 프로그램을 바로 실행하는 방법에 대해 서술하고 있습니다.

## 1) 부팅 시 동작할 서비스에 대한 스크립트 작성
먼저 AMK 부팅시, 작성한 프로그램 데몬이 OS에 상주하기 위해 필요한 서비스 파일을 만들어야 합니다.
아래 명령을 통해 서비스 스크립트 파일을 작성합니다.
sudo nano /lib/systemd/system/myscript.service

>myscript.service 파일 내용
>#! /bin/sh
>
>[Unit] 
>Description=My Script Service
>After=multi-user.target
>
>[Service]
>Type=idle
>ExecStart=/usr/bin/python /home/pi/ai-makers-kit/python3/button.py
>
>[Install]
>WantedBy=multi-user.target

## 2) 작성한 서비스 파일을 시스템에 등록
아래 순서로 작성한 서비스 스크립트를 라즈비안 시스템에 등록합니다.

1) 서비스 파일 권한 설정을 644로 변경
아래 명령어를 통해 수행합니다.
sudo chmod 644 /lib/systemd/system/myscript.service

2) 해당 서비스 파일이 시스템 상에 데몬으로 올라가도록 설정
sudo systemctl daemon-reload
sudo systemctl enable myscript.service

3) 재부팅
sudo reboot

4) 상주 중인 서비스 상태 확인 (필요시)
$ sudo systemctl status myscript.service

4번 과정을 통해 작성한 파이썬 프로그램이 OS에 자동으로 실행된 것을 확인할 수 있습니다.
