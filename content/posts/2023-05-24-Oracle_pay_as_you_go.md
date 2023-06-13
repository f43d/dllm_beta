---
title: Oracle Pay as you Go
date: 2023-05-24
description: "終於開到 Oracle Pay as go account"
tags:
  - Oracle
  - VPS
  - FreshRSS
  - Vaultwarden
categories: Site部大開發
weight: 0
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

話說四月份去北京，為咗可以順利牆外上網，所以除咗自己屋企用嘅[PiVPN](https://www.pivpn.io/) 之外，仲特別開多個 VPN service 以防萬一。

誰不知誤打誤撞之下，一時大意唔小心 terminate 了早前開的Oracle 4x24 arm server。 北京返來仲以為再重開好了，結果唔係真係唔係次次都咁好運，試咗好幾日一直都係都係以下臭名昭著既 error:
> Out of capacity for shape VM.Standard.A1.Flex in availability domain AD-1. Create the instance in a different availability domain or try again later. If you specified a fault domain, try creating the instance without specifying a fault domain, otherwise try creating the instance in a different availability domain. If that doesn’t work, please try again later.

或者人就係咁啦，其實我申請個 server 返嚟都唔知有咩嘢用，但係越申請唔到就越係想用旁門左道嘅方式達到目的。首先發現原來 [Mac上有個 app](https://apps.apple.com/us/app/rapidclick/id419891002?mt=12) 可以每隔若干時間自動 click 一次，而且佢仲會記得 dialogue box 上 submit button 既relative position！於是可以 set 到每隔三四十秒試一次 create instance。不過試左一輪原來 Oracle 大概會係三個鐘頭左右自動log out。 試咗一個星期左右，完全冇成效。
當然網上亦有[好多教程](https://hitrov.medium.com/resolving-oracle-cloud-out-of-capacity-issue-and-getting-free-vps-with-4-arm-cores-24gb-of-a3d7e6a027a8)[寫個script](https://blog.user.today/oracle-cloud-free-tier-cron/) 自動去 create instance. 不過對我呢種技術小白嚟講門檻實在太高，都係搞唔掂。

其實舊年11月，Oracle [已經講咗](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm) 用會將閑置嘅 Always free Tier 服務注銷。當然你有政策[我有對策](https://www.youtube.com/watch?v=nEwGDrocrg4)! 有陣時諗真，用現有既VPS run 個 script 去搶免費VPS， 搶到之後又冇真正嘅用途，所以又要再另外 run 個 script 做啲無謂嘢，扮到個 server 好忙咁哋咁等人哋唔會取消服務，其實真係唔知乜嘢玩法。講穿咗可能就係貪小便宜同埋以為可以薅大公司羊毛好似好勁咁囉。

咁都有正路既方法，就係 upgrade 去 Pay as you go 嘅 tier。 既然係課金，咁就可以開任何嘅服務，按使用量收費。如果開嘅服務唔超過 always free tier大， 咁理論上係唔會產生任何嘅費用。

不過 upgrade去 pay as you go 又係另一個惡夢！先後試過好幾張唔同嘅信用卡，但係最後都係未能夠成功通過驗證。
網上對於點樣申請 pay as you go account 就覺得[更加係玄學](https://www.v2ex.com/t/925595)。有人話要用 VPN， 有人話要用銀聯卡。有人喺牆內大陸信用卡成功申請，亦有肉身喺海外，外幣信用咭都一樣申請唔到。

話說有一晚又試咗幾次之後都唔成功， 之後佢建議聯絡CS。勢估唔到 Oracle竟然有 Online CS live Chat。於是我向一肚苦水向CS求救。

Oracle 描述查詢問題只能夠打 240 個 characters，所以我邊打邊改洋洋灑灑打咗以下呢一堆嘢，剛好239個 characters：

> Dear Oracle,
>
> I've tried to upgrade to pay as you go.
> I've tried 3 credit cards I'm using at daily,  none of them went through.
> Using ACCURATE info, NO VPN, NO means to trick you, NOTHING! Happy user for a year, just want to upgrade.
> HELP!


可能孝感動天，CS問我試過用乜嘢信用卡申請，我如實告知。跟住佢話你一直用緊 always free 好地地，點解突然間想轉 pay as you go 呢？

> "因為我唔小心 terminate 咗我個4×24 arm server 呀屌！"
> "我試咗成個月啦，一直都開唔番呀屌！"
> "我真係唔介意俾錢[至奇]呀！我只係想開個 server 來做正經 project 呀！"

可能CS見我咁有誠意，於是佢話會 review 我個 case，叫我等 email。

三日之後，各位觀眾，我成功開咗 Pay as you go 啦！

而我亦都好順利咁樣開到個 4×24 嘅 arm instance !

我喺新 Server 上面安裝咗 [Docker](https://www.docker.com/) 同 [Portainer](https://www.portainer.io/)， 希望真係會正正經經將我而家用緊嘅 service 好似 [FreshRSS](https://www.freshrss.org/) ， [Vaultwarden](https://github.com/dani-garcia/vaultwarden) 等等服搬去呢個新 server上邊。

我再三睇過 [Oracle 既 terms](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm)， 如果只係用 always free tier 嘅服務，其實真係唔會產生任何嘅費用。

如果大家仍然擔心嘅話，其實可以喺 budget 試試試上 set個提示，當 Oracle project 到收費去到你 budget 嘅若干百分比就可以自動提醒你，咁就唔怕無啦啦比人收左一大筆錢了。


