# my-code-snippets

[MkDocs](https://github.com/mkdocs/mkdocs) で作成したコードスニペットをまとめたサイト.

ここにある Documents を Build するには Mkdocs が必要.

- MkDocs のインストール

以下のように pip でインストールすることができる.

```
$ sudo pip3 install mkdocs
```

-V もしくは --version オプションによりバージョンを表示できる.

バージョンを表示させることでインストールができたかどうかを試す.

```
$ mkdocs -V
mkdocs, version 1.0.4 from /usr/local/lib/python3.7/site-packages/mkdocs (Python 3.7)
```

- material theme を使う

```
$ sudo pip3 install mkdocs-material
```

- mathjax を使う

```
$ sudo pip3 install python-markdown-math
```

- ビルド

```
$ mkdocs build
```

生成した ファイルは http://kira924age.com/my-code-snippets/ にあげてる.

