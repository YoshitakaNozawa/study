以下は2020年08月06日 Qiita掲載記事です。
https://qiita.com/NozawaYoshitaka/items/0752b423379a09a2b3c4
---
# 野澤理論 爆誕

この記事は[「マイスター・ギルド：暑中見舞！夏のアドベントカレンダー2020」](https://note.com/meisterguild/n/n09826f484617) 5日目の記事です。


はじめまして。
マイスター・ギルド（以下、MGまたは弊社） 開発部のエンジニア　野澤です。

MGには去年の9月ごろからインターンとしてお世話になり、今年4月から正式に入社しました。

MGに入ってそこまで長くないのですが、楽しいイベント（事件）がいくつも起こっており、これは紹介しなくてはいけないと思い筆をとっております。
本記事では、中でも印象深かったものを技術の話をまじえてご紹介したいと思います。
(前半が物語、後半がちょっとした技術の話という構成です)

※注意
この記事は、
マイスター・ギルドって会社は愉快な社員が集まってるよ！楽しいよ！
ってことを伝えるためのものです。

間違っても、
　「そこの企業さん、こんなアプリ一緒に作りませんか？」
　「数字に強い私を引き抜きしませんか？ヘッドハント待ってるよ〜！」
とか、打算なんてこれっぽっちもありません。これっぽっちもないんだ。

いいね？


##野澤理論の誕生物語

物語の始まりは2020年4月某日のお昼でした。
弊社CTO（ちょっと、テクニカルな楽しい、おじさんの略、諸説あり）のSlackでの呟きから始まったのです。

![CTO_1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/25a60af5-47d4-fb6d-b93c-3115f632f293.png)


このつぶやきに、なぜか自分含め多くのメンバーが食いつきました。
23件も返信がついています！
（みんな、トイレットペーパーそんな好きなん？毎日使うから？）

弊社CTOも驚きを隠せないようです。

![CTO_2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/06e5afec-bced-91b9-5362-598beb384d89.png)

「可能ですね！」
「こうやればどうだろ？」
とか意見がいくつも上がりました。
頭の中はトイレットペーパー一色です。


この日の夜、床についたときにも何故か私の頭からトイレットペーパーが離れませんでした（毎日使うからかな？）。
「いけそうやなぁ。数式書いてみんとわからんけど」
という感じで、私は深い眠りについたのでした。


次の日の朝は、目覚まし時計が鳴る前に目が覚めました。不思議と爽やかな心地です。
私の頭の中には、やはりトイレットペーパーがありました。

目覚めのコーヒーを飲みながら、机に向かいます。
鉛筆を手に取り、頭のカオスを解きほぐすかのように筆を走らせました。
自分でもびっくりです。
筆がのるのる。

気づいたときには、次のような式が導かれていました。

![トイレットペーパー関数.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/396c041c-b369-e34e-1f5d-0368cc6794ec.png)
![トイレットペーパー関数2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/caee9b75-2bc2-b56e-d052-8c0788d9a3f3.png)

達成感に溢れていました。
トイレットペーパー関数なんて名前つけてます。
馬鹿みたいで驚きです。
MGメンバーに共有したい気持ちに駆られました。
地動説を提唱したコペルニクスも最初はこんな気持ちだったのではないかと思います。

始業30分ほど前に照れまじりに、私は以下のような投稿をしました。

![nozawa_slack.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/fb484d34-7bd6-0170-a4a6-6cebdb0debca.png)


「何、馬鹿なことしてんの？ 草」
と嘲笑される不安もありましたが、杞憂でした。

![nozawa_slack 2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/bcc52f8a-2c13-df92-f1c0-fcc9ad0af8e3.png)


「めっちゃ受け入れてくれるやん！！喜」
大阪人の血があふれんばかりの歓喜で心が叫びたがっているんだ。
否、叫んだ瞬間でした。

続々とコメントしてくれるメンバー

![nozawa_slack 3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/6ed59cda-4f07-38bc-6af3-2762c1707972.png)
![nozawa_slack 4.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/7240a1b3-e68c-5c7d-429a-119f39a7e1f9.png)
![nozawa_slack 5.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/760a5c01-bc89-bc68-9cbd-cd8b862f4ccd.png)

トイレットペーパー現象が生まれましたね。
他には

![nozawa_slack 6.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/768c54be-7954-bf91-7f70-dd345d2fc772.png)

はい、ここで来ました
＿人人人人人人＿
＞　野澤理論　＜
￣Y^Y^Y^Y^Y^Y￣
三田村さんが名付けてくれました。
トイレットペーパーの直径が半分になる前には残量が50%切ってるよって理論（なんだこの理論）

スタンプもできました（最高か？）

![riron.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/4409fc4c-7598-ba4d-733e-cd6114c2afef.png)

最近では、こんな使われ方している野澤理論スタンプ
![riron2.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/712d4cfe-e3d2-e4cd-3f27-f18002d73bc8.jpeg)

からの？→野澤理論

いじりに使われてます。笑

こんな感じで、自分の名前がついた理論（非公式）が爆誕したのでした。


以上のように、MGは優しくて愉快なメンバーが集まって、ゆるーく（だけども馴れ合わず）ソフトウェア開発を行なっています！


##どうやって式を作ったのか？
今回のようなトイレットペーパーの減りを表現した式をどのようにつくったのかについて、思考過程をお見せしたいと思います。

変化を表現した式をつくるには前提として、微分したら変化が求まることを知っている必要があります（高校の数学または物理で習います）。

### 思考過程
1. 断面積は計測しにくいから、直径使うと後々使い勝手よさそうやなぁ。
↓

2. 直径(D)と長さ(L)を使った式、なんでもいいから作られへんかな？
↓

3. 断面積をD, Lそれぞれ使って表したら等式作れるかなぁ。
↓

4. あ、いけた。
↓

5. Lで微分したろ。そしたら、Lに対しての変化でるなぁ。
↓

6. あ、いけた。

てな感じです。


## 実際に求めた式を使ってみた
上で書いたように、私はトイレットペーパーの直径を計測できたら残量がわかるようになる式を求めました。

では、実際に直径を測って、残量を調べてみようとなりますよね。
調べてみました。
ARアプリで！！！
ここで定規は使っちゃいけないんだ。

### 直径の計測方法
インターン期間中にチームで作った[BabyScale](https://www.m-gild.com/app/babyscale/)という赤ちゃんの身長を簡単に測れるiOSアプリを使って直径を測ってみます！
↓赤ちゃんを計測する例。タップするだけで簡単に計測完了！

![babyscale.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/5eb9ff67-8319-2ba8-fe83-be257997b2ee.png)

### ARアプリでトイレットペーパーの直径を測ってみた
かかとなんてないけど、タップする。
<img width="400" alt="toilet.jpg" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/9e73dda9-469c-127e-4f62-caaf83666341.jpeg">

できた！
<img width="400" alt="toilet2.jpg" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/718fc0cb-fc68-9ccb-471d-7fdbf5a21eda.jpeg">

8cmです！！

### 直径から残量求めてみた
では、気になる残量ですが、、、、Pythonで計算プログラムを書いてみました。
実行してみると、、、

``` Python3:toilet_paper.py
def get_remaining_quantity(diameter):
	"""
	diameter: diameter of a toilet paper roll (m)
	
	This function calculates remaining quantity of a toilet paper roll (%)
	given the diameter of a roll (m).
	"""
	# diameter of standard new roll
	MAX_DIAMETER = 110. * 10**-3
	# diameter of standard core　　　
	MIN_DIAMETER = 40. * 10**-3
	
	remaining_quantity = (diameter**2 - MIN_DIAMETER **2) * 100 / (MAX_DIAMETER**2 - MIN_DIAMETER **2)

	return remaining_quantity


roll_diameter = 80 * 10**-3
result = "{:.1f} %".format(get_remaining_quantity(roll_diameter))

print(result)

# result
# 45.7 %
```
45.7 %でした！！


皆さんもぜひ、[BabyScaleをインストール](https://apps.apple.com/jp/app/BabyScale/id1491116366?mt=8)して、トイレットペーパーの残量を調べてみてはいかがでしょうか？

## 最後に
最後まで読んでいただきありがとうございます。

MGは楽しい人が集まってるし、新しい技術を使った開発ができる会社です。
新しいものを受け入れてくれる環境がここにはあります。
社外向けなのに、こんな訳のわからないトイレットペーパーの記事を「いいんちゃ〜う」でOK出すくらい、いい意味で狂った会社です。

でも、それくらい狂ってた方が楽しい！以上！