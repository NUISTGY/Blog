---
title:  Git工作区中文件状态总结
tags: [git]
categories: [git]
date: 2022-9-16

---

# 总结

Git工作区中文件的状态👇

Git工作区中的文件存在两种状态:

- untracked未跟踪（未被纳入版本控制)
- tracked已跟踪（被纳入版本控制)
  - 1.Unmodified 未修改状态
  - 2.Modified 已修改状态
  - 3.Staged 已暂存状态

# 详细说明

**注⚠：使用git status查看文件状态**

## untracker

设想：此时在.git所在目录中新建文件Hello.txt
则：此时查看文件Hello.txt状态为 <font color="#660000">untracker</font> 

## tracker

### Staged

设想：此时对Hello.txt文件进行git add. 操作
则：此时查看文件Hello.txt状态为 <font color="#006600">Staged</font>

### Unmodified

设想：此时对Hello.txt文件进行git commit 操作
则：此时查看文件Hello.txt状态为 Unmodified，注意此时调用状态查看结果并不显示

### Modified

设想：此时对Hello.txt文件内容进行修改操作
则：此时查看文件Hello.txt状态为 <font color="#660000">Modified</font> (未放入缓存区)

紧接着：再次执行git add. 操作
则：此时查看文件Hello.txt状态为 <font color="#006600">Modified</font> (已归入缓存区)