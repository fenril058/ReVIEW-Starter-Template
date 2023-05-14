# README

これは [Re:VIEW Starter](https://kauplan.org/reviewstarter/) で生成したRe:VIEW Starter用プロジェクトに
若干のツールを付け加えて運用しやすくしたTemplateです。

付け加えたのは以下のファイルです。
- Gemfile
- package.json
- .github/workflows/on_push.yml


## コンパイル方法

Github Actionsを用いてpushするたびにPDFを生成するようにしています。
DockerHubのkauplan/review2.5を使用しています。
詳しくは [.github/workflows/on_push.yml](.github/workflows/on_push.yml) を参照してください。


Localでのコンパイルは以下のようにDockerを使う場合と使わない場合の2通りありますが、
Dockerを使ったほうが安全です。
なぜならば、Ruby と Re:VIEW と LaTeX の version によっては問題が生じるからです。

Docker を使う場合：

```terminal
$ docker pull kauplan/review2.5    # 3GB 以上ダウンロードするので注意
$ git clone https://github.com/fenril058/ReVIEW-Starter-Template
$ cd ReVIEW-Starter-Template
$ docker run --rm -v $PWD:/work -w /work kauplan/review2.5 rake pdf
$ ## または rake docker:pdf でもOK
```

Docker を使わない場合 (Ruby と Re:VIEW と LaTeX が必要)：

```terminal
$ git clone https://github.com/fenril058/ReVIEW-Starter-Template
$ cd ReVIEW-Starter-Template
$ bundle install
$ bundle exec rake pdf
```

これで「mybook.pdf」が生成されます。

### Versionの差異によって生じる問題
1. Ruby 3.1でYAMLに関して非互換な変更が入っておりそれによりエラーが発生します。
   - 修正はそこまで大変ではありませんが素直に3.0.xを使うのがいまのところおすすめです。
2. LaTeXのversionによってはエラーが出る可能性があります。
3. LaTeXのversionが違うと組版結果に微妙な差異が生じる場合があります。


## 校正
Linterとして、textlintをつかった校正ができるようになっています。
Node.jsが必要です。

```terminal
$ npm install
$ npx textlint contents # contents/ 以下のすべてのRe:VIEWファイルを校正
$ ## npx textlint contents/ファイル名 で特定のファイルだけを校正
```

校正ルールは [.textlintrc.json](.textlintrc.json)に記述されています。
デフォルトでは[prh](https://github.com/prh/prh)から持ってきたルールも有効になっています。
適宜修正しながら使用してください。


## Re:VIEW Starterのオプション
[Re:VIEW Starter](https://kauplan.org/reviewstarter/)ではたくさんのオプションが選べますが、
このリポジトリでは以下のオプションを選んでいます。

- 著者名（サークル名）： 空白
- ヨミ：空白
- タイトル：空白
- サブタイトル：空白
- サイズ：A5
- フォントサイズ：9pt
- 章や目次を必ず右ページ始まりにする
- 章：行数：2行
- 章：寄せ：中央寄せ
- 章：装飾：上下に太い線
- 節：装飾：下線
- 項：装飾：なし, 項番号あり
- その他： 節や項がページ先頭のとき、タイトル上部を詰める
- プログラム用等幅フォント：beramono
- ターミナル用等幅フォント：inconsolata
- プログラムやターミナルの表示オプション
  - キャプションを見映えよくする
  - プログラム (//list) に枠線をつける
- その他のオプション
  - 目次をつける
  - 最初のページに「大扉」（タイトルページ）をつける
  - 最後のページに「奥付」（発行日などの表示）をつける
  - 奥付にひっそり「powered by Re:VIEW Starter」と入れる
  - デバッグモードにする （生成した中間LaTeXファイルを削除しない）
  - 画像が現在位置に入りきらず次のページに送られるとき、大きなスペースを空けない
  - 原稿ファイルを「contents」ディレクトリにまとめる
- 印刷所：○○印刷所

## Reference
- [Original の README.md](./README.orig.md)
