# Rock_Paper_Scissors
<img src="figure/titlefigure.png">
Kaggle diary for Rock, Paper, Scissors competition

このリポジトリはRock, Paper, Scissors competitionのKaggle日記です。

## コンペ情報
強化学習コンペ、内容は極めてシンプル、ジャンケンを1000回行って、多く勝ったほうが勝ち。<br>
一見、究極の運ゲーと思ったが、他の人を出し抜くために色々な人が色々な工夫を凝らしてエージェントを
作成するので、randomを使う運ゲーでは上位に行くことはできない。
興味深いことに、完全な運ゲーであるのにも関わらず、(参加者の多さだけで判断しているだけだが)Santaコンペよりもレベルは高い気がする。なんか上に行くのは難しそう。

## Timeline
1/16 joined
2/2  deadline
Santa2020とdeadlineが同じ...

##　目標
金　上位12
銀　上位74
銅　上位148

one-chance 銅メダルの気持ちで、そこまで熱心にやるつもりはない。

## 結果
204/1663<br>
こちらはあと一つメダルに届かなかった。しかし、自分の考えた3エージェント混合モデルは、そこそこ上に登れたので、強化学習などが使えない割には善戦できたのではないか。<br>
とにかく、機械学習コンペと違うベクトルで難しいことが実感できた。
### 1/16
***
geometryカーネルと、np.randomを混ぜたエージェントを作成した。randomを選択する確率も乱数によって決定づけられている。閾値を0.2~0.6に変化させて、動向を確認した。
Notebook name: rock_paper_scissors_geometry_random

| ランダム率 | 名前 | スコア |
|----|----|----|
|0.2|version1|560|
|0.3|version2|820|
|0.4|version3|440|
|0.5|version4|610|
|0.6|中止|中止|

当たり前だが、簡単に1000付近には到達しない。次にgeometryカーネルと、last_opponent_killを混ぜたエージェントを作成した。また、ランダム率の幅も狭くした(同時に変えるのは本来は好ましくないが、まあまだ実験序盤だしいいかな)
Notebook name:rock_paper_scissors_geometry_last_opp_kill

| ランダム率 | 名前 | スコア |
|----|----|----|
|0.26|version6|550|
|0.28|version7|680|
|0.30|version8|500|
|0.32|version9|600|
|0.34|中止||

ここで、フォークしたもとのカーネルを見ると、スコアが1000を超えていた。元のがそんなに強いのか...しかし、上位に行くと読まれるであろう。ランダムは少し混ぜる。

| ランダム率 | 名前 | スコア |
|----|----|----|
|0|default|830|
|0.001|version10|470|
|0.05|version11|520|
|0.1|version12|650|

ランダムを混ぜても強くならないなこれ、もしかしたら壊れているのかもしれない。

### 1/21
***
フォークしたもとのカーネルを少し読んだが、何言っているか良くわからなかった。ただ、複素数を予測に使っていることだけ分かった。120°回転が手に対応している?
パラメータがいくつか指定できそうなので、こちらを主軸にしていく。新しいNotebookに変えた Notebook名:RockPaperScissor_geometry

| alpha | 名前 | スコア |
|----|----|----|
|0.01|version1|800|
|0.02|version2|750|
|0.03|version3|800|
|0.04|version4|660|
|0.05|version5|950|
|0.06|version6|860|

係数0.05が強そう。ここらへんで振動させるか、もっと係数を増やすか、守りに入るか...?

(いつかカーネルもちゃんと読んでみるか...)

### 1/22
***
とりあえず係数を増やす!!!
| alpha | 名前 | スコア |
|----|----|----|
|0.07|version7|750|
|0.08|version8|770|
|0.09|version9|680|
|0.10|version10|755|

### 1/23
***
微妙いな...0.05,0.06が強そうなので、その当たりで振動させる?????
| alpha | 名前 | スコア |
|----|----|----|
|0.052|version11|820|
|0.054|version12|820|
|0.056|version13|780|
|0.058|version14|715|
|0.06(二回目)|version15|660|

### 1/24
***
Cassavaのモデリングがうまく行かなくて辛い
ジャンケンはあきらめて、とりあえずversion6を大量提出する(運ゲー)。

### 1/26
***
今の所メダルは無理そう。放置して運勝ちを狙うしかない。

### 1/27
***
もととなってるカーネルの解説がdiscissionに存在した。
https://www.kaggle.com/c/rock-paper-scissors/discussion/210305
また、RPS Dojo(道場)という、良い実験ノートブックを発見した。
https://www.kaggle.com/chankhavu/rps-dojo/data

mixing
version1&version2 : bumblepuppy
0.01,0.09,0.06　bumblepuppy

時間がかかってしまったが、これ最強じゃね?っていう戦略を考えたので、試していきたい。
単純にいえば、混ぜる。複数のエージェントを裏で戦わせて、

### 1/28
***
1/27と1/28の分
とても時間かかったけど、3つ混ぜた、人生厳しい
mixing ver6,7
mixing2 ver 1,2,4
1:bumblepuppy&geometry alpha=0.01 2つ
2:bumblepuppy&geometry alpha=0.06 2つ
3 alpha=0.01 3つ
4 alpha=0.06 3つ
5 alpha=0.06 4つ

4つまぜたときは、そこまで時間かからなかった。

じゃんけんはもうこの4つアンサンブルした黒魔術エージェントに掛けるしかない...いけー
しかし、1/27に1つサブして分かったのは、結構負けるということ、理論上は引き分けは多くても負けることは殆どないと思っていたので、びっくり。

理由としては、後半相手が、今の自分に対して有利な形に変化してきても、累積のスコアで表に出すエージェントを決定しているので、後半になっても、今までの結果に囚われたままになってしまうため、うまくいかない。チクショー

解決策としては、決められた周期でlistをresetという策が考えられる。そうすれば、今現在の対戦結果に従って、エージェントを選出することができる。この周期は色々考えられるが、とりあえず、50と75と100で行ってみる。それぞれ二回試す→明日

### 1/29
***
Dojoでトレーニングしたら、クソ弱かった。50,100当たりではリセットが早すぎたか?
とりあえず、200,250,300で再チャレンジ→これもDojoでクソ弱い

どうする？→そもそもリストの末尾n個をjudgeに渡してやればいいのでは？？？(list[-最近のn個:]として渡してしまえばいい)
これでうまくいかなかったら私の負けです。(とはいえ複数回同じのsubmitするけど)

sub
mixing2:ver (現在一番強いやつ)
mixing2:ver (現在一番強いやつ)
mixing3:ver1 強そう
mixing3:ver10 弱そう
mixing3:ver11 弱そう
### 1/30
***
200 version12
250 version13
300 version14


弱かった、何でだろうと思っていたところ、痛恨のミスが有ったことが発覚。judgeで`[recent_(直近何個の対戦結果を用いるか決定する):]`(recentは正の数)としてしまっていた。本当は最後のn個を取り出さないといけないので`-recent_`としなければならなかったが、うっかりしていた。これを直せば上手くいくか？うまく行かなかったら私の負けです。というかこれに賭けるしかない。

200 version15 弱い
250 version16 弱い

### 1/31
***
やばい、手が尽きた...と思ったが、この方法なら、recent_をかなり小さい値にしても大丈夫なのでは？ということで、早く反応できるように、小さめの数字も試す。運が絡まない程度に大きく、素早く反応できる程度に小さいrecent_を見つけないといけないのだが...果たして
Dojoで試すと、強くなさそう、というか2つ混ぜたプログラムのほうが強そうなので、geometry&bundle＋直近抜き出しのプログラムで勝負
|recent_|version|score|
|----|----|----|
|20|version15|
|25|version16|
|30|version17|
|35|version18|
|40|version19|
ver19が少し強そう。
### 2/1
***
dlluは混ぜたほうが良いか?

|recent_|version|score|
|----|----|----|
|20|version20|
|25|version21|
|30|version22|
|35|version23|
|40|version24|

### 2/2
***
終了、歯が立たなかった。