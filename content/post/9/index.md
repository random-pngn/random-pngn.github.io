---
title: 'らんだむちゃん'
# [必須]タイトル 短めで！
subtitle: '' # 記事のタイトルの下に表示
summary: 'らんだむちゃん描く' # Cardに表示 無かったら本文1行目が入る 短めで！
math: false # Tex いらないならfalse
diagram: true # グラフ描画 必要ならtrue
authors:
- YuKi
# [必須] 著者 一応複数人かけるけどアイコン表示は一番上の人だけ
tags:
- 'グラフィック'
# [必須] タグ 複数可 既にあるタグを使おう
 # 作品紹介系テンプレ: 作品紹介, ゲーム, 音楽, グラフィック
 # ブログ系テンプレ: 競技プログラミング, ゲーム制作, DTM, LT会
date: "2020-02-18T00:00:00Z"
# [必須]
lastmod: "2020-02-18T00:00:00Z"
# [必須] 今日の日付を入れよう(これを元に最新記事取ってきてるはず)
featured: false # ???
draft: false # true にすると非表示になる

# [必須] 同一フォルダにfeatured.jpg/pngを入れて! 絶対!

projects: [] # project関係あればその名前を入れる

# 本文の書き方はこれを参照して(https://sourcethemes.com/academic/docs/writing-markdown-latex/)
---
# はじめに

<link rel="stylesheet" href="TemplateData/style.css">
<script src="TemplateData/UnityProgress.js"></script>
<script src="Build/UnityLoader.js"></script>
<script>
  var unityInstance = UnityLoader.instantiate("unityContainer", "Build/Gift4Yukari3.json", {onProgress: UnityProgress});
</script>
<div class="webgl-content">
  <div id="unityContainer" style="width: 960px; height: 540px"></div>
  <div class="footer">
    <div class="webgl-logo"></div>
    <div class="fullscreen" onclick="unityInstance.SetFullscreen(1)"></div>
    <div class="title">Gift For Yukari-san</div>
  </div>
</div>

こんにちは. Yuです.
描くたびに顔が変わるらんだむちゃん描きました.

完成までの過程を比較すると面白いかも.
