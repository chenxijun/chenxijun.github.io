---
layout: mypost
title: 记录一次Github Pages的域名更换
categories: [技术,杂谈,维护]
---

## 起因

自从搭建了这个个人博客，上面发文章都是自娱自乐，百度上找不到，也不能帮助别人。问题就出现在这里，为什么百度找不到。

![百度找不到](site-not-found.png)

经过搜索得知，百度蜘蛛违反了`robot.txt`规则，Github Pages全面禁止百度爬虫访问。完了，要折腾了。

## 选择服务和域名

网上的解决方案有两个，一是自己搭建VPS为百度蜘蛛反向代理自己的Github Pages，可是我都有VPS了还要Github Pages干啥；二是迁移到Gitee或是Coding的Pages服务上，好嘛，白嫖就选它了，我选了Gitee码云（后面迁入Coding）。

不过乘此机会，正好给自己置办一个域名，用DNS分流国内外，国内流向Gitee，国外流向Github，嘿嘿，计划通。域名这事呢，毕竟是高中生，国内购买域名还不能方便地备案，所以决定继续白嫖，选择了Freenom的免费域名服务，并选定了网站：[dustsblog.tk](https://dustblog.tk)。至于DNS，我白嫖了DNSPod的免费套餐。（超级白嫖狗🐕）

## 重命名仓库并搬家

Github仓库重命名为[dustsblog](https://github.com/chenxijun/dustsblog/)，更改CNAME，仓库导入Gitee，开启Pages，发现不能更改CNAME遂选择迁入Coding，调整CNAME。

对DNSPod进行调整，分境内外访问，快速便捷。

![重命名](rename-repo.png)

![CNAME](bind-domain.png)



## 遇到问题

### Freenom域名注册时的问题

- 域名选择时，点哪一个都是不可用：要登录
- 注册完毕后提示因技术原因被取消：要墙壁外面的IP，和外面对应的正确地址
- 感觉不稳定，随时会被停用

但是至少注册成功了

![域名](my-domanin.png)

## TODO：

家里通了IPv6，不如用树莓派搭建一个镜像站？