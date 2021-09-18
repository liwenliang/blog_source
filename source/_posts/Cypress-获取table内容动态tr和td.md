---
title: Cypress 获取table内容动态tr和td
tags:
  - Web开发
originContent: ''
categories:
  - 自动化测试
toc: false
date: 2021-09-17 13:09:32
---

前言
如何获取table内容动态tr和td的dom对象，今天给大家总结了方法。

获取table中DOM元素方法总结
```html
<table id="projectstatus" class="sortable pane bigtable stripped-odd"><tbody><tr class="header"><th tooltip="上次构建状态" title="上次构建状态"><a href="#" class="sortheader">&nbsp;&nbsp;S<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th><th tooltip="构建晴雨表" title="构建晴雨表"><a href="#" class="sortheader">&nbsp;&nbsp;W<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th><th initialsortdir="down"><a href="#" class="sortheader">名称<span class="sortarrow">&nbsp;&nbsp;↓</span></a></th><th><a href="#" class="sortheader">上次成功<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th><th><a href="#" class="sortheader">上次失败<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th><th><a href="#" class="sortheader">上次持续时间<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th><th width="1"><a href="#" class="sortheader">&nbsp;<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th><th><a href="#" class="sortheader">&nbsp;<span class="sortarrow">&nbsp;&nbsp;&nbsp;</span></a></th></tr><tr id="job_FCA玛莎拉蒂自动化测试" class=" job-status-blue"><td data="4"><img src="/static/ecc2f937/images/24x24/blue.png" alt="成功" tooltip="成功" style="width: 24px; height: 24px; " class="icon-blue icon-md" title="成功"></td><td data="100" class="healthReport" onmouseover="this.className='healthReport hover';return true;
        " onmouseout="this.className='healthReport';return true;"><a href="job/FCA%E7%8E%9B%E8%8E%8E%E6%8B%89%E8%92%82%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95/lastBuild" class="build-health-link"><img src="/static/ecc2f937/images/24x24/health-80plus.png" alt="100%" style="width: 24px; height: 24px; " class="icon-health-80plus icon-md"></a><div class="healthReportDetails"><table border="0"><thead><tr><th align="left">W</th><th align="left">描述</th><th align="right">%</th></tr></thead><tbody><tr><td align="left"><img src="/static/ecc2f937/images/16x16/health-80plus.png" style="width: 16px; height: 16px; " class="icon-health-80plus icon-sm"></td><td>构建稳定性: 最近没有失败的构建。</td><td align="right">100</td></tr></tbody></table></div></td><td><a href="job/FCA%E7%8E%9B%E8%8E%8E%E6%8B%89%E8%92%82%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95/" class="model-link inside">1</a></td><td data="2020-07-17T02:00:14Z">
        2 小时 56 分
        - <a href="job/FCA%E7%8E%9B%E8%8E%8E%E6%8B%89%E8%92%82%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95/lastSuccessfulBuild/" class="model-link inside">#26</a></td><td data="2020-07-15T01:48:39Z">
        2 天 3 小时
        - <a href="job/FCA%E7%8E%9B%E8%8E%8E%E6%8B%89%E8%92%82%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95/lastFailedBuild/" class="model-link inside">#7</a></td><td data="1767243">29 分</td><td><a href="job/FCA%E7%8E%9B%E8%8E%8E%E6%8B%89%E8%92%82%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95/build?delay=0sec"><img src="/static/ecc2f937/images/24x24/clock.png" onclick="return build_id2549(this)" alt="Schedule a 构建 for1" style="width: 24px; height: 24px; " title="Schedule a 构建 for FCA玛莎拉蒂自动化测试" class="icon-clock icon-md"></a><script>function build_id2549(img) {
                  new Ajax.Request(img.parentNode.href);
                  hoverNotification('构建 scheduled', img, -100);
                  return false;
                }</script></td><td>&nbsp;</td></tr><tr id="job_2" class=" job-status-blue"><td data="4"><img src="/static/ecc2f937/images/24x24/blue.png" alt="成功" tooltip="成功" style="width: 24px; height: 24px; " class="icon-blue icon-md" title="成功"></td><td data="40" class="healthReport" onmouseover="this.className='healthReport hover';return true;
        " onmouseout="this.className='healthReport';return true;"><a href="job/%E4%B8%80%E6%B1%BD%E6%95%B0%E6%8D%AE%E8%BF%90%E8%90%A5%E6%8E%A5%E5%8F%A3/lastBuild" class="build-health-link"><img src="/static/ecc2f937/images/24x24/health-20to39.png" alt="40%" style="width: 24px; height: 24px; " class="icon-health-20to39 icon-md"></a><div class="healthReportDetails"><table border="0"><thead><tr><th align="left">W</th><th align="left">描述</th><th align="right">%</th></tr></thead><tbody><tr><td align="left"><img src="/static/ecc2f937/images/16x16/health-20to39.png" style="width: 16px; height: 16px; " class="icon-health-20to39 icon-sm"></td><td>构建稳定性: 最近 5 次构建中有 3 次失败。</td><td align="right">40</td></tr></tbody></table></div></td><td><a href="job/%E4%B8%80%E6%B1%BD%E6%95%B0%E6%8D%AE%E8%BF%90%E8%90%A5%E6%8E%A5%E5%8F%A3/" class="model-link inside">2</a></td><td data="2020-06-12T02:16:03Z">
        1 月 5 天
        - <a href="job/%E4%B8%80%E6%B1%BD%E6%95%B0%E6%8D%AE%E8%BF%90%E8%90%A5%E6%8E%A5%E5%8F%A3/lastSuccessfulBuild/" class="model-link inside">#16</a></td><td data="2020-06-12T01:58:43Z">
        1 月 5 天
        - <a href="job/%E4%B8%80%E6%B1%BD%E6%95%B0%E6%8D%AE%E8%BF%90%E8%90%A5%E6%8E%A5%E5%8F%A3/lastFailedBuild/" class="model-link inside">#14</a></td><td data="18773">18 秒</td><td><a href="job/%E4%B8%80%E6%B1%BD%E6%95%B0%E6%8D%AE%E8%BF%90%E8%90%A5%E6%8E%A5%E5%8F%A3/build?delay=0sec"><img src="/static/ecc2f937/images/24x24/clock.png" onclick="return build_id2550(this)" alt="Schedule a 构建 for 一汽数据运营接口" style="width: 24px; height: 24px; " title="Schedule a 构建 for 一2" class="icon-clock icon-md"></a><script>function build_id2550(img) {
                  new Ajax.Request(img.parentNode.href);
                  hoverNotification('构建 scheduled', img, -100);
                  return false;
                }</script></td><td>&nbsp;</td></tr><tr id="job_3" class=" job-status-blue"><td data="4"><img src="/static/ecc2f937/images/24x24/blue.png" alt="成功" tooltip="成功" style="width: 24px; height: 24px; " class="icon-blue icon-md" title="成功"></td><td data="100" class="healthReport" onmouseover="this.className='healthReport hover';return true;
        " onmouseout="this.className='healthReport';return true;"><a href="job/%E9%95%BF%E5%9F%8E%E8%BD%A6%E4%B8%BB%E5%8D%A1%E4%B8%8A%E6%8A%A5%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%8F%A3/lastBuild" class="build-health-link"><img src="/static/ecc2f937/images/24x24/health-80plus.png" alt="100%" style="width: 24px; height: 24px; " class="icon-health-80plus icon-md"></a><div class="healthReportDetails"><table border="0"><thead><tr><th align="left">W</th><th align="left">描述</th><th align="right">%</th></tr></thead><tbody><tr><td align="left"><img src="/static/ecc2f937/images/16x16/health-80plus.png" style="width: 16px; height: 16px; " class="icon-health-80plus icon-sm"></td><td>构建稳定性: 最近没有失败的构建。</td><td align="right">100</td></tr></tbody></table></div></td><td><a href="job/%E9%95%BF%E5%9F%8E%E8%BD%A6%E4%B8%BB%E5%8D%A1%E4%B8%8A%E6%8A%A5%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%8F%A3/" class="model-link inside">3</a></td><td data="2020-07-17T02:30:49Z">
        2 小时 25 分
        - <a href="job/%E9%95%BF%E5%9F%8E%E8%BD%A6%E4%B8%BB%E5%8D%A1%E4%B8%8A%E6%8A%A5%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%8F%A3/lastSuccessfulBuild/" class="model-link inside">#55</a></td><td data="2020-07-14T08:29:55Z">
        2 天 20 小时
        - <a href="job/%E9%95%BF%E5%9F%8E%E8%BD%A6%E4%B8%BB%E5%8D%A1%E4%B8%8A%E6%8A%A5%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%8F%A3/lastFailedBuild/" class="model-link inside">#50</a></td><td data="26883">26 秒</td><td><a href="job/%E9%95%BF%E5%9F%8E%E8%BD%A6%E4%B8%BB%E5%8D%A1%E4%B8%8A%E6%8A%A5%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%8F%A3/build?delay=0sec"><img src="/static/ecc2f937/images/24x24/clock.png" onclick="return build_id2551(this)" alt="Schedule a 构建 for 3" style="width: 24px; height: 24px; " title="Schedule a 构建 for3" class="icon-clock icon-md"></a><script>function build_id2551(img) {
                  new Ajax.Request(img.parentNode.href);
                  hoverNotification('构建 scheduled', img, -100);
                  return false;
                }</script></td><td>&nbsp;</td></tr></tbody></table>
```
![image.png](https://blogimage.houjiyi.com/Frp6LrxgnFrDQPbDrjYWfNkruSwW)
1.获取动态tr个数
```javascript
describe('处理table方法总结', function () {
    beforeEach(function () {
        var username = "admin"
        var password = ""
        //登陆地址
        cy.visit('http://xx.xx.xxx.xx:xxxx/login?from=%2F')
        cy.get('#j_username').type(username)
        cy.get('body > div > div > form > div:nth-child(2) > input').type(password)
        cy.get('body > div > div > form > div.submit.formRow > input').click()

    })
    it('动态获取tr', function () {
        cy.get('#projectstatus > tbody >tr').then(($el) => {
            const tr_total = $el.length
            cy.log(tr_total)
        })
    });})
```
![image.png](https://blogimage.houjiyi.com/FilWXH4zt8W2n0a29C72zQb8eJpQ)
2.获取动态tr去断言td内容

```javascript
describe('处理table方法总结', function () {
    beforeEach(function () {
        var username = "admin"
        var password = ""
        //登陆地址
        cy.visit('http://xx.xx.xxx.xx:xxxx/login?from=%2F')
        cy.get('#j_username').type(username)
        cy.get('body > div > div > form > div:nth-child(2) > input').type(password)
        cy.get('body > div > div > form > div.submit.formRow > input').click()

    })
    it('动态获取tr', function () {
        cy.get('#projectstatus > tbody >tr').then(($el) => {
            const tr_total = $el.length
            cy.log(tr_total)
            var list =['29 分','18 秒',"26 秒"]
             for (var i = 1; i < tr_total; i++) {
                cy.get('#projectstatus > tbody >tr').eq(i).find('td:nth-child(6)')
                    .should('contain', list[i-1])
            }
        })
    });})
```
![image.png](https://blogimage.houjiyi.com/Fo_OZgN9gABXL9Ne5UfJd_dk9vuX)

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家的支持。

更多文章关注小编公众号：自动化测试 To share

