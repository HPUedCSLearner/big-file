[toc]







## git alias

```bash
git config --global alias.ci commit
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
```



## git with lfs

```bash
git lfs track dataset/eat_pytorch_datasets.zip
git lfs track ${path}/*.zip
```



```bash
git lfs install
git lfs track dataset/eat_pytorch_datasets.zip
git ci -m "add llm pdf & add lfs file eat pytorch datasets"
git push 
```



## git reset with --mixed

```bash
git reset 6294f1d --mixed
```



```bash
feng@feng-X99M-D3:~/github/big-file$ git log --oneline -22
386fa79 (HEAD -> main) add llm.pdf and eat pytorch datasets
6294f1d (origin/main, origin/HEAD) change docker markdown file
e61b3bc add markdown file
5ed9b25 add Task Optimeize Funlayer
b1e76ae add linux/pdf
f18403c Initial commit
feng@feng-X99M-D3:~/github/big-file$ git reset 6294f1d --mixed
feng@feng-X99M-D3:~/github/big-file$ git st
位于分支 main
您的分支与上游分支 'origin/main' 一致。

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）
	dataset/
	llm/

提交为空，但是存在尚未跟踪的文件（使用 "git add" 建立跟踪）
feng@feng-X99M-D3:~/github/big-file$ git lsf install
git：'lsf' 不是一个 git 命令。参见 'git --help'。

最相似的命令是
	lfs

```

