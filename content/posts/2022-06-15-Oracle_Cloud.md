---
title: 薅甲骨文4核24g羊毛
date: 2022-06-20T03:13:44.555Z
categories: Site部大開發
weight: 0
tags: ["Oracle" "VPS"]
cover:
  hidden: false
  relative: false
showToc: false
TocOpen: false
hidemeta: false
comments: false
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: false
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowWordCount: false
ShowRssButtonInSectionTermList: false
UseHugoToc: false
---

有玩小雞的都知道甲骨文4核24G羊毛不好薅，基本上都是

` Error : Out of capacity for shape VM.Standard.A1.Flex in availability domain`

今天不知行了什麼狗屎運，竟然給我了個 **4核24G內存**的 Arm 免費雲，重點**完全沒有**使用[旁門左道腳本](https://bestofphp.com/repo/hitrov-oci-arm-host-capacity)下成功開啓!
其實去年 Black Friday 買的兩個VPS 也沒有怎樣使用，只是美帝羊毛當前大家都起哄先薅為快。
原先一早已開啓的 **E2.1.Micro instance** 簡單安裝了個 Wireguard VPN，用了好幾個月一直相安無事。

這個 效能強勁的Arm VPS 到底打算怎樣用還沒有主意， LowendBox上剛好有介紹新開VPS的[保安](https://lowendbox.com/blog/how-to-begin-on-your-new-vps-or-dedicated-server/)和[測試](https://lowendbox.com/blog/how-to-use-yabs-to-check-your-new-vps-or-dedicated-server/) 流程 , 正好學習一下

甲骨文預設使用SSH Key登陸 ，想起早前看過 SSH Certificate 的[正確打開方法](https://smallstep.com/blog/use-ssh-certificates/) ，Reddit 上亦有討論過是否要將 SSH Private Key [放在 BitWarden Vault](https://www.reddit.com/r/Bitwarden/comments/mr0mrh/storing_ssh_keys/)。說到 SSH Client，我的主力機是Mac和iPad ，[Termius](termius.com) 當然是首選，，一直想找開源的替代品，最好是能夠在不同 device 上同步設定，可惜至今未有着落。

從來對[BT (aapanel)](https://www.aapanel.com/new/index.html)都無愛， 於是安裝了 [Hestia](https://www.hestiacp.com/) ， 我另一個 VPS 上用 [CapRover](https://caprover.com/) 感覺比 [YunoHost](https://yunohost.org/) 要好得多，(我也不想 [DD Debian](https://blog.csdn.net/MinLearn/article/details/112477367)  怕被甲骨文封號， 下一步可能會安裝CapRover 或者直接上Docker。
