---
author: admin
comments: true
date: 2010-01-13 15:36:39
layout: post
slug: about-testng-dependsonmethods-and-dependsongroups
title: About TestNG DependsOnMethods and DependsOnGroups
wordpress_id: 338
categories:
- TestNG
tags:
- DependsOnGroups
- DependsOnMethods
- TestNG
---

TestNG默认测试执行顺序是随机执行的，这显然不符合天朝和谐的国情，还好有DependsOnMethods 和DependsOnGroups两个定义执行顺序的Annotation，但是偶最近发现一个不知道是不是Bug的问题：



	
  * **Hard dependencies**. All the methods you depend on must have run and succeeded for you to run. If at least one failure occurred in your dependencies, you will not be invoked and marked as a SKIP in the report.

	
  * **Soft dependencies**. You will always be run after the methods you depend on, even if some of them have failed. This is useful when you just want to make sure that your test methods are run in a certain order but their success doesn't really depend on the success of others. A soft dependency is obtained by adding "alwaysRun=true" in your @Test annotation.


以上是TestNG对于排序的官方解释，DependsOnMethods是硬的，DependsOnGroups是软的。

我定义的测试用例执行顺序如下：

`








`

`
`

`
public class TestInitializeDate {
@Test(groups={"Initialize"})
public void TestInitializeDate() {}}`

`public class TestProfilesAdd{
@Test(groups={"Profiles"},dependsOnMethods={"TestInitializeDate "})
public void TestProfilesAdd() {}}

public class TestProfilesSearchAndEdit{
@Test(groups={"Profiles"},dependsOnMethods={"TestProfilesAdd"})
public void TestProfilesSearchAndEdit() {}}

public class TestDeleteProfiles{
@Test(groups={"Profiles"},dependsOnMethods={"TestProfilesSearchAndEdit"})
public void TestDeleteProfiles() {}}

public class TestDeleteInitializeDate{
@Test(groups={"Profiles"},dependsOnMethods={"TestDeleteProfiles"})
public void TestDeleteInitializeDate() {}}

`

结果只有初始化的TestInitializeDate 能够执行，其他测试用例全部SKIP

如果将TestProfilesAdd的Depends方法改成dependsOnGroups={"Initialize"}，就能按照预定顺序执行

奇怪了，这个算BUG么？
