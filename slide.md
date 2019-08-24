### プログラミングLT 2019 Summer
- - -

## 日常生活で全く役に立たない
## 時間の小話

---

#### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)   
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* Rails 歴 1年半             <!-- .element: class="fragment" -->
* 趣味で Ruby にパッチを投げたりとか             <!-- .element: class="fragment" -->
* エディタは Vim             <!-- .element: class="fragment" -->

---

# 今日話すこと

---

## 日常生活で全く役に立たない
## 時間の小話

---

#### Ruby の Time クラス
- - -

```ruby
Time.now                      # => 2019-08-22 09:38:51 +0900
Time.new(2019, 8, 24)         # => 2019-08-24 00:00:00 +0900
Time.new(2019, 8, 24).zone    # => "JST"
Time.parse("2019/08/24")      # => 2019-08-24 00:00:00 +0900
# Error: ArgumentError (mon out of range)
Time.parse("2019/13/24")
```

* Time.now で現在の時刻を取得
* Time.new で任意の時刻を定義
* Time#zone でタイムゾーンを取得
* Time.parse で文字列から時刻情報をパース
* パースに失敗した場合は例外が発生

---

# 問題

---

## Time.parse("2019/-12/01")
## するとどうなる？

---

# やってみよう！

---

```ruby
Time.parse("2019/-12/01")
# => -0012-01-01 00:00:00 +0918
```

---

# 🤔

---

### `-0012-01-01 00:00:00 +0918`
### とは

---

#### `Time.parse` はどのように処理されるのか
#### みてみよう

---

#### Time.parse の処理
- - -

* パースは `Date._parse` で行われている
* その結果から Time オブジェクトを生成

```ruby
# 文字列から時刻をパースして Hash で返す
Date._parse("2019/08/24")    # => {:year=>2019, :mon=>8, :mday=>24}

# さっきの文字列をパースするとこうなる
# -12年とパースされていた…
Date._parse("2019/-12/01")   # => {:year=>-12, :mon=>1}

# 結果的にこういう風に解釈される
Time.new(-12, 1)             # => -0012-01-01 00:00:00 +0918
```

### -12年 としてパースされている！！             <!-- .element: class="fragment" -->

---

## じゃあ
## `+0918` はどこからでてきた 🤔

---

## ぐぐってみた

---

#### ざっくりまとめ
- - -

* UNIX では tz database という情報を元にタイムゾーンを参照している
* 現在使用されている日本標準時は 1888/1/1 から反映されている
* それよりも前は "LMT" というタイムゾーンが設定される
  * これが `+09:18` の正体

```ruby
Time.parse("2019/-12/01").zone               # => "LMT"
Time.parse("2019/-12/01").strftime("%::z")   # => "+09:18:59"
Time.new(1888, 1, 1)        # => 1888-01-01 00:00:00 +0900
Time.new(1887, 12, 31) # => 1887-12-31 00:00:00 +0918
```

---

## ただし
## Ubuntu だとな！！！             <!-- .element: class="fragment" -->

---


## LMT が設定されるかは環境依存
## Ubuntu だとLMTが設定されたが
## Mac だと設定されなかった

---

### まとめ

---

### まとめ
- - -

* 最初は Ruby のバグかと思ったけど調べていくと原因がわかってきて便利            <!-- .element: class="fragment" -->
* -12 年にパースされる原因を調べようと Ruby の実装を調べてみたら正規表現まみれでそっ閉じした            <!-- .element: class="fragment" -->
* わからないことがあればぐぐろう            <!-- .element: class="fragment" -->
* 時刻周りは闇            <!-- .element: class="fragment" -->

---

#### 参照
- - -

* [18分59秒をめぐって日本標準時の歴史をひもとくことに - エムスリーテックブログ](https://www.m3tech.blog/entry/timezone-091859)
* [タイムゾーンのオフセット+09:18:59 はて、18:59はなんぞや? &#8211; Abacus Technologies Blog &#8211; kana.me 要](https://kana.me/entry/timezone-1888-01-01-offset-09-18-59)
* [1901/12/13 20:45:51以前のAsia/Tokyoの結果は環境によって違うことがあるのでようちゅうい - トミールの技術系日記](https://tomi-ru.hatenablog.com/entry/2018/07/13/1901/12/13_20%3A45%3A51%E4%BB%A5%E5%89%8D%E3%81%AEAsia/Tokyo%E3%81%AE%E7%B5%90%E6%9E%9C%E3%81%AF%E7%92%B0%E5%A2%83%E3%81%AB%E3%82%88%E3%81%A3%E3%81%A6%E9%81%95%E3%81%86%E3%81%93%E3%81%A8%E3%81%8C)

---

# 宣伝

---

### Ruby Hack Challenge Holiday
- - -

* [Ruby Hack Challenge Holiday](https://rhc.connpass.com/event/140200/) というイベントをやっています
* Ruby 本体を自分でビルドしたりハックしたりするイベント       <!-- .element: class="fragment" -->
* Ruby のコミッタの方が主催しているので Ruby について聞けるチャンス       <!-- .element: class="fragment" -->
* Ruby 本体の開発に興味がある人はぜひ！！       <!-- .element: class="fragment" -->
* 次回は 09/01 開催予定       <!-- .element: class="fragment" -->


---


## ご清聴
## ありがとうございました
