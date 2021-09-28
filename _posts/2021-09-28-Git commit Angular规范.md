---
layout: post
category: Linux
title: Git commit Angular规范
tags: 
  - Linux
  - Git
---

### Commit message 的格式
Commit message包括三个部分:
- Header
- Body
- Footer

```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

其中，Header 是**必需**的，Body 和 Footer 可以省略。

#### Header
Header包含三个字段
- type (必须) commit的类别
  - feat: 新功能 (feature)
  - fix: 修补bug
  - docs: 文档 (documentation)
  - style: 格式 (不影响代码运行的变动)
  - refactor: 重构 (既不是新增功能，也不是修改bug的代码变动)
  - test: 增加测试
  - chore: 构建过程或辅助工具的变动
  - revert: 回滚操作
    > 撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header
    ```
    revert: feat(pencil): add 'graphiteWidth' option

    This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
    ```
    > Body部分的格式是固定的，必须写成`This reverts commit <hash>.`，其中的hash是被撤销 commit 的 SHA 标识符。
- scope (可选) 说明commit影响的范围
- subject (必须) commit简短描述，不超过50个字符(动词开头，使用第一人称现在时，首字母小写，结尾不加句号)

#### Body
Body部分是对本次commit的详细描述，可以分成多行。

范例：

```
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

- *使用第一人称现在时，比如使用change而不是changed或changes*
- *应该说明代码变动的动机，以及与以前行为的对比*

**撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header**
```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

Body部分的格式是固定的，必须写成`This reverts commit <hash>.`，其中的hash是被撤销 commit 的 SHA 标识符。

#### Footer
Footer部分只用于两种情况

- 不兼容变动
  > 如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。
  ```
  BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
  ```
- 关闭Issue
  > 如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue
  ```shell
  # 关闭单个Issue
  Closes #234
  # 关闭多个Issue
  Closes #123, #245, #992
  ```

  ### 使用 commit template

  使用的[Linell编写的模板](https://gist.github.com/Linell/bd8100c4e04348c7966d)

  ```shell
  curl https://gist.githubusercontent.com/Linell/bd8100c4e04348c7966d/raw/84c0ea6e0f0a1431d406be6b7bb6e136949090cd/.git-commit-template.txt >> ~/.git-commit-template.txt
  git config --global commit.template ~/.git-commit-template.txt
  ```

  手动创建模板
  ```shell
  touch ~/.git-commit-template.txt
  # 打开.git-commit-template.txt,写入以下内容
  # 保存后执行
  git config --global commit.template ~/.git-commit-template.txt
  # 替换git commit编辑器为vim
  git config --global core.editor vim
  ```
  ```
  # Type(<scope>): <subject>

  # <body>

  # <footer>


  # Type should be one of the following:
  # * feat (new feature)
  # * fix (bug fix)
  # * docs (changes to documentation)
  # * style (formatting, missing semi colons, etc; no code change)
  # * refactor (refactoring production code)
  # * test (adding missing tests, refactoring tests; no production code change)
  # * chore (updating grunt tasks etc; no production code change)
  # * revert (must start with revert: followed by the headers that were uncommitted,fixed body: This reverts commit <hash>)
  # Scope is just the scope of the change. Something like (admin) or (teacher).
  # Subject should use impertivite tone and say what you did.
  # The body should go into detail about changes made.
  # The footer should contain any JIRA (or other tool) issue references or actions.
  ```

  ### 参考
  > [Commit message 和 Change log 编写指南 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
  > 
  > [Linell/.git-commit-template.txt](https://gist.github.com/Linell/bd8100c4e04348c7966d)