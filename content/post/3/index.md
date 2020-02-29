---
title: 'らんだむすたじお！'
# [必須]タイトル 短めで！
subtitle: 'らんだむちゃん録画環境 in Unity' # 記事のタイトルの下に表示
summary: 'ランダムのWebサイトを作りました' # Cardに表示 無かったら本文1行目が入る 短めで！
math: false # Tex いらないならfalse
diagram: false # グラフ描画 必要ならtrue
authors:
- ATsU
# [必須] 著者 一応複数人かけるけどアイコン表示は一番上の人だけ
tags:
- 'グラフィック'
- 'らんだむちゃん'
- 'LT会'
- 'Vtuber計画'
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
Hello Random-chan!

らんだむちゃんVtuber計画進捗報告です. Unityで録画環境を作りました. 以下のようなことができます.

- kinectでのモーショントラッキング
- 表情切り替え, 自動まばたき
- ランタイム中の設定画面操作
- スライド表示, 切り替え
- 簡易カメラ操作

  使用したアセットは末尾に示します. また, Universal Rendering Pipelineを使用しているのでグラフィック面で少々仕様が違うことがあります.

# ここから部員向け
1. 操作(デフォルト)
- Esc:設定画面切り替え
- ↑, ↓:設定項目選択
- Space:決定
- →, ←:スライド切り替え
- c:カメラ切り替え
- w:口を開ける
- a, d:ウインク
- s:ジト目

2. 設定項目追加

- プロジェクトはスケルトンに入っています. それを編集するか, コピーしてとってもらえればと思います.
- (Assets/)Resources/Scripts/Setting.csで関数, パネルの追加を行えます. 詳しい方法はスクリプトを見るか直接聞いてください. 設定画面出し入れは空ゲームオブジェクトにStudioManagerをアタッチすればできます.
- (Assets/)Resources/Scripts/CameraMove.csをカメラにアタッチすることで位置の保存, ランタイム中の位置変更ができます. ポジションはインスペクタで設定できます.
- (Assets/)Resources/Scripts/SlideManager.csをImageコンポーネントにアタッチすることでスライドが使えます. スライドは現在Sprite(png, jpegなどをインポートして変換)のみ対応しています.

3. 動画に合わせていじるべきところ
- マテリアル:シェーダーは作成したもの(入力ベクトルに応じて2値化っぽくするもの)を使っていますが, 素のシェーダーの方がいいかもです.
- カメラ操作:ランタイム中の操作に合わせて実装してください.
- 何か欲しい機能があれば言ったら実装するかもしれません.

# まとめ
動画...作ろう...

#使用したアセット
1. kinect with MS-SDK

https://assetstore.unity.com/packages/tools/kinect-with-ms-sdk-7747
