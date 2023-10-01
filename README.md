<img src="https://github.com/kunishou/amenokaku-code-instruct/blob/main/image/amenokaku.png" alt="amenokaku_code">

# AmenokakuCode-instruct

## 概要
- コードに特化した5.2KのInstructionデータセットです。
- データセットに含まれるデータは商用利用できるラインセンスが付与されたプログラミング学習コンテンツから収集、加工し作成しました（英語のコンテンツは日本語に自動翻訳し、翻訳の不自然な箇所を手動で修正）。
- また、ライセンスが明記されていない学習コンテンツについては権利者に個別に連絡を取り、本データセットへの掲載の許諾を得ております。

## HuggingFace
https://huggingface.co/datasets/kunishou/amenokaku-code-instruct

## データセット詳細
指示タスクの内訳としてはコード生成（code_generation）が1050レコード、コードの挙動確認（check_code_behavor）が150レコード、コードのバグ修正（code_fix）が4000レコードになります。
詳細な内訳は下表の通りになります。

<details>
<summary>データ例1（コード生成）</summary>

```
Instruction:
  Pythonを用いて、renketsu.pngを4-連結数により、色分けせよ。
  
  4-連結数とは近傍との画素の状態を見る値である。
  通常、近傍は注目画素x0(x,y)が0でない場合に対して、次のように定義される。
  
  x4(x-1,y-1) x3(x,y-1) x2(x+1,y-1)
  x5(x-1,y)   x0(x,y)   x1(x+1,y)
  x6(x-1,y+1) x7(x,y+1) x8(x+1,y+1)
  
  ここで4連結数とは、次式で計算される。
  
  S = (x1 - x1 x2 x3) + (x3 - x3 x4 x5) + (x5 - x5 x6 x7) + (x7 - x7 x8 x1) 
  
  S = [0,4]の範囲をとり、
  - S = 0 は内部点
  - S = 1 は端点
  - S = 2 は連結点
  - S = 3 は分岐点
  - S = 4 は交差点
  を示す。

Output:

  import cv2
  import numpy as np
  import matplotlib.pyplot as plt
  
  
  # hilditch thining
  def hilditch(img):
      # get shape
      H, W, C = img.shape
  
      # prepare out image
      out = np.zeros((H, W), dtype=np.int)
      out[img[..., 0] > 0] = 1
  
      # inverse pixel value
      tmp = out.copy()
      _tmp = 1 - tmp
  
      count = 1
      while count > 0:
          count = 0
          tmp = out.copy()
          _tmp = 1 - tmp
  
          tmp2 = out.copy()
          _tmp2 = 1 - tmp2
          
          # each pixel
          for y in range(H):
              for x in range(W):
                  # skip black pixel
                  if out[y, x] < 1:
                      continue
                  
                  judge = 0
                  
                  ## condition 1
                  if (tmp[y, min(x+1, W-1)] * tmp[max(y-1,0 ), x] * tmp[y, max(x-1, 0)] * tmp[min(y+1, H-1), x]) == 0:
                      judge += 1
                      
                  ## condition 2
                  c = 0
                  c += (_tmp[y, min(x+1, W-1)] - _tmp[y, min(x+1, W-1)] * _tmp[max(y-1, 0), min(x+1, W-1)] * _tmp[max(y-1, 0), x])
                  c += (_tmp[max(y-1, 0), x] - _tmp[max(y-1, 0), x] * _tmp[max(y-1, 0), max(x-1, 0)] * _tmp[y, max(x-1, 0)])
                  c += (_tmp[y, max(x-1, 0)] - _tmp[y, max(x-1, 0)] * _tmp[min(y+1, H-1), max(x-1, 0)] * _tmp[min(y+1, H-1), x])
                  c += (_tmp[min(y+1, H-1), x] - _tmp[min(y+1, H-1), x] * _tmp[min(y+1, H-1), min(x+1, W-1)] * _tmp[y, min(x+1, W-1)])
                  if c == 1:
                      judge += 1
                      
                  ## condition 3
                  if np.sum(tmp[max(y-1, 0) : min(y+2, H), max(x-1, 0) : min(x+2, W)]) >= 3:
                      judge += 1
  
                  ## condition 4
                  if np.sum(out[max(y-1, 0) : min(y+2, H), max(x-1, 0) : min(x+2, W)]) >= 2:
                      judge += 1
  
                  ## condition 5
                  _tmp2 = 1 - out
  
                  c = 0
                  c += (_tmp2[y, min(x+1, W-1)] - _tmp2[y, min(x+1, W-1)] * _tmp2[max(y-1, 0), min(x+1, W-1)] * _tmp2[max(y-1, 0), x])
                  c += (_tmp2[max(y-1, 0), x] - _tmp2[max(y-1, 0), x] * (1 - tmp[max(y-1, 0), max(x-1, 0)]) * _tmp2[y, max(x-1, 0)])
                  c += (_tmp2[y, max(x-1, 0)] - _tmp2[y, max(x-1, 0)] * _tmp2[min(y+1, H-1), max(x-1, 0)] * _tmp2[min(y+1, H-1), x])
                  c += (_tmp2[min(y+1, H-1), x] - _tmp2[min(y+1, H-1), x] * _tmp2[min(y+1, H-1), min(x+1, W-1)] * _tmp2[y, min(x+1, W-1)])
                  if c == 1 or (out[max(y-1, 0), max(x-1,0 )] != tmp[max(y-1, 0), max(x-1, 0)]):
                      judge += 1
  
                  c = 0
                  c += (_tmp2[y, min(x+1, W-1)] - _tmp2[y, min(x+1, W-1)] * _tmp2[max(y-1, 0), min(x+1, W-1)] * (1 - tmp[max(y-1, 0), x]))
                  c += ((1-tmp[max(y-1, 0), x]) - (1 - tmp[max(y-1, 0), x]) * _tmp2[max(y-1, 0), max(x-1, 0)] * _tmp2[y, max(x-1, 0)])
                  c += (_tmp2[y, max(x-1,0 )] - _tmp2[y, max(x-1,0 )] * _tmp2[min(y+1, H-1), max(x-1, 0)] * _tmp2[min(y+1, H-1), x])
                  c += (_tmp2[min(y+1, H-1), x] - _tmp2[min(y+1, H-1), x] * _tmp2[min(y+1, H-1), min(x+1, W-1)] * _tmp2[y, min(x+1, W-1)])
                  if c == 1 or (out[max(y-1, 0), x] != tmp[max(y-1, 0), x]):
                      judge += 1
  
                  c = 0
                  c += (_tmp2[y, min(x+1, W-1)] - _tmp2[y, min(x+1, W-1)] * (1 - tmp[max(y-1, 0), min(x+1, W-1)]) * _tmp2[max(y-1, 0), x])
                  c += (_tmp2[max(y-1, 0), x] - _tmp2[max(y-1, 0), x] * _tmp2[max(y-1, 0), max(x-1, 0)] * _tmp2[y, max(x-1, 0)])
                  c += (_tmp2[y, max(x-1, 0)] - _tmp2[y, max(x-1, 0)] * _tmp2[min(y+1, H-1), max(x-1, 0)] * _tmp2[min(y+1, H-1), x])
                  c += (_tmp2[min(y+1, H-1), x] - _tmp2[min(y+1, H-1), x] * _tmp2[min(y+1, H-1), min(x+1, W-1)] * _tmp2[y, min(x+1, W-1)])
                  if c == 1 or (out[max(y-1, 0), min(x+1, W-1)] != tmp[max(y-1, 0), min(x+1, W-1)]):
                      judge += 1
  
                  c = 0
                  c += (_tmp2[y, min(x+1, W-1)] - _tmp2[y, min(x+1, W-1)] * _tmp2[max(y-1, 0), min(x+1, W-1)] * _tmp2[max(y-1, 0), x])
                  c += (_tmp2[max(y-1, 0), x] - _tmp2[max(y-1, 0), x] * _tmp2[max(y-1, 0), max(x-1, 0)] * (1 - tmp[y, max(x-1, 0)]))
                  c += ((1 - tmp[y, max(x-1, 0)]) - (1 - tmp[y, max(x-1, 0)]) * _tmp2[min(y+1, H-1), max(x-1, 0)] * _tmp2[min(y+1, H-1), x])
                  c += (_tmp2[min(y+1, H-1), x] - _tmp2[min(y+1, H-1), x] * _tmp2[min(y+1, H-1), min(x+1, W-1)] * _tmp2[y, min(x+1, W-1)])
                  if c == 1 or (out[y, max(x-1, 0)] != tmp[y, max(x-1, 0)]):
                      judge += 1
                  
                  if judge >= 8:
                      out[y, x] = 0
                      count += 1
                      
      out = out.astype(np.uint8) * 255
  
      return out
  
  
  # Read image
  img = cv2.imread("gazo.png").astype(np.float32)
  
  # hilditch thining
  out = hilditch(img)
  
  # Save result
  cv2.imwrite("out.png", out)
  cv2.imshow("result", out)
  cv2.waitKey(0)
  cv2.destroyAllWindows()

```
</details>

<details>
<summary>データ例2（コードの挙動確認）</summary>

```
Instruction:

  次に示すのはJavaScriptのコードです。何が出力されるでしょうか？
  
  class Chameleon {
    static colorChange(newColor) {
      this.newColor = newColor;
      return this.newColor;
    }
  
    constructor({ newColor = "green" } = {}) {
      this.newColor = newColor;
    }
  }
  
  const freddie = new Chameleon({ newColor: "purple" });
  freddie.colorChange("orange");
  
Output:

  答え：TypeError

  colorChange関数は静的です。静的メソッドは、それらが作成されたコンストラクタ上でのみ動作するように設計されており、どの子達にも受け継がれません。 freddieは子となりますので、この関数は受け継がれず、freddieインスタンスでは利用できません。
  その結果、TypeErrorが投げられます。

```
</details>

<details>
<summary>データ例3（コードのバグ修正）</summary>

```
# IndentationError

Instruction:

  次に示すpythonコードの誤りを修正しなさい。
  
  def : get_node_idx ( heap , node ) :
      for idx , item in enumerate ( heap ) :
          dist , n = item
          if n == node :
              return idx
  return - 1
  
Output:

  def get_node_idx ( heap , node ) :
      for idx , item in enumerate ( heap ) :
          dist , n = item
          if n == node :
              return idx
      return - 1

```
</details>

|source name|num record|liscence|url|
|:----|:----|:----|:----|
|データサイエンス100本ノック（構造化データ加工編）（Python解答）|100|[MIT](https://github.com/The-Japan-DataScientist-Society/100knocks-preprocess/blob/master/LICENSE)|https://github.com/The-Japan-DataScientist-Society/100knocks-preprocess|
|データサイエンス100本ノック（構造化データ加工編）（SQL解答）|100|[MIT](https://github.com/rootassist/100knocks-preprocess-inSQLandPython-withColab/blob/master/LICENSE)|https://github.com/rootassist/100knocks-preprocess-inSQLandPython-withColab|
|画像処理100本ノック|100|[MIT](https://github.com/ryoppippi/Gasyori100knock/blob/master/LICENSE)|https://github.com/ryoppippi/Gasyori100knock|
|言語処理100本ノック2020|100|[MIT](https://github.com/nlp100/nlp100.github.io/blob/develop/LICENSE)<br>[MIT](https://github.com/upura/nlp100v2020/blob/master/LICENSE)|(問題) https://github.com/nlp100/nlp100.github.io<br>(解答) https://github.com/upura/nlp100v2020|
|Python初学者のためのpandas100本ノック※|100|AmenokakuCode Liscence|https://qiita.com/kunishou/items/bd5fad9a334f4f5be51c|
|Python初学者のためのPolars100本ノック※|100|AmenokakuCode Liscence|https://qiita.com/kunishou/items/1386d14a136f585e504e|
|100 Numpy Execieses|100|[MIT](https://github.com/rougier/numpy-100/blob/master/LICENSE.txt)|https://github.com/rougier/numpy-100|
|100 Julia Exercises|100|The Unliscence|https://github.com/RoyiAvital/Julia100Exercises|
|自作Python100本ノック|100|AmenokakuCode Liscence|https://qiita.com/ahpjop/items/373f807d68044cda1c9b|
|Python-for-Beginners-Solve-50-Exercises-Live|50|[MIT](https://github.com/garg10may/Python-for-Beginners-Solve-50-Exercises-Live/blob/master/LICENSE)|https://github.com/garg10may/Python-for-Beginners-Solve-50-Exercises-Live|
|R初学者のためのtidyverse100本ノック|100|AmenokakuCode Liscence|https://qiita.com/nekobo/items/cbf32a13637273f229da|
|JavaScripu Questions|155|[MIT](https://github.com/lydiahallie/javascript-questions/blob/master/LICENSE)|https://github.com/lydiahallie/javascript-questions|
|Break-It-Fix-It|4,000|[MIT](https://github.com/michiyasunaga/BIFI/blob/main/LICENSE)|https://github.com/michiyasunaga/BIFI|

※ 私が過去に作成した学習コンテンツです。

## ライセンス
個々のデータのライセンスは収集元のライセンスに従うため、データセット全体では混合ライセンスになります。
また、データ自体にライセンスが明記されておらず個別に権利者に言語モデル学習用途でデータセットへの掲載許諾を取ったデータに関しては [AmenokakuCode Liscence](https://github.com/kunishou/amenokaku-code-instruct/blob/main/AmenokakuCode%20Liscence) というライセンスを付与しています。このライセンスは、言語モデルでの学習用途に限り自由にデータを利用することを許可するものになります（そのため、データ自体を販売したり、配布することは認めていません）。

## データセットの更新
データセットについては、商用利用可能なプログラミング学習コンテンツを見つけたら今後随時追加していきたいと思います。  
**もし、有益なコンテンツを見つけたり、自身で作成した学習コンテンツを提供しても良いという方がおりましたら是非ご連絡下さい。**

## データセット名
Amenokaku は古事記に登場する[天迦久神](http://kojiki.kokugakuin.ac.jp/shinmei/amenokakunokami/)（あめのかくのかみ）という鹿の神様の名前を参考にしました。
