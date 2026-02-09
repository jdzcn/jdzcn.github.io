---
layout: post
title: debian使用socks5代理安装chrome/brave浏览器
tags: [linux]
---

### Brave
```shell
sudo apt install apt-transport-https curl

sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg --socks5-hostname localhost:1080

echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=amd64] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list

sudo apt-get -o Acquire::http::proxy="socks5h://127.0.0.1:1080/" update

sudo apt-get -o Acquire::http::proxy="socks5h://127.0.0.1:1080/" install brave-browser
```

### chrome
```shell
wget -qO - https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /usr/share/keyrings/googlechrome-linux-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/googlechrome-linux-keyring.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
sudo apt update
sudo apt install -y google-chrome-stable
```

### Reference
- [Installing Brave on Linux](https://brave.com/linux/#linux)