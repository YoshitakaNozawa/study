以下は2021年11月19日 Qiita掲載記事です。
https://qiita.com/NozawaYoshitaka/items/89fe2c7da2097d50fed5
---
#  【ARKit】「空間座標の情報をもった写真がとれる連写機能実装してよ」に応えてみた

## 執筆の経緯
- 2年前に[BabyScale](https://apps.apple.com/jp/app/BabyScale/id1491116366?mt=8)というiOSアプリを作ったんですが、そのときに「空間座標の情報をもった写真がとれる連写機能実装してよ」という要望に頑張って応えて知見が貯まったのにもかかわらず、忙しさを言い訳に社内に情報共有していなかった。
- 今（本記事執筆時）ググってみても情報が出てこなかったので、公開する価値はあるかもしれない。いや、せめて社内に共有する必要はあるのでは？
- 社内で技術ブログを書くことになった。これはいい機会かもなぁ。

## 対象となる読者

- 空間座標の情報を含んだ写真を連続撮影する（連写）機能を実装したい人
- 必要な機能がライブラリになかったときに絶望せずになんとかする方法を知りたい人

となります。ニッチ！


## TL;DR
- ARKitのライブラリに連写機能がなかったから自分で作ったよ
- 何を考えて、何をして、自分で作れるようになったかの過程を公開するよ
- 実装はこんな感じだよ

## 「空間座標の情報をもった写真がとれる連写機能実装してよ」に応えたい
[BabyScale](https://apps.apple.com/jp/app/BabyScale/id1491116366#?platform=iphone)という、スマホで簡単に赤ちゃんの身長計測ができて、写真とともに記録もできるアプリを作りました。

![スクリーンショット 2021-11-19 10.37.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/593aad45-a771-6132-6d1a-2ae01b2ebcc3.png)


初回リリース直後の身長計測方法は

1. iPhoneの外側カメラで赤ちゃんの静止画を撮影（通常のカメラアプリと同じように）
2. 静止画上で長さを測りたい部分をタップ（上図参照：頭のてっぺん→首→お股→ひざ→かかと）

という流れなんですが、1の静止画撮影が難しいという意見を社内のママさんエンジニアからいただきました。

「赤ちゃんが動いちゃうから写真がブレちゃう。何度もとり直すの面倒。連写機能あったら楽かも？」

社内のエンジニアからの意見でしたが、ひとりのユーザーさんがそう言うんだから、エンジニアとしては困りごとを解決したい気持ちになりますよね。

ARKitに連写機能のライブラリはありませんでしたので、「技術的にどうやって実現するん？これ？」と困りましたが、必死に頭を働かせた結果、「とりあえず手を動かそう。わからない部分は調査しよう。」という至極当然な結論に至りました。

## なんもわからんの状態から何をしたか
まずは、何がわからないのか、何はわかっているのか、を洗い出しました。

- わからない： そもそも一般的な連写機能ってどうやって実装するの？
- わかる： 空間座標の情報をもった写真（フレーム）をとれるようにする必要がある

なんもわからん！ってなったときに私はいつもこのように洗い出しを行なってから、わからない部分の調査をし、理解を深めていきます。調査をして理解がすすむと、最初は見えていなかったわからない部分が新たに見えてきます。
わからない部分の洗い出し→調査→わからない部分の洗い出し→... を繰り返すことで、どうすればいいか見えるようになってきます。

### 一般的な連写機能
<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/556658/60e49996-6a40-d8da-868b-15b48e9c46a4.png" width=70%>
iPhoneのカメラは1秒間に30もしくは60フレーム（静止画）を取得できるのですが、それを間引いて取得し、シャッター音を出しているのが連写機能の仕組みとなります。
非常にシンプルなものであることがわかりました。

### 座標の情報を含んだ写真を連続撮影する（連写）機能
次に、座標の情報を含んだ写真を連続撮影する（連写）機能はどうやったら実現するのか？を考えました。
ARKitのライブラリを調査したところ、ARフレームを1秒間に60フレーム取得できることがわかりました。
「一般的な連写機能と同じ要領でARフレームを間引いて取得するようにしたら実現するじゃん！」となったので、実装できそうなことがわかりました。

このアイデアが出るまでは心の中と外で300回くらい「無理じゃね？」と呟いてましたが、なんとかなってよかったです。

## 実装例
連写が終わったあとは、取得した複数枚の画像の中からどれかを選択する工程が入ります。
以下2つの実装例を載せます。

- ARSessionのframe画像を間引いて取得する（連写機能）[^1][^2][^3][^4]
- 取り出した複数frameをスクロールviewで表示する & 選択する[^10][^11]

### 開発環境
開発当時の環境になります。（今でも動くとは思います。）

```
iOS: 13.2.3
Xcode: 11.2
Swift: 5.1.2
```

### ARSessionのframe画像を間引いて取得する（連写機能）

``` Swift
import UIKit
import SceneKit
import ARKit
import AVFoundation

class ViewController: ARSCNViewDelegate {

    @IBOutlet var sceneView: ARSCNView!
    @IBOutlet weak var stopBtn: UIButton!

    var stopFlg: Bool = false   // true: stop, false: run
    var continuousShootingFlg: Bool = false   // true: run, false: stop
    var counter: Int = 0
    var frameImages : Array<UIImage> = []
    var tmpframeImages : Array<UIImage> = []
    var selectedImage : CGImage?

    override func viewDidLoad() {
        super.viewDidLoad()

        sceneView.delegate = self
        sceneView.session.delegate = self
        // シーンを生成してARSCNViewにセット
        sceneView.scene = SCNScene()
        
        // 諸々の初期化
        counter = 0
        stopFlg = false
        continuousShootingFlg = false
        numSphere = 0
        returnBtn.isHidden = true
        stopBtn.isHidden = true
        albumBtn.isHidden = false
        helpBtn.isHidden = false
        statusLabel.isHidden = true
        tmpframeImages = []
        
        // セッション開始
        let configuration = ARWorldTrackingConfiguration()
        //        configuration.planeDetection = [.horizontal, .vertical]   // for iOS 11.3 or later
        configuration.planeDetection = .horizontal
        configuration.isLightEstimationEnabled = true
        sceneView.session.run(configuration, options: [.resetTracking, .removeExistingAnchors])
    }
}

extension ViewController: ARSessionDelegate {
    // ARFrameの更新時(60fps)
    func session(_ session: ARSession, didUpdate frame: ARFrame) {
        // stopBtnをタップすると起動する
        if (continuousShootingFlg && stopFlg) {            
            if ( counter % 6 == 0 ){ // (1/60 * 6)秒ごとに処理する
                // シャッター音
                AudioServicesPlaySystemSound(1108);
                // ARframeの画像を取得する
                guard let currentFrame = sceneView.session.currentFrame else {
                    print("Error: Current frame is nil. [\(#function)]")
                    return
                }
                // 表示時には90度回転する
                let ciImage = CIImage(cvPixelBuffer: currentFrame.capturedImage).oriented(.right)
                // CIImge -> CGIImage
                let context = CIContext()
                if let cgImage = context.createCGImage(ciImage, from: ciImage.extent) {
                    let frameImage = UIImage(cgImage: cgImage)
                    frameImages.append(frameImage)
                }
            }
            
            if ( counter == 54 ){ // 54 (= (10 - 1) * 6) frame取得したらsessionをpause
                sceneView.session.pause()
                
                let nextVC = self.storyboard?.instantiateViewController(withIdentifier: "ARFrames") as! FramesViewController
                nextVC.images = frameImages
                // tmpリストに一時的に格納
                tmpframeImages = frameImages
                nextVC.modalPresentationStyle = .fullScreen
                self.present(nextVC, animated: true, completion: nil)
            }
            
            counter += 1
        }
    }
}

```

- `func session(_ session: ARSession, didUpdate frame: ARFrame)`を使えばARFrameの更新のたびに、function内の記述が実行されます。
(`sceneView.session.delegate = self`を記述するのを忘れていて、「Frameが取り出せない！」って一人で騒いでしまったのは秘密)
- `if (continuousShootingFlg && stopFlg)`で連写撮影終了後にFrameから画像を取り出すようにしています。
- `Int: counter`を設置することによって、自由に取り出したいframeを設定できるようにしています。
上の場合は、1/10secごとに10frameを取得しています。

### 取り出した複数frameをスクロールviewで表示する & 選択する

新しく作ったUIViewController（`FramesViewController `）で以下を記述します。
なお、`images`は連写撮影で取得した10枚の画像のことです。これらは画面遷移前に渡しています。
複数の画像を渡して表示しているくらいの内容なので、解説は省かせていただきます。

``` Swift
import UIKit

class FramesViewController: UIViewController {

    var images: [UIImage]?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let scrollView = UIScrollView()
        var numFlame: Int = 0
        
        let scrollView = UIScrollView()
        let frameWidth = UIScreen.main.bounds.width
        let frameHeight = UIScreen.main.bounds.height
        
        scrollView.frame = CGRect(
            x: 0.0,
            y: 0.0,
            width: frameWidth,
            height: frameHeight
        )
        
        scrollView.delegate = self
        // 10枚のframeをscrollViewに渡す
        for i in 0...9 {
            numFlame += 1
            let ImageView = UIImageView(image: images?[i])
            
            ImageView.frame = CGRect(x: frameWidth * CGFloat(i),
                                     y: 0.0,
                                     width: frameWidth,
                                     height: frameHeight)
            
            ImageView.contentMode = UIView.ContentMode.scaleAspectFit
            scrollView.addSubview(ImageView)
        }
        
        scrollView.contentSize = CGSize(width: frameWidth * CGFloat(numFlame),
                                        height: frameHeight)
        scrollView.isPagingEnabled = true
        // スクロールビューを追加
        self.view.addSubview(scrollView)
    }
}
```

## 最後に
以上のように、「空間座標の情報をもった写真がとれる連写機能実装してよ」に応えてみました。
大変でしたが、なんもわからん状態からできるようになった状態への変化は非常に楽しいものでした。
今後も何か知見が貯まったらアウトプットしていきたいなぁと思います（やるとは言ってない）。


[^1]: https://qiita.com/eKushida/items/a36c77548e95531307ab
[^2]: https://qiita.com/m-yamada1992/items/ec5d6d9f94d83d251f0d
[^3]: https://teratail.com/questions/105970
[^4]: https://qiita.com/kotetuco/items/30f4c0eeea12ac3904a8

[^10]: http://norche.hatenablog.com/entry/2017/11/15/233807
[^11]: https://www.hfoasi8fje3.work/entry/2019/02/18/%E3%80%90Swift%E3%80%91%E6%A8%AA%E3%82%B9%E3%82%AF%E3%83%AD%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B%E3%83%9A%E3%83%BC%E3%82%B8%E3%82%92%E5%AE%9F%E8%A3%85%E3%81%99%E3%82%8B%28UIScrollView/UIPageControl%29
