<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [github-gitee](#github-gitee)
- [:point_right:git 提交规范](#point_rightgit-%E6%8F%90%E4%BA%A4%E8%A7%84%E8%8C%83)
- [系统设置相关](#%E7%B3%BB%E7%BB%9F%E8%AE%BE%E7%BD%AE%E7%9B%B8%E5%85%B3)
  - [:point_right:git config - 获取和设置配置项](#point_rightgit-config---%E8%8E%B7%E5%8F%96%E5%92%8C%E8%AE%BE%E7%BD%AE%E9%85%8D%E7%BD%AE%E9%A1%B9)
  - [git help - 帮助命令](#git-help---%E5%B8%AE%E5%8A%A9%E5%91%BD%E4%BB%A4)
- [获取和创建项目](#%E8%8E%B7%E5%8F%96%E5%92%8C%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE)
  - [:point_right:git init - 创建空的git项目](#point_rightgit-init---%E5%88%9B%E5%BB%BA%E7%A9%BA%E7%9A%84git%E9%A1%B9%E7%9B%AE)
  - [:point_right:git clone - 将存储库克隆到新目录中](#point_rightgit-clone---%E5%B0%86%E5%AD%98%E5%82%A8%E5%BA%93%E5%85%8B%E9%9A%86%E5%88%B0%E6%96%B0%E7%9B%AE%E5%BD%95%E4%B8%AD)
- [基本操作命令](#%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C%E5%91%BD%E4%BB%A4)
  - [:point_right:git add - 将文件内容添加到索引](#point_rightgit-add---%E5%B0%86%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%B7%BB%E5%8A%A0%E5%88%B0%E7%B4%A2%E5%BC%95)
  - [:point_right:git status --  显示工作树状态](#point_rightgit-status-----%E6%98%BE%E7%A4%BA%E5%B7%A5%E4%BD%9C%E6%A0%91%E7%8A%B6%E6%80%81)
  - [:point_right:git diff - 显示提交、提交和工作树等之间的变化](#point_rightgit-diff---%E6%98%BE%E7%A4%BA%E6%8F%90%E4%BA%A4%E6%8F%90%E4%BA%A4%E5%92%8C%E5%B7%A5%E4%BD%9C%E6%A0%91%E7%AD%89%E4%B9%8B%E9%97%B4%E7%9A%84%E5%8F%98%E5%8C%96)
  - [:point_right:git commit - 提交对存储库的更改信息](#point_rightgit-commit---%E6%8F%90%E4%BA%A4%E5%AF%B9%E5%AD%98%E5%82%A8%E5%BA%93%E7%9A%84%E6%9B%B4%E6%94%B9%E4%BF%A1%E6%81%AF)
  - [git notes - 添加或检查对象注释](#git-notes---%E6%B7%BB%E5%8A%A0%E6%88%96%E6%A3%80%E6%9F%A5%E5%AF%B9%E8%B1%A1%E6%B3%A8%E9%87%8A)
  - [:point_right:git restore - 恢复工作树文件](#point_rightgit-restore---%E6%81%A2%E5%A4%8D%E5%B7%A5%E4%BD%9C%E6%A0%91%E6%96%87%E4%BB%B6)
  - [:point_right:git reset - 重置当前HEAD到指定的状态](#point_rightgit-reset---%E9%87%8D%E7%BD%AE%E5%BD%93%E5%89%8Dhead%E5%88%B0%E6%8C%87%E5%AE%9A%E7%9A%84%E7%8A%B6%E6%80%81)
  - [:point_right:git rm - 从工作树和索引中删除文件](#point_rightgit-rm---%E4%BB%8E%E5%B7%A5%E4%BD%9C%E6%A0%91%E5%92%8C%E7%B4%A2%E5%BC%95%E4%B8%AD%E5%88%A0%E9%99%A4%E6%96%87%E4%BB%B6)
  - [:point_right:git mv -  移动或重命名一个文件、一个目录或一个符号链接](#point_rightgit-mv----%E7%A7%BB%E5%8A%A8%E6%88%96%E9%87%8D%E5%91%BD%E5%90%8D%E4%B8%80%E4%B8%AA%E6%96%87%E4%BB%B6%E4%B8%80%E4%B8%AA%E7%9B%AE%E5%BD%95%E6%88%96%E4%B8%80%E4%B8%AA%E7%AC%A6%E5%8F%B7%E9%93%BE%E6%8E%A5)
- [分支与合并](#%E5%88%86%E6%94%AF%E4%B8%8E%E5%90%88%E5%B9%B6)
  - [:point_right:git branch - 列出、创建或删除分支](#point_rightgit-branch---%E5%88%97%E5%87%BA%E5%88%9B%E5%BB%BA%E6%88%96%E5%88%A0%E9%99%A4%E5%88%86%E6%94%AF)
  - [:point_right:git checkout - 切换分支或恢复工作树文件](#point_rightgit-checkout---%E5%88%87%E6%8D%A2%E5%88%86%E6%94%AF%E6%88%96%E6%81%A2%E5%A4%8D%E5%B7%A5%E4%BD%9C%E6%A0%91%E6%96%87%E4%BB%B6)
  - [git switch - 切换分支](#git-switch---%E5%88%87%E6%8D%A2%E5%88%86%E6%94%AF)
  - [:point_right:git merge - 合并两个或以上的分支](#point_rightgit-merge---%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%88%96%E4%BB%A5%E4%B8%8A%E7%9A%84%E5%88%86%E6%94%AF)
  - [git mergetool  - 合并工具](#git-mergetool----%E5%90%88%E5%B9%B6%E5%B7%A5%E5%85%B7)
  - [:point_right:git log - 展示commit信息](#point_rightgit-log---%E5%B1%95%E7%A4%BAcommit%E4%BF%A1%E6%81%AF)
  - [git stash - 将更改存储在脏工作目录中](#git-stash---%E5%B0%86%E6%9B%B4%E6%94%B9%E5%AD%98%E5%82%A8%E5%9C%A8%E8%84%8F%E5%B7%A5%E4%BD%9C%E7%9B%AE%E5%BD%95%E4%B8%AD)
  - [:point_right:git tag - 创建，列出，删除或验证使用GPG签名的标记对象](#point_rightgit-tag---%E5%88%9B%E5%BB%BA%E5%88%97%E5%87%BA%E5%88%A0%E9%99%A4%E6%88%96%E9%AA%8C%E8%AF%81%E4%BD%BF%E7%94%A8gpg%E7%AD%BE%E5%90%8D%E7%9A%84%E6%A0%87%E8%AE%B0%E5%AF%B9%E8%B1%A1)
  - [git worktree - 管理多个工作树](#git-worktree---%E7%AE%A1%E7%90%86%E5%A4%9A%E4%B8%AA%E5%B7%A5%E4%BD%9C%E6%A0%91)
- [共享和更新项目](#%E5%85%B1%E4%BA%AB%E5%92%8C%E6%9B%B4%E6%96%B0%E9%A1%B9%E7%9B%AE)
  - [git fetch - 从远程分支拉取代码](#git-fetch---%E4%BB%8E%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF%E6%8B%89%E5%8F%96%E4%BB%A3%E7%A0%81)
  - [:point_right:git pull - 拉取远端分支并合并](#point_rightgit-pull---%E6%8B%89%E5%8F%96%E8%BF%9C%E7%AB%AF%E5%88%86%E6%94%AF%E5%B9%B6%E5%90%88%E5%B9%B6)
  - [:point_right:git push - 推送到远端分支](#point_rightgit-push---%E6%8E%A8%E9%80%81%E5%88%B0%E8%BF%9C%E7%AB%AF%E5%88%86%E6%94%AF)
  - [:point_right:git remote - 管理远程仓库](#point_rightgit-remote---%E7%AE%A1%E7%90%86%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93)
  - [:point_right:git submodule - 子模块管理](#point_rightgit-submodule---%E5%AD%90%E6%A8%A1%E5%9D%97%E7%AE%A1%E7%90%86)
- [检查比较](#%E6%A3%80%E6%9F%A5%E6%AF%94%E8%BE%83)
  - [:point_right:git show - 显示各种类型的对象](#point_rightgit-show---%E6%98%BE%E7%A4%BA%E5%90%84%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%AF%B9%E8%B1%A1)
  - [:point_right:git log - 显示提交日志](#point_rightgit-log---%E6%98%BE%E7%A4%BA%E6%8F%90%E4%BA%A4%E6%97%A5%E5%BF%97)
  - [:point_right:git diff - 查看文件在工作目录与暂存区的差别](#point_rightgit-diff---%E6%9F%A5%E7%9C%8B%E6%96%87%E4%BB%B6%E5%9C%A8%E5%B7%A5%E4%BD%9C%E7%9B%AE%E5%BD%95%E4%B8%8E%E6%9A%82%E5%AD%98%E5%8C%BA%E7%9A%84%E5%B7%AE%E5%88%AB)
  - [git difftool - 使用常见差异工具在修订之间比较和编辑文件](#git-difftool---%E4%BD%BF%E7%94%A8%E5%B8%B8%E8%A7%81%E5%B7%AE%E5%BC%82%E5%B7%A5%E5%85%B7%E5%9C%A8%E4%BF%AE%E8%AE%A2%E4%B9%8B%E9%97%B4%E6%AF%94%E8%BE%83%E5%92%8C%E7%BC%96%E8%BE%91%E6%96%87%E4%BB%B6)
  - [diff range-diff - 比较两个提交范围](#diff-range-diff---%E6%AF%94%E8%BE%83%E4%B8%A4%E4%B8%AA%E6%8F%90%E4%BA%A4%E8%8C%83%E5%9B%B4)
  - [git shortlog - 总结 git log输出](#git-shortlog---%E6%80%BB%E7%BB%93-git-log%E8%BE%93%E5%87%BA)
  - [git describe - 命令显示离当前提交最近的标签](#git-describe---%E5%91%BD%E4%BB%A4%E6%98%BE%E7%A4%BA%E7%A6%BB%E5%BD%93%E5%89%8D%E6%8F%90%E4%BA%A4%E6%9C%80%E8%BF%91%E7%9A%84%E6%A0%87%E7%AD%BE)
- [补丁](#%E8%A1%A5%E4%B8%81)
  - [git apply - 将补丁应用于文件和/或索引](#git-apply---%E5%B0%86%E8%A1%A5%E4%B8%81%E5%BA%94%E7%94%A8%E4%BA%8E%E6%96%87%E4%BB%B6%E5%92%8C%E6%88%96%E7%B4%A2%E5%BC%95)
  - [:point_right:git cherry-pick - 就是将指定的提交（commit）应用于其他分支](#point_rightgit-cherry-pick---%E5%B0%B1%E6%98%AF%E5%B0%86%E6%8C%87%E5%AE%9A%E7%9A%84%E6%8F%90%E4%BA%A4commit%E5%BA%94%E7%94%A8%E4%BA%8E%E5%85%B6%E4%BB%96%E5%88%86%E6%94%AF)
  - [:point_right:git rebase - 合并变更](#point_rightgit-rebase---%E5%90%88%E5%B9%B6%E5%8F%98%E6%9B%B4)
  - [:point_right:git revert - 用于撤回某次提交的内容，同时再产生一个新的提交(commit)](#point_rightgit-revert---%E7%94%A8%E4%BA%8E%E6%92%A4%E5%9B%9E%E6%9F%90%E6%AC%A1%E6%8F%90%E4%BA%A4%E7%9A%84%E5%86%85%E5%AE%B9%E5%90%8C%E6%97%B6%E5%86%8D%E4%BA%A7%E7%94%9F%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E6%8F%90%E4%BA%A4commit)
- [Debugging调试](#debugging%E8%B0%83%E8%AF%95)
  - [:point_right:git bisect  - 用来查找哪一次提交引入了错误](#point_rightgit-bisect----%E7%94%A8%E6%9D%A5%E6%9F%A5%E6%89%BE%E5%93%AA%E4%B8%80%E6%AC%A1%E6%8F%90%E4%BA%A4%E5%BC%95%E5%85%A5%E4%BA%86%E9%94%99%E8%AF%AF)
  - [:point_right:git blame - 显示修改版本和作者上次修改文件的每一行](#point_rightgit-blame---%E6%98%BE%E7%A4%BA%E4%BF%AE%E6%94%B9%E7%89%88%E6%9C%AC%E5%92%8C%E4%BD%9C%E8%80%85%E4%B8%8A%E6%AC%A1%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%E7%9A%84%E6%AF%8F%E4%B8%80%E8%A1%8C)
  - [:point_right:git grep - 文本搜索](#point_rightgit-grep---%E6%96%87%E6%9C%AC%E6%90%9C%E7%B4%A2)
- [向导](#%E5%90%91%E5%AF%BC)
  - [:point_right:gitattributes - git 属性](#point_rightgitattributes---git-%E5%B1%9E%E6%80%A7)
  - [gitcli  -  Git 命令行界面和约定](#gitcli-----git-%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2%E5%92%8C%E7%BA%A6%E5%AE%9A)
  - [:point_right:giteveryday - Everyday Git的一组有用的最小命令](#point_rightgiteveryday---everyday-git%E7%9A%84%E4%B8%80%E7%BB%84%E6%9C%89%E7%94%A8%E7%9A%84%E6%9C%80%E5%B0%8F%E5%91%BD%E4%BB%A4)
  - [:point_right:git hooks](#point_rightgit-hooks)
  - [:point_right:gitignore - 指定要忽略未跟踪文件](#point_rightgitignore---%E6%8C%87%E5%AE%9A%E8%A6%81%E5%BF%BD%E7%95%A5%E6%9C%AA%E8%B7%9F%E8%B8%AA%E6%96%87%E4%BB%B6)
  - [:point_right:gitmodules -  定义子模块属性](#point_rightgitmodules----%E5%AE%9A%E4%B9%89%E5%AD%90%E6%A8%A1%E5%9D%97%E5%B1%9E%E6%80%A7)
- [Email (TODO)](#email-todo)
  - [git am](#git-am)
  - [git format-patch](#git-format-patch)
  - [git send-email](#git-send-email)
  - [git request-pull](#git-request-pull)
- [外部系统](#%E5%A4%96%E9%83%A8%E7%B3%BB%E7%BB%9F)
  - [git svn 与svn系统之间的操作](#git-svn-%E4%B8%8Esvn%E7%B3%BB%E7%BB%9F%E4%B9%8B%E9%97%B4%E7%9A%84%E6%93%8D%E4%BD%9C)
  - [git fast-import - 快速Git数据导入器的后端](#git-fast-import---%E5%BF%AB%E9%80%9Fgit%E6%95%B0%E6%8D%AE%E5%AF%BC%E5%85%A5%E5%99%A8%E7%9A%84%E5%90%8E%E7%AB%AF)
- [管理](#%E7%AE%A1%E7%90%86)
  - [:point_right:git clean - 从工作树中删除未跟踪的文件](#point_rightgit-clean---%E4%BB%8E%E5%B7%A5%E4%BD%9C%E6%A0%91%E4%B8%AD%E5%88%A0%E9%99%A4%E6%9C%AA%E8%B7%9F%E8%B8%AA%E7%9A%84%E6%96%87%E4%BB%B6)
  - [:point_right:git gc - 清理不必要的文件并优化本地存储库](#point_rightgit-gc---%E6%B8%85%E7%90%86%E4%B8%8D%E5%BF%85%E8%A6%81%E7%9A%84%E6%96%87%E4%BB%B6%E5%B9%B6%E4%BC%98%E5%8C%96%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E5%BA%93)
  - [git fsck -  验证数据库中对象的连通性和有效性](#git-fsck----%E9%AA%8C%E8%AF%81%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%AD%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%BF%9E%E9%80%9A%E6%80%A7%E5%92%8C%E6%9C%89%E6%95%88%E6%80%A7)
  - [git reflog - 管理reflog信息](#git-reflog---%E7%AE%A1%E7%90%86reflog%E4%BF%A1%E6%81%AF)
  - [git filter-branch - 重写分支](#git-filter-branch---%E9%87%8D%E5%86%99%E5%88%86%E6%94%AF)
  - [git instaweb - 在gitweb中](#git-instaweb---%E5%9C%A8gitweb%E4%B8%AD)
  - [git archive - 从命名树创建文件存档](#git-archive---%E4%BB%8E%E5%91%BD%E5%90%8D%E6%A0%91%E5%88%9B%E5%BB%BA%E6%96%87%E4%BB%B6%E5%AD%98%E6%A1%A3)
  - [git bundle - 通过归档移动对象和引用](#git-bundle---%E9%80%9A%E8%BF%87%E5%BD%92%E6%A1%A3%E7%A7%BB%E5%8A%A8%E5%AF%B9%E8%B1%A1%E5%92%8C%E5%BC%95%E7%94%A8)
- [服务器管理](#%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AE%A1%E7%90%86)
  - [git daemon - Git服务器](#git-daemon---git%E6%9C%8D%E5%8A%A1%E5%99%A8)
  - [git update-server-info - 更新服务器信息](#git-update-server-info---%E6%9B%B4%E6%96%B0%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%BF%A1%E6%81%AF)
- [管道命令](#%E7%AE%A1%E9%81%93%E5%91%BD%E4%BB%A4)
  - [git cat-file - 提供存储库对象的内容或类型和大小信息](#git-cat-file---%E6%8F%90%E4%BE%9B%E5%AD%98%E5%82%A8%E5%BA%93%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%86%85%E5%AE%B9%E6%88%96%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%A4%A7%E5%B0%8F%E4%BF%A1%E6%81%AF)
  - [git check-ignore - 调试gitignore / exclude文件](#git-check-ignore---%E8%B0%83%E8%AF%95gitignore--exclude%E6%96%87%E4%BB%B6)
  - [git checkout-index - 将文件从索引复制到工作树](#git-checkout-index---%E5%B0%86%E6%96%87%E4%BB%B6%E4%BB%8E%E7%B4%A2%E5%BC%95%E5%A4%8D%E5%88%B6%E5%88%B0%E5%B7%A5%E4%BD%9C%E6%A0%91)
  - [git commit-tree -  创建一个新的提交对象](#git-commit-tree----%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E6%8F%90%E4%BA%A4%E5%AF%B9%E8%B1%A1)
  - [git count-objects - 计算解压缩的对象数及其磁盘消耗](#git-count-objects---%E8%AE%A1%E7%AE%97%E8%A7%A3%E5%8E%8B%E7%BC%A9%E7%9A%84%E5%AF%B9%E8%B1%A1%E6%95%B0%E5%8F%8A%E5%85%B6%E7%A3%81%E7%9B%98%E6%B6%88%E8%80%97)
  - [git diff-index - 将树与工作树或索引进行比较](#git-diff-index---%E5%B0%86%E6%A0%91%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A0%91%E6%88%96%E7%B4%A2%E5%BC%95%E8%BF%9B%E8%A1%8C%E6%AF%94%E8%BE%83)
  - [git for-each-ref - 每个参考的输出信息](#git-for-each-ref---%E6%AF%8F%E4%B8%AA%E5%8F%82%E8%80%83%E7%9A%84%E8%BE%93%E5%87%BA%E4%BF%A1%E6%81%AF)
  - [git hash-object - 计算对象ID，并可选择从文件创建blob](#git-hash-object---%E8%AE%A1%E7%AE%97%E5%AF%B9%E8%B1%A1id%E5%B9%B6%E5%8F%AF%E9%80%89%E6%8B%A9%E4%BB%8E%E6%96%87%E4%BB%B6%E5%88%9B%E5%BB%BAblob)
  - [git ls-files - 显示索引和工作树中的文件信息](#git-ls-files---%E6%98%BE%E7%A4%BA%E7%B4%A2%E5%BC%95%E5%92%8C%E5%B7%A5%E4%BD%9C%E6%A0%91%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6%E4%BF%A1%E6%81%AF)
  - [git ls-tree - 列出树对象的内容](#git-ls-tree---%E5%88%97%E5%87%BA%E6%A0%91%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%86%85%E5%AE%B9)
  - [git merge-base - 为合并找到尽可能好的共同祖先](#git-merge-base---%E4%B8%BA%E5%90%88%E5%B9%B6%E6%89%BE%E5%88%B0%E5%B0%BD%E5%8F%AF%E8%83%BD%E5%A5%BD%E7%9A%84%E5%85%B1%E5%90%8C%E7%A5%96%E5%85%88)
  - [git read-tree - 将树信息读入索引](#git-read-tree---%E5%B0%86%E6%A0%91%E4%BF%A1%E6%81%AF%E8%AF%BB%E5%85%A5%E7%B4%A2%E5%BC%95)
  - [git rev-list - 以反向时间顺序列出提交对象](#git-rev-list---%E4%BB%A5%E5%8F%8D%E5%90%91%E6%97%B6%E9%97%B4%E9%A1%BA%E5%BA%8F%E5%88%97%E5%87%BA%E6%8F%90%E4%BA%A4%E5%AF%B9%E8%B1%A1)
  - [git rev-parse -  挑选和按摩参数](#git-rev-parse----%E6%8C%91%E9%80%89%E5%92%8C%E6%8C%89%E6%91%A9%E5%8F%82%E6%95%B0)
  - [git show-ref - 列出本地存储库中的引用](#git-show-ref---%E5%88%97%E5%87%BA%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E5%BA%93%E4%B8%AD%E7%9A%84%E5%BC%95%E7%94%A8)
  - [git symbolic-ref -  读取，修改和删除符号引用](#git-symbolic-ref----%E8%AF%BB%E5%8F%96%E4%BF%AE%E6%94%B9%E5%92%8C%E5%88%A0%E9%99%A4%E7%AC%A6%E5%8F%B7%E5%BC%95%E7%94%A8)
  - [git update-index - 将工作树中的文件内容注册到索引](#git-update-index---%E5%B0%86%E5%B7%A5%E4%BD%9C%E6%A0%91%E4%B8%AD%E7%9A%84%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%B3%A8%E5%86%8C%E5%88%B0%E7%B4%A2%E5%BC%95)
  - [git update-ref - 安全地更新存储在ref中的对象名](#git-update-ref---%E5%AE%89%E5%85%A8%E5%9C%B0%E6%9B%B4%E6%96%B0%E5%AD%98%E5%82%A8%E5%9C%A8ref%E4%B8%AD%E7%9A%84%E5%AF%B9%E8%B1%A1%E5%90%8D)
  - [git verify-pack -  验证打包的Git存档文件](#git-verify-pack----%E9%AA%8C%E8%AF%81%E6%89%93%E5%8C%85%E7%9A%84git%E5%AD%98%E6%A1%A3%E6%96%87%E4%BB%B6)
  - [git write-tree - 从当前索引创建树对象](#git-write-tree---%E4%BB%8E%E5%BD%93%E5%89%8D%E7%B4%A2%E5%BC%95%E5%88%9B%E5%BB%BA%E6%A0%91%E5%AF%B9%E8%B1%A1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# github-gitee

添加ssl-key

```shell
ssh-keygen -t rsa -C "1245863260@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/tianqixin/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):    # 直接回车
Enter same passphrase again:                   # 直接回车
Your identification has been saved in /Users/tianqixin/.ssh/id_rsa.
Your public key has been saved in /Users/tianqixin/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:MDKVidPTDXIQoJwoqUmI4LBAsg5XByBlrOEzkxrwARI 429240967@qq.com
The key's randomart image is:
+---[RSA 3072]----+
|E*+.+=**oo       |
|%Oo+oo=o. .      |
|%**.o.o.         |
|OO.  o o         |
|+o+     S        |
|.                |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

然后在github gitee **SSH and GPG keys** 绑定

```shell
cd workpath
git init
git add .
git commit -m "git.md"
git remote add github git@github.com:StoneBridPathWeave/notebook.git
git remote add gitee git@gitee.com:hujixu/notebook.git
git remote -v  #查看仓库信息
git push [-f 强制] github master
git push [-f 强制] gitee master
```

# :point_right:git 提交规范

Commit message 格式

```bash
<type>(<scope>): <subject>
```

**type必填**表示提交类型，值有以下几种：

- feat - 新功能 feature
- fix - 修复 bug
- docs - 文档注释
- style - 代码格式(不影响代码运行的变动)
- refactor - 重构、优化(既不增加新功能，也不是修复bug)
- perf - 性能优化
- test - 增加测试
- chore - 构建过程或辅助工具的变动
- revert - 回退
- build - 打包

# 系统设置相关

## :point_right:git config - 获取和设置配置项

linux：

1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 `git config` 时带上 `--system` 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 你可以传递 `--global` 选项让 Git 读写此文件，这会对你系统上 **所有** 的仓库生效。

windows：

​	一般情况下是 `C:\Users\$USER` ）的 `.gitconfig` 文件。

常用命令

```bash
# --gloabl 全局生效
git config --global user.name "stonebirdjx"
git config --global user.email stonebirdjx@example.com

# -e/--edit 启动编辑模式
git config --global -e

# -l/--list 查看配置类型
git config --list

# git config key获取配置
git config user.name
```

## git help - 帮助命令

```bash
git --help
# git help command

git help config
```

# 获取和创建项目

## :point_right:git init - 创建空的git项目

重新初始化一个现有的

```bash
git init [-q | --quiet] [--bare] [--template=<template-directory>]
	  [--separate-git-dir <git-dir>] [--object-format=<format>]
	  [-b <branch-name> | --initial-branch=<branch-name>]
	  [--shared[=<permissions>]] [<directory>]
```

常用命令

```bash
-q/--quiet # 安静模式创建
-b <branch-name>
--initial-branch=<branch-name> # 初始化的分支名称
--shared[=(false|true|umask|group|all|world|everybody|<perm>)] # 指定 Git 存储库将在多个用户之间共享。

# 例子
git init .
git init . -b main
```

## :point_right:git clone - 将存储库克隆到新目录中

为克隆存储库中的每个分支创建远程跟踪分支（使用`git branch --remotes`可见），并创建、签出从克隆存储库当前活动的分支派生的初始分支。

`git clone <repository>]`

常用命令

```bash
-b/--branch <name> # clone指定的分支
--depth <深度> # 创建一个历史记录被截断为指定提交次数的“浅”克隆。
--shallow-since=<date> # 创建浅克隆，只包含指定的时间之后的历史记录。
--shallow-exclude=<revision> # 创建一个有历史的浅克隆，不包含可以被指定远程分支或标签访问的提交。本选项可以使用多次。
--[no-]single-branch # 仅克隆直到单一分支末尾的历史，该分支被 --branch 选项或主分支远程 HEAD 指定。
--[no-]shallow-submodules # 所有克隆的子模块都将是浅克隆，深度为1。
--[no-]remote-submodules # 克隆的所有子模块将使用子模块的远程跟踪分支的状态来更新子模块

# 例子
git clone git@github.com:stonebirdjx/k8s-ladder.git
git clone -b stonebird-tmp git@github.com:stonebirdjx/k8s-ladder.git
```

# 基本操作命令

## :point_right:git add - 将文件内容添加到索引

```bash
git clone [--template=<template-directory>]
	  [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
	  [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
	  [--dissociate] [--separate-git-dir <git-dir>]
	  [--depth <depth>] [--[no-]single-branch] [--no-tags]
	  [--recurse-submodules[=<pathspec>]] [--[no-]shallow-submodules]
	  [--[no-]remote-submodules] [--jobs <n>] [--sparse] [--[no-]reject-shallow]
	  [--filter=<filter> [--also-filter-submodules]] [--] <repository>
	  [<directory>]
```

常用命令

```bash
-f/--force #允许添加已被忽略的文件。
-e/--edit # 用户编辑后添加
-u/--update # 在索引已经有与 <指定路径> 匹配项的地方更新索引。
--refresh # 不添加文件，只刷新索引中的 stat() 信息。
--ignore-errors # 如果由于索引错误而无法添加某些文件，请不要中止操作，而是继续添加其他文件。

# 例子
git add *.md
git add *.go --ignore-errors
```

## :point_right:git status --  显示工作树状态

```bash
git status [<options>] [--] [<pathspec>…​]
```

常用命令

```bash
-b/--branch # 即使在短文中也要显示分支和跟踪信息。

# 例子
git status
git status -b stonebird-tmp
```

在下面的表格中，这三个类别分别显示在不同的部分，这些字符用于显示跟踪路径的前两个部分的`X`和`Y`字段。

- '' = 未修改的
- *M* = 修改过的
- *T* = file type changed (regular file, symbolic link or submodule)
- *A*=添加
- *D* = 删除
- *R* = 重命名
- *C* = copied (if config option status.renames is set to "copies")
- *U*=更新但未合并

```bash
X          Y     Meaning
-------------------------------------------------
	     [AMD]   not updated
M        [ MTD]  updated in index
T        [ MTD]  type changed in index
A        [ MTD]  added to index
D                deleted from index
R        [ MTD]  renamed in index
C        [ MTD]  copied in index
[MTARC]          index and work tree matches
[ MTARC]    M    work tree changed since index
[ MTARC]    T    type changed in work tree since index
[ MTARC]    D    deleted in work tree
	        R    renamed in work tree
	        C    copied in work tree
-------------------------------------------------
D           D    unmerged, both deleted
A           U    unmerged, added by us
U           D    unmerged, deleted by them
U           A    unmerged, added by them
D           U    unmerged, deleted by us
A           A    unmerged, both added
U           U    unmerged, both modified
-------------------------------------------------
?           ?    untracked
!           !    ignored
-------------------------------------------------
```

## :point_right:git diff - 显示提交、提交和工作树等之间的变化

```bash
git diff [<options>] [<commit>] [--] [<path>…​]
git diff [<options>] --cached [--merge-base] [<commit>] [--] [<path>…​]
git diff [<options>] [--merge-base] <commit> [<commit>…​] <commit> [--] [<path>…​]
git diff [<options>] <commit>…​<commit> [--] [<path>…​]
git diff [<options>] <blob> <blob>
git diff [<options>] --no-index [--] <path> <path>
```

常用命令

```bash
# 例子
# 比较工作区与暂存区
　git diff 不加参数即默认比较工作区与暂存区
# 比较暂存区与最新本地版本库（本地库中最近一次commit的内容）
  git diff --cached  [<path>...] 
# 比较工作区与最新本地版本库
  git diff HEAD [<path>...]  如果HEAD指向的是master分支，那么HEAD还可以换成master
# 比较工作区与指定commit-id的差异
  git diff commit-id  [<path>...] 
# 比较暂存区与指定commit-id的差异
  git diff --cached [<commit-id>] [<path>...] 
# 比较两个commit-id之间的差异
  git diff [<commit-id>] [<commit-id>]
# 比较两个分支
  git diff branch1 branch2
```

## :point_right:git commit - 提交对存储库的更改信息

```bash
git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
	   [--dry-run] [(-c | -C | --squash) <commit> | --fixup [(amend|reword):]<commit>)]
	   [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
	   [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
	   [--date=<date>] [--cleanup=<mode>] [--[no-]status]
	   [-i | -o] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	   [(--trailer <token>[(=|:)<value>])…​] [-S[<keyid>]]
	   [--] [<pathspec>…​]
```

常用命令

```bash
-a  # 参数用于先将所有工作区的变动文件，提交到暂存区，再运行git commit。用了-a参数，就不用执行git add .命令了。
-m # -m参数用于添加提交说明。
—allow-empty # --allow-empty参数用于没有提交信息的 commit。
—amend # --amend参数用于撤销上一次 commit，然后生成一个新的 commit。
—fixup # --fixup参数的含义是，当前添加的 commit 是以前某一个 commit 的修正。以后执行互动式的git rebase的时候，这两个 commit

# 例子
git commit -m "feat:crontab"
```

## git notes - 添加或检查对象注释

```bash
git notes [list [<object>]]
git notes add [-f] [--allow-empty] [-F <file> | -m <msg> | (-c | -C) <object>] [<object>]
git notes copy [-f] ( --stdin | <from-object> [<to-object>] )
git notes append [--allow-empty] [-F <file> | -m <msg> | (-c | -C) <object>] [<object>]
git notes edit [--allow-empty] [<object>]
git notes show [<object>]
git notes merge [-v | -q] [-s <strategy> ] <notes-ref>
git notes merge --commit [-v | -q]
git notes merge --abort [-v | -q]
git notes remove [--ignore-missing] [--stdin] [<object>…​]
git notes prune [-n] [-v]
git notes get-ref
```

常用命令

```bash
git notes add -m 'Tested-by: Johannes Sixt <j6t@kdbg.org>' 72a144e2
```

## :point_right:git restore - 恢复工作树文件

```bash
git restore [<options>] [--source=<tree>] [--staged] [--worktree] [--] <pathspec>…
git restore [<options>] [--source=<tree>] [--staged] [--worktree] --pathspec-from-file=<file> [--pathspec-file-nul]
git restore (-p|--patch) [<options>] [--source=<tree>] [--staged] [--worktree] [--] [<pathspec>…]
```

常用命令

```bash
git restore # 指令使得在工作空间但是不在暂存区的文件撤销更改(内容恢复到没修改之前的状态)
git restore --staged # 作用是将暂存区的文件从暂存区撤出，但不会更改文件的内容。

# 例子
git switch master
git restore --source master~2 Makefile  
rm -f hello.c
git restore hello.c                     
```

## :point_right:git reset - 重置当前HEAD到指定的状态

```BASH
git reset [-q] [<tree-ish>] [--] <pathspec>…​
git reset [-q] [--pathspec-from-file=<file> [--pathspec-file-nul]] [<tree-ish>]
git reset (--patch | -p) [<tree-ish>] [--] [<pathspec>…​]
git reset [--soft | --mixed [-N] | --hard | --merge | --keep] [-q] [<commit>]
```

常用命令

```bash
git reset HEAD^            # 回退所有内容到上一个版本  
git reset HEAD^ hello.php  # 回退 hello.php 文件的版本到上一个版本  
git reset  052e           # 回退到指定版本

#--soft 参数用于回退到某个版本：
git reset --soft HEAD
git reset --soft HEAD~3

# --hard 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交：
git reset --hard HEAD~3  # 回退上上上一个版本  
git reset –hard bae128  # 回退到某个版本回退点之前的所有信息。 
git reset --hard origin/master    # 将本地的状态回退到和远程的一样 
```

**HEAD 说明：**

- HEAD 表示当前版本
- HEAD^ 上一个版本
- HEAD^^ 上上一个版本
- HEAD^^^ 上上上一个版本
- 以此类推...

可以使用 ～数字表示

- HEAD~0 表示当前版本
- HEAD~1 上一个版本
- HEAD^2 上上一个版本
- HEAD^3 上上上一个版本
- 以此类推...

## :point_right:git rm - 从工作树和索引中删除文件

```bash
git rm [-f | --force] [-n] [-r] [--cached] [--ignore-unmatch]
	  [--quiet] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	  [--] [<pathspec>…​]
```

常用命令

```bash
git rm  -f # 同时从工作区和索引中删除文件。即本地的文件也被删除了。
git rm --cached # 从索引中删除文件。
```

## :point_right:git mv -  移动或重命名一个文件、一个目录或一个符号链接

```bash
git mv [<options>] <source>…​ <destination>
```

常用命令

```bash
-f/--force # 强制执行
-k #跳过会导致错误情况的移动或重命名操作
-n/--dry-run# 什么都不做；只显示会发生什么
```

# 分支与合并

## :point_right:git branch - 列出、创建或删除分支

```bash
git branch [--color[=<when>] | --no-color] [--show-current]
	[-v [--abbrev=<n> | --no-abbrev]]
	[--column[=<options>] | --no-column] [--sort=<key>]
	[--merged [<commit>]] [--no-merged [<commit>]]
	[--contains [<commit>]] [--no-contains [<commit>]]
	[--points-at <object>] [--format=<format>]
	[(-r | --remotes) | (-a | --all)]
	[--list] [<pattern>…​]
git branch [--track[=(direct|inherit)] | --no-track] [-f]
	[--recurse-submodules] <branchname> [<start-point>]
git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]
git branch --unset-upstream [<branchname>]
git branch (-m | -M) [<oldbranch>] <newbranch>
git branch (-c | -C) [<oldbranch>] <newbranch>
git branch (-d | -D) [-r] <branchname>…​
git branch --edit-description [<branchname>]
```

常用命令

```bash
-d/--delete # 删除一个分支。
-f/--force # 强制执行
-D # 强制删除 等于-d --force
-m/--move # 移动/重命名一个分支，连同它的配置和 reflog。
-M # 等于-m --force
-c/--copy # 复制一个分支，连同它的配置和 reflog。
-a/--all # 列出远程跟踪的分支和本地分支。 与`--list`组合，以匹配可选的模式。
-l/--list # 列出分支
--show-current # 打印当前分支的名称。

# 例子
git branch stonebird 
git branch --list
```

## :point_right:git checkout - 切换分支或恢复工作树文件

```bash
git checkout [-q] [-f] [-m] [<branch>]
git checkout [-q] [-f] [-m] --detach [<branch>]
git checkout [-q] [-f] [-m] [--detach] <commit>
git checkout [-q] [-f] [-m] [[-b|-B|--orphan] <new-branch>] [<start-point>]
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <pathspec>…​
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] --pathspec-from-file=<file> [--pathspec-file-nul]
git checkout (-p|--patch) [<tree-ish>] [--] [<pathspec>…​]
```

常用命令

```bash
-f/--force # 强制执行
-b <new-branch> # 创建新分支
-B <new-branch> # 创建新分支

# 例子
git checkout stonebird
git checkout -b stonebird
```

## git switch - 切换分支

```bash
git switch [<options>] [--no-guess] <branch>
git switch [<options>] --detach [<start-point>]
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
git switch [<options>] --orphan <new-branch>
```

常用命令

```bash
-c/--create <new-branch>

# 例子
git branch -f <new-branch>
git switch <new-branch>
```

## :point_right:git merge - 合并两个或以上的分支

```bash
git merge [-n] [--stat] [--no-commit] [--squash] [--[no-]edit]
	[--no-verify] [-s <strategy>] [-X <strategy-option>] [-S[<keyid>]]
	[--[no-]allow-unrelated-histories]
	[--[no-]rerere-autoupdate] [-m <msg>] [-F <file>]
	[--into-name <branch>] [<commit>…​]
git merge (--continue | --abort | --quit)
```

常用命令

```bash
--no-ff #表示禁用Fast forward;可以保存之前的分支历史。能够更好的查看merge历史，以及branch状态.
#保证版本提交、分支结构清晰
git merge <branch> # 将指定分支合并到当前分支
git merge --abort # 终止正在进行的合并
# 例子
git merge dev
git merge fixes enhancements # 在当前分支上合并 fixes 和 enhancements
# merge tag
git fetch origin
git merge v1.2.3^0
git merge --ff-only v1.2.3
```

## git mergetool  - 合并工具

> 解决合并冲突

```bash
git mergetool [--tool=<tool>] [-y | --[no-]prompt] [<file>…​]
```

需要配置

```bash
#difftool 配置  
git config --global diff.tool bc4  
git config --global difftool.bc4.cmd "\"c:/program files (x86)/beyond compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\""


#mergeftool 配置  
git config --global merge.tool bc4
git config --global mergetool.bc4.cmd  "\"c:/program files (x86)/beyond compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\""  
git config --global mergetool.bc4.trustExitCode true

#让git mergetool不再生成备份文件(*.orig)  
git config --global mergetool.keepBackup false

# 例子
git mergetool
```

## :point_right:git log - 展示commit信息

```bash
git log [<options>] [<revision-range>] [[--] <path>…]
```

常用命令

```bash
git log --no-merges。
# 显示整个提交历史，但跳过任何合并内容
git log v2.6.12.. include/scsi drivers/scsi
# 显示自版本’v2.6.12’以来改变`include/scsi`或`drivers/scsi`子目录中任何文件的所有提交。
git log --since="2 weeks ago" -- gitk。
# 显示过去两周内对文件’gitk’的修改。 `--`是必要的，以避免与名为’gitk’的*分支相混淆。
git log --name-status release..test
# 显示在 "test "分支中但尚未在 "release "分支中的提交，以及每个提交修改的路径列表。
git log --follow builtin/rev-list.c。
# 显示改变`builtin/rev-list.c`的提交，包括那些在文件被赋予现在名字之前发生的提交。
git log --branches --not --remotes=origin。
# 显示所有在本地分支中但不在 "origin "的远程跟踪分支中的提交（你有而origin没有的东西）。
git log master --not --remotes=*/master。
# 显示所有在本地主库但不在任何远程仓库主库分支中的提交。
git log -p -m -first-parent。
# 显示包括变化差异的历史，但只从 "主分支 "的角度，跳过来自合并分支的提交，并显示合并带来的全部变化差异。 这只有在遵循严格的政策，在停留在一个集成分支时合并所有主题分支时才有意义。
git log -L '/int main/',/^}/:main.c。
# 显示了文件`main.c`中的函数`main()`是如何随时间演变的。
git log -3
# 将显示的提交数量限制在3个。
```

## git stash - 将更改存储在脏工作目录中

当要记录工作目录和索引的当前状态，但想要返回到干净的工作目录时，则使用`git stash`。 该命令保存本地修改，并恢复工作目录以匹配`HEAD`提交。

```bash
git stash list [<log-options>]
git stash show [-u | --include-untracked | --only-untracked] [<diff-options>] [<stash>]
git stash drop [-q | --quiet] [<stash>]
git stash pop [--index] [-q | --quiet] [<stash>]
git stash apply [--index] [-q | --quiet] [<stash>]
git stash branch <branchname> [<stash>]
git stash [push [-p | --patch] [-S | --staged] [-k | --[no-]keep-index] [-q | --quiet]
	     [-u | --include-untracked] [-a | --all] [(-m | --message) <message>]
	     [--pathspec-from-file=<file> [--pathspec-file-nul]]
	     [--] [<pathspec>…​]]
git stash save [-p | --patch] [-S | --staged] [-k | --[no-]keep-index] [-q | --quiet]
	     [-u | --include-untracked] [-a | --all] [<message>]
git stash clear
git stash create [<message>]
git stash store [(-m | --message) <message>] [-q | --quiet] <commit>c
```

常用命令

```bash
git stash 
git stash pop
```

## :point_right:git tag - 创建，列出，删除或验证使用GPG签名的标记对象

```bash
git tag [-a | -s | -u <key-id>] [-f] [-m <msg> | -F <file>] [-e]
	<tagname> [<commit> | <object>]
git tag -d <tagname>…​
git tag [-n[<num>]] -l [--contains <commit>] [--no-contains <commit>]
	[--points-at <object>] [--column[=<options>] | --no-column]
	[--create-reflog] [--sort=<key>] [--format=<format>]
	[--merged <commit>] [--no-merged <commit>] [<pattern>…​]
git tag -v [--format=<format>] <tagname>…
```

常用命令

```bash
-a
--annotate
创建一个未签名的带注释的标记对象

-f
--force
Replace an existing tag with the given name (instead of failing)

-d
--delete
删除具有给定名称的现有标签。

-l
--list
列出当前分支所有tag

# 例子
git tag v1.1.0
git tag -a v1.0 
git tag -d v1.0.0 # 删除
git push origin v1.0.0 # 推送远程tag
git push -d origin v1.0.0 # 删除远程tag
```

## git worktree - 管理多个工作树

```bash
git worktree add [-f] [--detach] [--checkout] [--lock [--reason <string>]]
		   [-b <new-branch>] <path> [<commit-ish>]
git worktree list [-v | --porcelain [-z]]
git worktree lock [--reason <string>] <worktree>
git worktree move <worktree> <new-path>
git worktree prune [-n] [-v] [--expire <expire>]
git worktree remove [-f] <worktree>
git worktree repair [<path>…]
git worktree unlock <worktree>
```

例子

```bash
# 增加一个新的 worktree ，并指定了其关联的目录是 path ，关联的分支是 <branch> 。后者是一个可选项，默认值是 HEAD 分支。如果 <branch> 已经被关联到了一个 worktree ，则这次 add 会被拒绝执行，可以通过增加 -f | --force 选项来强制执行。
git worktree add <path> [<branch>]
```

# 共享和更新项目

## git fetch - 从远程分支拉取代码

`git fetch + git merge == git pull`

```bash
git fetch [<options>] [<repository> [<refspec>…]]
git fetch [<options>] <group>
git fetch --multiple [<options>] [(<repository> | <group>)…]
git fetch --all [<options>]
```

如果直接使用git fetch，则步骤如下：

创建并更新本 地远程分支。即创建并更新origin/xxx 分支，拉取代码到origin/xxx分支上。
在FETCH_HEAD中设定当前分支-origin/当前分支对应，如直接到时候git merge就可以将origin/abc合并到abc分支上。

常用命令

```bash
git fetch origin
git fetch origin branch1
git fetch origin branch1:branch2
git fetch origin :branch2 # git fetch origin master:branch2
```

## :point_right:git pull - 拉取远端分支并合并

```bash
# git pull <远程主机名> <远程分支名>:<本地分支名>
git pull [<options>] [<repository> [<refspec>…]]
```

常用命令

```bash
--rebase，# 它运行git rebase而不是git merge。

# 例子
git pull origin next:master
git pull --rebase origin next:master
# 如果远程分支(next)要与当前分支合并，则冒号后面的部分可以省略。
git pull origin next
```

## :point_right:git push - 推送到远端分支

```bash
git push [--all | --mirror | --tags] [--follow-tags] [--atomic] [-n | --dry-run] [--receive-pack=<git-receive-pack>]
	   [--repo=<repository>] [-f | --force] [-d | --delete] [--prune] [-v | --verbose]
	   [-u | --set-upstream] [-o <string> | --push-option=<string>]
	   [--[no-]signed|--signed=(true|false|if-asked)]
	   [--force-with-lease[=<refname>[:<expect>]] [--force-if-includes]]
	   [--no-verify] [<repository> [<refspec>…]]
```

命令格式如下：

```bash
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```bash
git push <远程主机名> <本地分支名>
```

常用命令

```bash
-f/--force # 强制推送

# 例子
git push --force origin
git push origin HEAD # 将当前分支推送到远程的同名的简单方法
```

## :point_right:git remote - 管理远程仓库

```bash
git remote [-v | --verbose]
git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=(fetch|push)] <name> <URL>
git remote rename [--[no-]progress] <old> <new>
git remote remove <name>
git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
git remote set-branches [--add] <name> <branch>…
git remote get-url [--push] [--all] <name>
git remote set-url [--push] <name> <newurl> [<oldurl>]
git remote set-url --add [--push] <name> <newurl>
git remote set-url --delete [--push] <name> <URL>
git remote [-v | --verbose] show [-n] <name>…
git remote prune [-n | --dry-run] <name>…
git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)…]
```

常用命令

```bash
git remote add origin git@github.com:stonebirdjx/git-test.git
git remote -v
git remote rm name  # 删除远程仓库
git remote rename old_name new_name  # 修改仓库名
```

## :point_right:git submodule - 子模块管理

```bash
git submodule [--quiet] [--cached]
git submodule [--quiet] add [<options>] [--] <repository> [<path>]
git submodule [--quiet] status [--cached] [--recursive] [--] [<path>…]
git submodule [--quiet] init [--] [<path>…]
git submodule [--quiet] deinit [-f|--force] (--all|[--] <path>…)
git submodule [--quiet] update [<options>] [--] [<path>…]
git submodule [--quiet] set-branch [<options>] [--] <path>
git submodule [--quiet] set-url [--] <path> <newurl>
git submodule [--quiet] summary [<options>] [--] [<path>…]
git submodule [--quiet] foreach [--recursive] <command>
git submodule [--quiet] sync [--recursive] [--] [<path>…]
git submodule [--quiet] absorbgitdirs [--] [<path>…]
```

例子

```bash
git submodule add https://github.com/stonebirdjx/assets.git assets
git status # 可以看到目录有增加1个文件.gitmodules
git submodule # 查看子模块
git submodule update
git submodule update --remote
git submodule init # 子模块初始化
```

# 检查比较

## :point_right:git show - 显示各种类型的对象

对于提交，它显示日志消息和文本差异，对于标签，它显示标签消息和引用对象。

```bash
git show [<options>] [<object>…]
```

常用命令

```bash
# 查看commitid
git show ${commit_id}
git show ${commit_id} ${file_name}

# 查看tag
git tag # 获取tag_name
git show ${tag_name}
```

## :point_right:git log - 显示提交日志

```bash
git log [<options>] [<revision-range>] [[--] <path>…]
```

常用命令

```bash
-n <number>
--max-count=<number>
限制输出的提交数量。

--skip=<number>
在开始显示提交输出之前，跳过’数’的提交


# 例子
git log -n 2
git log ${file_name1} ${file_name2}
git log ${commitid} ${commitid2}

# tagName或branchame
git log test..master 查询master分支中的提交记录但不包含test分支记录
```

## :point_right:git diff - 查看文件在工作目录与暂存区的差别

```bash
git diff [<options>] [<commit>] [--] [<path>…]
git diff [<options>] --cached [--merge-base] [<commit>] [--] [<path>…]
git diff [<options>] [--merge-base] <commit> [<commit>…] <commit> [--] [<path>…]
git diff [<options>] <commit>…​<commit> [--] [<path>…]
git diff [<options>] <blob> <blob>
git diff [<options>] --no-index [--] <path> <path>
```

常用命令

```bash
# 工作目录 vs 暂存区
git diff <filename>

# 也可查看和另一分支的区别。
git diff <branch> <filename>

# 暂存区 vs Git仓库
git diff --cached <filename>
# 表示查看已经 add 进暂存区但是尚未 commit 的内容同最新一次 commit 时的内容的差异。
git diff --cached <commit> <filename>

# 工作目录 vs Git仓库
git diff <commit> <filename>

# git 仓库 vs git 仓库
git diff <commit> <commit>
```

## git difftool - 使用常见差异工具在修订之间比较和编辑文件

```bash
git difftool [<options>] [<commit> [<commit>]] [--] [<path>…]
```

例子

```bash
git difftool ${commit_ID} ${commit—id2}
```

## diff range-diff - 比较两个提交范围

例如一个分支的两个版本。

```bash
git range-diff [--color=[<when>]] [--no-color] [<diff-options>]
	[--no-dual-color] [--creation-factor=<factor>]
	[--left-only | --right-only]
	( <range1> <range2> | <rev1>…<rev2> | <base> <rev1> <rev2> )
	[[--] <path>…]
```

例子

```bash
git range-diff theirs~5..theirs ours~4..ours

          T1--T2--T3--T4--T5   <-- theirs
         /
...--o--*   <-- base
         \
          O1--O2--O3--O4   <-- ours
```

## git shortlog - 总结 git log输出

```bash
git shortlog [<options>] [<revision-range>] [[--] <path>…​]
git log --pretty=short | git shortlog [<options>]
```

## git describe - 命令显示离当前提交最近的标签

```bash
git describe [--all] [--tags] [--contains] [--abbrev=<n>] [<commit-ish>…​]
git describe [--all] [--tags] [--contains] [--abbrev=<n>] --dirty[=<mark>]
git describe <blob>
```

常用命令

```bash
# 不加任何参数的情况下，git describe 只会列出带有注释的tag
git describe
```

# 补丁

## git apply - 将补丁应用于文件和/或索引

```bash
git apply [--stat] [--numstat] [--summary] [--check] [--index | --intent-to-add] [--3way]
	  [--apply] [--no-add] [--build-fake-ancestor=<file>] [-R | --reverse]
	  [--allow-binary-replacement | --binary] [--reject] [-z]
	  [-p<n>] [-C<n>] [--inaccurate-eof] [--recount] [--cached]
	  [--ignore-space-change | --ignore-whitespace]
	  [--whitespace=(nowarn|warn|fix|error|error-all)]
	  [--exclude=<path>] [--include=<path>] [--directory=<root>]
	  [--verbose | --quiet] [--unsafe-paths] [--allow-empty] [<patch>…]
```

常用命令

```bash
git apply --verbose {{path/to/file}}
git apply --index {{path/to/file}}
curl {{https://example.com/file.patch}} | git apply
git apply --stat --apply {{path/to/file}}
git apply --reverse {{path/to/file}}
git apply --cache {{path/to/file}}
```

## :point_right:git cherry-pick - 就是将指定的提交（commit）应用于其他分支

这时分两种情况。一种情况是，你需要另一个分支的所有代码变动，那么就采用合并（`git merge`）。另一种情况是，你只需要部分代码变动（某几个提交），这时可以采用 Cherry pick。

```bash
git cherry-pick [--edit] [-n] [-m <parent-number>] [-s] [-x] [--ff]
		  [-S[<keyid>]] <commit>…
git cherry-pick (--continue | --skip | --abort | --quit)
```

常用命令

```bash
# 例子：将2555c6e用于现在分支
git cherry-pick 2555c6e
```

## :point_right:git rebase - 合并变更

rebase解释为变基，用于将变更合并到当前分支，与merge功能相似，但是合并后的gitGraph更优美

 注意：不推荐在公共分支上执行rebase操作，由于rebase的变基特性，会导致分支内容混乱

```bash
git rebase [-i | --interactive] [<options>] [--exec <cmd>]
	[--onto <newbase> | --keep-base] [<upstream> [<branch>]]
git rebase [-i | --interactive] [<options>] [--exec <cmd>] [--onto <newbase>]
	--root [<branch>]
git rebase (--continue | --skip | --abort | --quit | --edit-todo | --show-current-patch)
```

常用命令

```bash
git rebase -i 36224db
```

## :point_right:git revert - 用于撤回某次提交的内容，同时再产生一个新的提交(commit)

```bash
git revert [--[no-]edit] [-n] [-m <parent-number>] [-s] [-S[<keyid>]] <commit>…
git revert (--continue | --skip | --abort | --quit)
```

常用命令

```bash
git revert c950839
```

# Debugging调试

## :point_right:git bisect  - 用来查找哪一次提交引入了错误

```bash
git bisect start [--term-{new,bad}=<term> --term-{old,good}=<term>]
	  [--no-checkout] [--first-parent] [<bad> [<good>...]] [--] [<paths>...]
git bisect (bad|new|<term-new>) [<rev>]
git bisect (good|old|<term-old>) [<rev>...]
git bisect terms [--term-good | --term-bad]
git bisect skip [(<rev>|<range>)...]
git bisect reset [<commit>]
git bisect (visualize|view)
git bisect replay <logfile>
git bisect log
git bisect run <cmd>...
git bisect help

```

例子

```bash
# git log --pretty=oneline
# git bisect start命令启动查错，它的格式：git bisect start [终点] [起点]
 git bisect start HEAD 4d83cf

# 使用git bisect good命令，标识本次没有问题。
git bisect good

# 使用git bisect bad命令，标识本次提交有问题。
git bisect bad

# 跳过某次检查
git bisect skip 

# 退出bisect模式
git bisect reset
```

## :point_right:git blame - 显示修改版本和作者上次修改文件的每一行

```bash
git blame [-c] [-b] [-l] [--root] [-t] [-f] [-n] [-s] [-e] [-p] [-w] [--incremental]
	    [-L <range>] [-S <revs-file>] [-M] [-C] [-C] [-C] [--since=<date>]
	    [--ignore-rev <rev>] [--ignore-revs-file <file>]
	    [--color-lines] [--color-by-age] [--progress] [--abbrev=<n>]
	    [<rev> | --contents <file> | --reverse <rev>..<rev>] [--] <file>
```

例子

```bash
git blame -L 40,60 foo
git blame -L 40,+21 foo
git blame -L '/^sub hello {/,/^}$/' foo
git blame v2.6.18.. -- foo
git blame --since=3.weeks -- foo
```

## :point_right:git grep - 文本搜索

```bash
git grep [-a | --text] [-I] [--textconv] [-i | --ignore-case] [-w | --word-regexp]
	   [-v | --invert-match] [-h|-H] [--full-name]
	   [-E | --extended-regexp] [-G | --basic-regexp]
	   [-P | --perl-regexp]
	   [-F | --fixed-strings] [-n | --line-number] [--column]
	   [-l | --files-with-matches] [-L | --files-without-match]
	   [(-O | --open-files-in-pager) [<pager>]]
	   [-z | --null]
	   [ -o | --only-matching ] [-c | --count] [--all-match] [-q | --quiet]
	   [--max-depth <depth>] [--[no-]recursive]
	   [--color[=<when>] | --no-color]
	   [--break] [--heading] [-p | --show-function]
	   [-A <post-context>] [-B <pre-context>] [-C <context>]
	   [-W | --function-context]
	   [--threads <num>]
	   [-f <file>] [-e] <pattern>
	   [--and|--or|--not|(|)|-e <pattern>…​]
	   [--recurse-submodules] [--parent-basename <basename>]
	   [ [--[no-]exclude-standard] [--cached | --no-index | --untracked] | <tree>…​]
	   [--] [<pathspec>…]
```

 例子

```bash
git grep "stonebird" 
git grep "stonebird" ${file_name}
```

# 向导

## :point_right:gitattributes - git 属性

.gitattributes

`gitattributes`文件中的每一行都是以下形式

```bash
pattern	attr1 attr2 ...

# 例子
*.js    eol=lf
*.jsx   eol=lf
*.json  eol=lf
```

>  如果是多系统协同工作，请把 .gitattributes 加入你的项目

## gitcli  -  Git 命令行界面和约定

--help的打印效果

## :point_right:giteveryday - Everyday Git的一组有用的最小命令

独立的开发人员不会与其他人交换补丁，而是使用以下命令单独在单个存储库中工作。

- [git-init [1\]](https://git-scm.com/docs/git-init) 创建一个新的存储库。
- [git-log [1\]](https://git-scm.com/docs/git-log) 看看发生了什么。
- [git-checkout [1\]](https://git-scm.com/docs/git-checkout) 和 [git-branch [1\]](https://git-scm.com/docs/git-branch) 切换分支。
- [git-add [1\]](https://git-scm.com/docs/git-add) 来管理索引文件。
- [git-diff [1\]](https://git-scm.com/docs/git-diff) 和 [git-status [1\]](https://git-scm.com/docs/git-status) 看你正在做什么。
- [git-commit [1\]](https://git-scm.com/docs/git-commit) 推进当前分支。
- [git-reset [1\]](https://git-scm.com/docs/git-reset) 和 [git-checkout [1\]](https://git-scm.com/docs/git-checkout) （带路径名参数）撤消更改。
- [git-merge [1\]](https://git-scm.com/docs/git-merge) 在本地分支之间合并。
- [git-rebase [1\]](https://git-scm.com/docs/git-rebase) 维护主题分支。
- [git-tag [1\]](https://git-scm.com/docs/git-tag) 标记已知点。

- [git-clone [1\]](https://git-scm.com/docs/git-clone) 从上游到本地存储库。
- 来自“origin”的 [git-pull [1\]](https://git-scm.com/docs/git-pull) 和 [git-fetch [1\]](https://git-scm.com/docs/git-fetch) 与上游保持同步。
- [git-push [1\]](https://git-scm.com/docs/git-push) 到共享存储库，如果采用CVS样式的共享存储库工作流程。
- [git-format-patch [1\]](https://git-scm.com/docs/git-format-patch) 准备电子邮件提交，如果采用Linux内核式公共论坛工作流程。
- [git-send-email [1\]](https://git-scm.com/docs/git-send-email) 发送您的电子邮件提交而不会被您的MUA损坏。
- [git-request-pull [1\]](https://git-scm.com/docs/git-request-pull) 创建上游拉动变化的摘要。
- [git-am [1\]](https://git-scm.com/docs/git-am) 应用您的贡献者通过电子邮件发送的补丁。
- [git-pull [1\]](https://git-scm.com/docs/git-pull) 从您信任的副手中合并。
- [git-format-patch [1\]](https://git-scm.com/docs/git-format-patch) 准备并发送建议的替代贡献者。
- [git-revert [1\]](https://git-scm.com/docs/git-revert) 撤消拙劣的提交。
- [git-push [1\]](https://git-scm.com/docs/git-push) 发布前沿。
- [git-daemon [1\]](https://git-scm.com/docs/git-daemon) 允许从存储库匿名下载。
- [git-shell [1\]](https://git-scm.com/docs/git-shell) 可以用作共享中央存储库用户的_受限登录shell_ 。
- [git-http-backend [1\]](https://git-scm.com/docs/git-http-backend) 提供了Git-over-HTTP（“Smart http”）的服务器端实现，允许提取和推送服务。
- [gitweb [1\]](https://git-scm.com/docs/gitweb) 为Git存储库提供了一个Web前端，可以使用 [git-instaweb [1\]](https://git-scm.com/docs/git-instaweb) 脚本进行设置。

## :point_right:git hooks

钩子都被存储在 Git 目录下的 `hooks` 子目录中。 也即绝大部分项目中的 `.git/hooks` 。

## :point_right:gitignore - 指定要忽略未跟踪文件

从与路径相同的目录中的`.gitignore`文件读取的模式，或在任何父目录中读取的模式，其中较高级别文件中的模式（直到工作树的顶层）被较低级别文件中的模式覆盖到包含该文件的目录。

```bash
# .gitignore 文件内容如下
*.log
*.temp
/vendor
```

## :point_right:gitmodules -  定义子模块属性

`.gitmodules`文件位于Git工作树的顶级目录中，是一个文本文件

```bash
# 参考 git submodules
```

# Email (TODO)

## git am

```bash
git am [--signoff] [--keep] [--[no-]keep-cr] [--[no-]utf8]
	 [--[no-]3way] [--interactive] [--committer-date-is-author-date]
	 [--ignore-date] [--ignore-space-change | --ignore-whitespace]
	 [--whitespace=<option>] [-C<n>] [-p<n>] [--directory=<dir>]
	 [--exclude=<path>] [--include=<path>] [--reject] [-q | --quiet]
	 [--[no-]scissors] [-S[<keyid>]] [--patch-format=<format>]
	 [--quoted-cr=<action>]
	 [--empty=(stop|drop|keep)]
	 [(<mbox> | <Maildir>)…​]
git am (--continue | --skip | --abort | --quit | --show-current-patch[=(diff|raw)] | --allow-empty)
```

## git format-patch

```
git format-patch [-k] [(-o|--output-directory) <dir> | --stdout]
		   [--no-thread | --thread[=<style>]]
		   [(--attach|--inline)[=<boundary>] | --no-attach]
		   [-s | --signoff]
		   [--signature=<signature> | --no-signature]
		   [--signature-file=<file>]
		   [-n | --numbered | -N | --no-numbered]
		   [--start-number <n>] [--numbered-files]
		   [--in-reply-to=Message-Id] [--suffix=.<sfx>]
		   [--ignore-if-in-upstream]
		   [--rfc] [--subject-prefix=Subject-Prefix]
		   [(--reroll-count|-v) <n>]
		   [--to=<email>] [--cc=<email>]
		   [--[no-]cover-letter] [--quiet] [--notes[=<ref>]]
		   [--interdiff=<previous>]
		   [--range-diff=<previous> [--creation-factor=<percent>]]
		   [--progress]
		   [<common diff options>]
		   [ <since> | <revision range> ]
```

 ## git send-email

```bash
git send-email [<options>] <file|directory>…​
git send-email [<options>] <format-patch options>
git send-email --dump-aliases
```

## git request-pull

```bash
git request-pull [-p] <start> <url> [<end>]
```

# 外部系统

## git svn 与svn系统之间的操作

```bash
git svn <command> [<options>] [<arguments>]
```

## git fast-import - 快速Git数据导入器的后端

# 管理

## :point_right:git clean - 从工作树中删除未跟踪的文件

```bash
git clean [-d] [-f] [-i] [-n] [-q] [-e <pattern>] [-x | -X] [--] [<pathspec>…]
```

常用命令

```BASH
git clean -f <path>
git clean -df
git clean -Xf
```

## :point_right:git gc - 清理不必要的文件并优化本地存储库

```bash
git gc [--aggressive] [--auto] [--quiet] [--prune=<date> | --no-prune] [--force] [--keep-largest-pack]
```

## git fsck -  验证数据库中对象的连通性和有效性

可以检查文件是否损坏，保持git的可靠性。

## git reflog - 管理reflog信息

## git filter-branch - 重写分支

## git instaweb - 在gitweb中

## git archive - 从命名树创建文件存档

## git bundle - 通过归档移动对象和引用

# 服务器管理

## git daemon - Git服务器

```bash
git daemon [--verbose] [--syslog] [--export-all]
	     [--timeout=<n>] [--init-timeout=<n>] [--max-connections=<n>]
	     [--strict-paths] [--base-path=<path>] [--base-path-relaxed]
	     [--user-path | --user-path=<path>]
	     [--interpolated-path=<pathtemplate>]
	     [--reuseaddr] [--detach] [--pid-file=<file>]
	     [--enable=<service>] [--disable=<service>]
	     [--allow-override=<service>] [--forbid-override=<service>]
	     [--access-hook=<path>] [--[no-]informative-errors]
	     [--inetd |
	      [--listen=<host_or_ipaddr>] [--port=<n>]
	      [--user=<user> [--group=<group>]]]
	     [--log-destination=(stderr|syslog|none)]
	     [<directory>…​]
```

## git update-server-info - 更新服务器信息

```
git update-server-info [-f | --force]
```

# 管道命令

## git cat-file - 提供存储库对象的内容或类型和大小信息

```bash
git cat-file <type> <object>
git cat-file (-e | -p) <object>
git cat-file (-t | -s) [--allow-unknown-type] <object>
git cat-file (--batch | --batch-check | --batch-command) [--batch-all-objects]
	     [--buffer] [--follow-symlinks] [--unordered]
	     [--textconv | --filters] [-z]
git cat-file (--textconv | --filters)
	     [<rev>:<path|tree-ish> | --path=<path|tree-ish> <rev>
```

## git check-ignore - 调试gitignore / exclude文件

```bash
git check-ignore [<options>] <pathname>…
git check-ignore [<options>] --stdin
```

## git checkout-index - 将文件从索引复制到工作树

```bash
git checkout-index [-u] [-q] [-a] [-f] [-n] [--prefix=<string>]
		   [--stage=<number>|all]
		   [--temp]
		   [-z] [--stdin]
		   [--] [<file>…]
```

## git commit-tree -  创建一个新的提交对象

```bash
git commit-tree <tree> [(-p <parent>)…]
git commit-tree [(-p <parent>)…] [-S[<keyid>]] [(-m <message>)…​]
		  [(-F <file>)…] <tree>
```

## git count-objects - 计算解压缩的对象数及其磁盘消耗

```bash
git count-objects [-v] [-H | --human-readable]
```

## git diff-index - 将树与工作树或索引进行比较

```bash
git diff-index [-m] [--cached] [<common diff options>] <tree-ish> [<path>…​]
```

## git for-each-ref - 每个参考的输出信息

```bash
git for-each-ref [--count=<count>] [--shell|--perl|--python|--tcl]
		   [(--sort=<key>)…] [--format=<format>] [<pattern>…​]
		   [--points-at=<object>]
		   (--merged[=<object>] | --no-merged[=<object>])
		   [--contains[=<object>]] [--no-contains[=<object>]]
```

## git hash-object - 计算对象ID，并可选择从文件创建blob

```b
git hash-object [-t <type>] [-w] [--path=<file>|--no-filters] [--stdin [--literally]] [--] <file>…​
git hash-object [-t <type>] [-w] --stdin-paths [--no-filters]
```

## git ls-files - 显示索引和工作树中的文件信息

```bash
git ls-files [-z] [-t] [-v] [-f]
		(--[cached|deleted|others|ignored|stage|unmerged|killed|modified])*
		(-[c|d|o|i|s|u|k|m])*
		[--eol]
		[-x <pattern>|--exclude=<pattern>]
		[-X <file>|--exclude-from=<file>]
		[--exclude-per-directory=<file>]
		[--exclude-standard]
		[--error-unmatch] [--with-tree=<tree-ish>]
		[--full-name] [--recurse-submodules]
		[--abbrev] [--] [<file>…]
```

## git ls-tree - 列出树对象的内容

```bash
git ls-tree [-d] [-r] [-t] [-l] [-z]
	    [--name-only] [--name-status] [--object-only] [--full-name] [--full-tree] [--abbrev[=<n>]] [--format=<format>]
	    <tree-ish> [<path>…]
```

## git merge-base - 为合并找到尽可能好的共同祖先

```bash
git merge-base [-a|--all] <commit> <commit>…
git merge-base [-a|--all] --octopus <commit>…
git merge-base --is-ancestor <commit> <commit>
git merge-base --independent <commit>…
git merge-base --fork-point <ref> [<commit>]
```

## git read-tree - 将树信息读入索引

```bash
git read-tree [[-m [--trivial] [--aggressive] | --reset | --prefix=<prefix>]
		[-u [--exclude-per-directory=<gitignore>] | -i]]
		[--index-output=<file>] [--no-sparse-checkout]
		(--empty | <tree-ish1> [<tree-ish2> [<tree-ish3>]])
```

## git rev-list - 以反向时间顺序列出提交对象

```bash
git rev-list [ --max-count=<number> ]
	     [ --skip=<number> ]
	     [ --max-age=<timestamp> ]
	     [ --min-age=<timestamp> ]
	     [ --sparse ]
	     [ --merges ]
	     [ --no-merges ]
	     [ --min-parents=<number> ]
	     [ --no-min-parents ]
	     [ --max-parents=<number> ]
	     [ --no-max-parents ]
	     [ --first-parent ]
	     [ --remove-empty ]
	     [ --full-history ]
	     [ --not ]
	     [ --all ]
	     [ --branches[=<pattern>] ]
	     [ --tags[=<pattern>] ]
	     [ --remotes[=<pattern>] ]
	     [ --glob=<glob-pattern> ]
	     [ --ignore-missing ]
	     [ --stdin ]
	     [ --quiet ]
	     [ --topo-order ]
	     [ --parents ]
	     [ --timestamp ]
	     [ --left-right ]
	     [ --left-only ]
	     [ --right-only ]
	     [ --cherry-mark ]
	     [ --cherry-pick ]
	     [ --encoding=<encoding> ]
	     [ --(author|committer|grep)=<pattern> ]
	     [ --regexp-ignore-case | -i ]
	     [ --extended-regexp | -E ]
	     [ --fixed-strings | -F ]
	     [ --date=<format>]
	     [ [ --objects | --objects-edge | --objects-edge-aggressive ]
	       [ --unpacked ]
	       [ --filter=<filter-spec> [ --filter-print-omitted ] ] ]
	     [ --missing=<missing-action> ]
	     [ --pretty | --header ]
	     [ --bisect ]
	     [ --bisect-vars ]
	     [ --bisect-all ]
	     [ --merge ]
	     [ --reverse ]
	     [ --walk-reflogs ]
	     [ --no-walk ] [ --do-walk ]
	     [ --count ]
	     [ --use-bitmap-index ]
	     <commit>… [ -- <paths>… ]
```

## git rev-parse -  挑选和按摩参数

```bash
git rev-parse [<options>] <args>…
```

## git show-ref - 列出本地存储库中的引用

```bash
git show-ref [-q|--quiet] [--verify] [--head] [-d|--dereference]
	     [-s|--hash[=<n>]] [--abbrev[=<n>]] [--tags]
	     [--heads] [--] [<pattern>…​]
git show-ref --exclude-existing[=<pattern>]
```

## git symbolic-ref -  读取，修改和删除符号引用

```bash
git symbolic-ref [-m <reason>] <name> <ref>
git symbolic-ref [-q] [--short] <name>
git symbolic-ref --delete [-q] <name>
```

## git update-index - 将工作树中的文件内容注册到索引

```bash
git update-index
	     [--add] [--remove | --force-remove] [--replace]
	     [--refresh] [-q] [--unmerged] [--ignore-missing]
	     [(--cacheinfo <mode>,<object>,<file>)…​]
	     [--chmod=(+|-)x]
	     [--[no-]assume-unchanged]
	     [--[no-]skip-worktree]
	     [--[no-]fsmonitor-valid]
	     [--ignore-submodules]
	     [--[no-]split-index]
	     [--[no-|test-|force-]untracked-cache]
	     [--[no-]fsmonitor]
	     [--really-refresh] [--unresolve] [--again | -g]
	     [--info-only] [--index-info]
	     [-z] [--stdin] [--index-version <n>]
	     [--verbose]
	     [--] [<file>…]
```

## git update-ref - 安全地更新存储在ref中的对象名

```bash
git update-ref [-m <reason>] [--no-deref] (-d <ref> [<oldvalue>] | [--create-reflog] <ref> <newvalue> [<oldvalue>] | --stdin [-z])
```

## git verify-pack -  验证打包的Git存档文件

```bash
git verify-pack [-v|--verbose] [-s|--stat-only] [--] <pack>.idx …​
```

## git write-tree - 从当前索引创建树对象

```bash
git write-tree [--missing-ok] [--prefix=<prefix>/]
```

