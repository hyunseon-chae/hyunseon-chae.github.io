---
layout: post
title:  "우분투 VNC 서버 설치 및 접속"
categories: GAN
---






## 우분투 내 VNC 설치

```
ubuntu@ip-(ip주소):~$sudo apt-get update
ubuntu@ip-(ip주소):~$sudo apt-get install ubuntu-desktop
ubuntu@ip-(ip주소):~$sudo apt-get install vnc4server
ubuntu@ip-(ip주소):~$sudo apt-get install gnome-panel
```



## tightVNC 프로그램 설치

1. TightVNC 설치 및 실행
2. Connection - Remote Host: 에 접속할 PC의 IP 입력
   - 입력 IP : 인스턴스의 public ipv4 주소 
   - IP 접근 제어는 EC2 생성할 때 입력한 보안그룹에 따라 결정
   - 따라서 보안 그룹 편집 필요
     - 인바운드 규칙 추가 :  포트 범위(5901), 프로토콜(tcp), 원본(본인 PC IP)



##  tightVNC를 이용한 GUI 원격 접속



###### 1. xstartup 파일 수정

```
ubuntu@ip-(ip주소):~$ vnc4server

ubuntu@ip-(ip주소):~$ vi ~/.vnc/xstartup
```



아래 내용으로 변경한다.

```
#!/bin/sh 
# Uncomment the following two lines for normal desktop: 
# unset SESSION_MANAGER 
# exec /etc/X11/xinit/xinitrc 
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup 
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources 
xsetroot -solid grey 
vncconfig -iconic & 
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" & 
x-window-manager & 
exec /usr/bin/startxfce4 &

#!/bin/sh 
autocutsel -fork 
xrdb $HOME/.Xresources 
xsetroot -solid grey 
export XKL_XMODMAP_DISABLE=1 
export XDG_CURRENT_DESKTOP="GNOME-Flashback:Unity" 
export XDG_MENU_PREFIX="gnome-flashback-" 
unset DBUS_SESSION_BUS_ADDRESS 
gnome-session --session=gnome-flashback-metacity --disable-acceleration-check --debug &
```



###### 2. vncserver 재실행 & vnc 접속을 위한 pwd 설정

```
ubuntu@ip-(ip주소):~$ vnc4server

//pwd 설정하면 자동으로 실행이 됩니다.
```



###### 3. tightVNC 로 접속

- tightVNC 실행
- Connection - Remote Host: 에 접속할 PC의 IP 입력
- 위에서 설정한 pwd 입력
- 접속 성공 확인

