---
title: mysql 批量
tags: 数据批量
categories: 数据库
date: 2018-03-20 20:35:16
preview: https://sandbox.runjs.cn/uploads/rs/247/l96nyyh9/mysql-logo.svg
subtitle: mysql 批量value插入 批量update case when then
---
<div id="cnblogs_post_body" class="blogpost-body"><p><span style="font-family: 隶书">insert into tableName(type1,type2,type3) values (“q1”,”w1”,”e1”),( “q2”,”w2”,”e2”) ,( “q3”,”w3”,”e3”)；</span></p>
<p><span style="font-family: 隶书"><span lang="EN-US">&nbsp;<span style="color: #ff9900"><strong>UPDATE app_history_statistics_by_hour SET h00 = <span style="color: #00ff00">CASE</span> <span style="color: #800080">statisticalType</span> <span style="color: #00ff00">WHEN</span> "qqq" <span style="color: #00ff00">THEN</span> 11111 WHEN "www" THEN 22211 WHEN 'eee' THEN 333111 ELSE 0 END <span style="color: #993366">WHERE </span>weeks='222';</strong></span></span></span></p>
<p><span style="font-family: 隶书"><span lang="EN-US"><span style="color: #ff9900"><strong>由于表结构有限，所以没能用实体一一对应，所以只能用hql</strong></span></span></span></p>
<p><span style="font-family: 隶书"><span lang="EN-US"><span style="color: #ff9900"><strong>hibernate &nbsp;hql &nbsp;批量插入 没找到固有的方法，所以 用以上方法，速度提升很多</strong></span></span></span></p></div>