---
title: Ruby 配置
date: 2017-09-23 14:27:29
tags:
---

[rbenv](https://github.com/rbenv/rbenv)

```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```



环境变量

```
echo 'export PATH="$HOME/.rbenv/bin:$PATH"\neval "$(rbenv init -)"' >> ~/.zshrc
```



ruby-build

```
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```



环境变量

```shell
export PATH="${HOME}/.rbenv/shims:${PATH}"
export RBENV_SHELL=zsh
source '${HOME}/.rbenv/libexec/../completions/rbenv.zsh'
command rbenv rehash 2>/dev/null
rbenv() {
  local command
  command="${1:-}"
  if [ "$#" -gt 0 ]; then
    shift
  fi

  case "$command" in
  rehash|shell)
    eval "$(rbenv "sh-$command" "$@")";;
  *)
    command rbenv "$command" "$@";;
  esac
}
```



查看 ruby 版本

```
rbenv install -l
```



安装 ruby

```
rbenv install 2.6.0
```

