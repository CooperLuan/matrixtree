Title: mutt/msmtp
Date: 2014-03-09 10:30:00
Category: 运维
Tags: mutt/msmtp/ubuntu
Author: Cooper
Summary: 用 mutt/msmtp 客户端发送邮件

## 安装

    :::sh
    apt-get update
    apt-get install mutt msmtp
    cd ~/
    # 新建配置文件
    touch .msmtprc

## 配置 msmtp

    :::text
    account default
    host smtp.exmail.com
    from you@example.com
    auth plain
    user you@example.com
    password you\ know

## 配置 mutt

    :::text
    set sendmail="/usr/bin/msmtp"
    set from=you@example.com

## 发送邮件

    :::sh
    echo "Oh my zsh" | mutt -s "ZIP zsh from mutt/msmtp" -e 'my_hdr From:you@example.com' -- you@example.com –
