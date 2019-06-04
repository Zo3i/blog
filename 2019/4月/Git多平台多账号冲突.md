> 自己的Commit还是要好好记录才是...由于公司使用的是coding,自己使用的是github
>
> 但是git的邮箱不同,导致提交的时候会有两个用户提交代码...

### 解决方案

- 下载git 裸库

  **git clone --bare xxx.git**

- 进入仓库

  **cd xxx.git**

- 运行脚本:

  ```shell
  #!/bin/sh
  git filter-branch --env-filter '
  OLD_EMAIL="tionsin@live.com"
  CORRECT_NAME="Zo3i"
  CORRECT_EMAIL="30296801+Zo3i@users.noreply.github.com"
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

- 查看新 Git 历史有没有错误。

  **git log**

- 把正确历史 push 到 Github

  **git push --force --tags origin 'refs/heads/*'**

### 注意点

修改完以后,再次提交可能被拒绝,所以有这样的冲突,还是建议直接写两个配置文件吧:

**git config --global user.name "Tionsin"**

**git config --global user.email "[tionsin#live.com](mailto:tionsin@live.com)"**