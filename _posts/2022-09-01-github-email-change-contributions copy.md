---
layout: post
title: 'GitHub email 变更同步 contributions'
date: 2022-09-01
categories: jekyll update
---

## 变更

> 命令行运行

```shell
git filter-branch --env-filter '

OLD_EMAIL="old_email@example.com"
CORRECT_NAME="correct_name"
CORRECT_EMAIL="correct_email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

## 提交
```shell
git push --force
```
