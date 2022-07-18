---
title:  "wsl 사용시 systemctl 명령어 먹히지 않을때"
excerpt: "System has not been booted with systemd as init system (PID 1). Can't operate."

categories: tips
tags:
  - [wsl, ubuntu, docker, systemctl]

toc: true
toc_sticky: true
 
date: 2022-07-18-00:02
last_modified_at: 2022-07-18-00:03
---

#### wsl 사용시 systemctl 명령어 먹히지 않을때

아래와 같이 systemctl 명령어가 먹히지 않을 때
![](../../assets/images/20220719-002434.png){: .center}

System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down


아래 두 명령어를 차례로 입력해주면 된다.

```bash
sudo -b unshare --pid --fork --mount-proc /lib/systemd/systemd --system-unit=basic.target
sudo -E nsenter --all -t $(pgrep -xo systemd) runuser -P -l $USER -c "exec $SHELL"
```

