---
title: 如何删除GitHub中的所有提交历史记录？
date: 2021-04-06 19:37:19
tags: gitHub的使用
categories: gitHub的使用
# categories是控制博客分类的目录
---
删除.git文件夹可能会导致git存储库中的问题
如果要删除所有提交历史记录，但将代码保持在当前状态，可以按照以下方式安全地执行此操作：
<!-- more -->
``` JS
git checkout --orphan latest_branch // 尝试  运行  
git add -A // 添加所有文件
git commit -am "commit message" // 提交更改
git branch -D master // 删除分支
git branch -m master // 将当前分支重命名
git push -f origin master // 最后，强制更新存储库
```