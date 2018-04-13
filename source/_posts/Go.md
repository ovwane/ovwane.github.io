#[Go](https://golang.org/)
安装Go

`brew install go`

安装分布式管理工具hg

 `brew install hg`
 
建立go的环境变量文件夹

```
cd $HOME

mkdir go
```

~/.zshrc

```
vim ~/.zshrc

export GOPATH=$HOME/go
export PATH=$HOME/bin:$GOPATH/bin:$PATH
```


`brew cask install goland`