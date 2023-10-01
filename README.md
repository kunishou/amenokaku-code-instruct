<img src="https://github.com/kunishou/amenokaku-code-instruct/blob/main/image/amenokaku.png" alt="amenokaku_code">

# AmenokakuCode-instruct

## 概要
- コードに特化した5.2KのInstructionデータセットです。
- データセットに含まれるデータは商用利用できるラインセンスが付与されたプログラミング学習コンテンツから収集、加工し作成しました（英語のコンテンツは日本語に自動翻訳し、翻訳の不自然な箇所を手動で修正）。
- また、ライセンスが明記されていない学習コンテンツについては権利者に個別に連絡を取り、本データセットへの掲載の許諾を得ております。

## データセット詳細
指示タスクの内訳としてはコード生成（code_generation）が1050レコード、コードの挙動確認（check_code_behavor）が150レコード、コードのバグ修正（code_fix）が4000レコードになります。
詳細な内訳は以下の通りになります。
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
