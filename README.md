# Feed Snippet

The feedsnippet is a tool for displaying the latest feed snippet in README.md, and so on.
It creates a markdown feed snippet from an RSS, ATOM, or JSON feeds and replace the old snippet with the new one.
The format of the generated feed can be formatted using the text template.

# Install

```
go install github.com/ikawaha/feedsnippet@latest
```

# Config

## Listing multiple feeds

```yaml
- urls:
    - https://zenn.dev/ikawaha/feed
  template: |-
    **Zenn**
    {{range . -}}
      * ![](./icon/zenn.png) [{{ .Title }}]({{ .Link }})
    {{ end }}
  limit: 5
  sort_by_published: true
  reverse: true
- urls:
    - https://qiita.com/ikawaha/feed
  template: |-
    **Qiita**
    {{range . -}}
      * ![](./icon/qiita.png) [{{ .Title }}]({{ .Link }})
    {{ end }}
  limit: 5
  sort_by_published: true
  reverse: true
```

Outputs:

#### Zenn
* ![](./icon/zenn.png) [その文字が JIS X 0208 に含まれるか？ あるいは unicode.RangeTable の使い方](https://zenn.dev/ikawaha/articles/20210116-ab1ac4a692ae8bb4d9cf)
* ![](./icon/zenn.png) [Go製全文検索エンジンBlugeで日本語形態素解析をおこなう](https://zenn.dev/ikawaha/articles/20201230-84b042603ccbbce645d5)
* ![](./icon/zenn.png) [実践：形態素解析 kagome v2](https://zenn.dev/ikawaha/books/kagome-v2-japanese-tokenizer)
* ![](./icon/zenn.png) [形態素解析器 kagome を Google App Engine で動かす](https://zenn.dev/ikawaha/articles/hatena-20161004-221708)
* ![](./icon/zenn.png) [形態素解析器 kagome を brew tap でインストールできるようにした](https://zenn.dev/ikawaha/articles/20201029-391c049a13fb8506361d)
#### Qiita
* ![](./icon/qiita.png) [Qiitaの記事をZenn形式のMarkdownで保存して管理する](https://qiita.com/ikawaha/items/ab9906581e34f26993a9)
* ![](./icon/qiita.png) [Goa v3 のテストをシュッとする](https://qiita.com/ikawaha/items/e0c2b3ed0fedb12f4847)
* ![](./icon/qiita.png) [人生で何度目かのダブル配列TRIEを書いた](https://qiita.com/ikawaha/items/edb4e18960ae6e4babc3)
* ![](./icon/qiita.png) [形態素解析器 kagome のユーザー辞書の使い方](https://qiita.com/ikawaha/items/9ebe3e1104fb80706d99)
* ![](./icon/qiita.png) [goa でデザイン・ファーストをシュッとする](https://qiita.com/ikawaha/items/6638ee8b6978aef50d65)


## Mixing multiple feeds

```yaml
- urls:
    - https://zenn.dev/ikawaha/feed
    - https://qiita.com/ikawaha/feed
  template: |-
    {{range . -}}
      * {{ if eq .Header.FeedLink "https://zenn.dev/ikawaha/feed" -}}
            ![](./icon/zenn.png)
        {{- else }}{{ if eq .Header.FeedLink "https://qiita.com/ikawaha/feed" -}}
            ![](./icon/qiita.png)
        {{- end }}{{ end -}}
      [{{ .Title }}]({{ .Link }})
    {{ end }}
  limit: 10
  sort_by_published: true
  reverse: true
```

Outputs:

* ![](./icon/zenn.png)[その文字が JIS X 0208 に含まれるか？ あるいは unicode.RangeTable の使い方](https://zenn.dev/ikawaha/articles/20210116-ab1ac4a692ae8bb4d9cf)
* ![](./icon/zenn.png)[Go製全文検索エンジンBlugeで日本語形態素解析をおこなう](https://zenn.dev/ikawaha/articles/20201230-84b042603ccbbce645d5)
* ![](./icon/zenn.png)[実践：形態素解析 kagome v2](https://zenn.dev/ikawaha/books/kagome-v2-japanese-tokenizer)
* ![](./icon/zenn.png)[形態素解析器 kagome を Google App Engine で動かす](https://zenn.dev/ikawaha/articles/hatena-20161004-221708)
* ![](./icon/zenn.png)[形態素解析器 kagome を brew tap でインストールできるようにした](https://zenn.dev/ikawaha/articles/20201029-391c049a13fb8506361d)
* ![](./icon/zenn.png)[Goa v3 入門](https://zenn.dev/ikawaha/books/goa-design-v3)
* ![](./icon/zenn.png)[Qiita/はてブの記事をZennでも管理する](https://zenn.dev/ikawaha/articles/20201012-e56b19cd33c396ae0465)
* ![](./icon/zenn.png)[はてなの記事をZenn形式のMarkdownで保存して管理する](https://zenn.dev/ikawaha/articles/hatena-20201012-205602)
* ![](./icon/qiita.png)[Qiitaの記事をZenn形式のMarkdownで保存して管理する](https://qiita.com/ikawaha/items/ab9906581e34f26993a9)
* ![](./icon/zenn.png)[Qiitaの記事をZenn形式のMarkdownで保存して管理する](https://zenn.dev/ikawaha/articles/qiita-ab9906581e34f26993a9)

# Automatic update of README.md using github workflow

```yaml
name: Update feed snippet

on:
  workflow_dispatch:
  schedule:
    - cron:  '0 * * * *'  

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Set up Go
      uses: actions/setup-go@v2
      with:
        go-version: 1.16

    - name: Install feedsnippet
      run: go install github.com/ikawaha/feedsnippet

    - name: Update README.md
      run: feedsnippet -config feedsnippet.yml -file README.md

    - name: git commit
      run: |
        git config --local user.email "ikawaha@users.noreply.github.com"
        git config --local user.name "ikawaha"
        git add README.md
        git commit -m "update posts"
        git push origin main
```

Outputs:


<!--[START github.com/ikawaha/feedsnippet]--><!--[2021-02-20T21:13:10Z]-->
* ![](./icon/zenn.png)[その文字が JIS X 0208 に含まれるか？ あるいは unicode.RangeTable の使い方](https://zenn.dev/ikawaha/articles/20210116-ab1ac4a692ae8bb4d9cf)
* ![](./icon/zenn.png)[Go製全文検索エンジンBlugeで日本語形態素解析をおこなう](https://zenn.dev/ikawaha/articles/20201230-84b042603ccbbce645d5)
* ![](./icon/zenn.png)[実践：形態素解析 kagome v2](https://zenn.dev/ikawaha/books/kagome-v2-japanese-tokenizer)
* ![](./icon/zenn.png)[形態素解析器 kagome を Google App Engine で動かす](https://zenn.dev/ikawaha/articles/hatena-20161004-221708)
* ![](./icon/zenn.png)[形態素解析器 kagome を brew tap でインストールできるようにした](https://zenn.dev/ikawaha/articles/20201029-391c049a13fb8506361d)
* ![](./icon/zenn.png)[Goa v3 入門](https://zenn.dev/ikawaha/books/goa-design-v3)
* ![](./icon/zenn.png)[Qiita/はてブの記事をZennでも管理する](https://zenn.dev/ikawaha/articles/20201012-e56b19cd33c396ae0465)
* ![](./icon/zenn.png)[はてなの記事をZenn形式のMarkdownで保存して管理する](https://zenn.dev/ikawaha/articles/hatena-20201012-205602)
* ![](./icon/qiita.png)[Qiitaの記事をZenn形式のMarkdownで保存して管理する](https://qiita.com/ikawaha/items/ab9906581e34f26993a9)
* ![](./icon/zenn.png)[Qiitaの記事をZenn形式のMarkdownで保存して管理する](https://zenn.dev/ikawaha/articles/qiita-ab9906581e34f26993a9)
<!--[END github.com/ikawaha/feedsnippet]-->









---
MIT