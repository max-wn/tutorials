---
title: Git basis
category: Git
layout:
updated:
---

## git configuration

```bash
git config --global user.name "name"    # add user name
git config --global user.email "email"  # add user e-mail
git config -l                           #  check config
```

## linux config file:

`/home/myuser/.gitconfig`

## windows config file:

`C:\Users\myuser\.gitconfig`

## basic

```bash
git init .                         # generation data base git in dir
git status                         # check the status
git add -A                         # add all files to staging
git commit -m "my initial commit"  # initial commit
git remote -v                      # see projects
```

## file operations

```bash
git rm --cached [file_name]  # удалить файл из репозитория гит
rm -fr .git                  # полностью удалить директорию гит
touch .gitignore             # игнорировать --> туда вписать файл, директорию,
                             # расширение и т д
git checkout -- file.txt     # откатывает назад изменения в файле
```

## clone, push, pull

```bash
git clone 'link in project'               # downloud in copmuter
git push origin master                    # upload master in GitHub remote
git push --set-upstream origin fix_error  # push and generation branch in github
git push origin --delete fix_error        # delete global branch
```

## ssh

```bash
ssh-keygen                          # key generation
git remote -v                       # cheking the link
git remote set-url origin link SSH  # remove link
```

## commit and reset

```bash
git checkout [hash]      # return to a diferent hash
git commit --amend       # chenge last commit
git reset --hard HEAD~2  # delete all version 2 commits back
git reset --soft HEAD~2  # delete all commit log 2 commits back
git reset [file]         # remove from the done position
```

---

THE END
