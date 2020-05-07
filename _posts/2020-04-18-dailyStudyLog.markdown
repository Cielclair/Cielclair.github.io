---
layout: post
title: 学习日报
---

### 4/18

#### 学习目标
- [x] 建立GitHub pages
> 运行 `bundle exec jekyll serve` 在 [http://127.0.0.1:4000](http://127.0.0.1:4000) 监听
- [x] 字体图标
- [ ] 页面布局

#### 遇到的问题 & 如何解决
- 如何将在Jekyll建立的静态页面关联到GitHub pages
> Gemfile
>> p.s. 如果没有打开vpn，将source改成 https://gems.ruby-china.com
- 内边框的实现
> 用伪元素 `::before` `::after`
> ```CSS
content: '';
position: absolute; # 父级元素需要relative
```
> css3 中，为了与伪类区分，伪元素前应该使用两个冒号，即 `:hover` 伪类，`::before` 伪元素。当然为了向下兼容，用一个冒号也是可以的，不过建议尽量使用规范的写法。
>> [参考](http://www.alloyteam.com/2015/04/beforeafter%e4%bc%aa%e5%85%83%e7%b4%a0%e5%a6%99%e7%94%a8/)

#### 其他
<!-- - 刻意练习 -->
<!-- > 基因限制？更专注 有信心 -->

<!-- - 分享： -->
<!-- > 区块链 -->
 <!-- 挖矿：用显卡计算账本挖出比特币售卖 -->

 <!-- 图灵完备 -->
----

### 4/19

#### 学习目标
- [ ] 页面布局

#### 遇到的问题 & 如何解决

#### 其他
