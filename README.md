![banner](https://s3.amazonaws.com/f.cl.ly/items/2z1v1z1v0i1j0y192k30/WarriorJS%20Banner.png)

*ロゴを制作してくれた [guillecura](https://dribbble.com/guillecura) さんに心から感謝します。*

[![Travis](https://img.shields.io/travis/olistic/warriorjs.svg?style=flat-square)](https://travis-ci.org/olistic/warriorjs)
[![npm](https://img.shields.io/npm/v/warriorjs.svg?style=flat-square)](https://www.npmjs.com/package/warriorjs)
[![Gitter](https://img.shields.io/gitter/room/olistic/warriorjs.svg?style=flat-square)](https://gitter.im/olistic/warriorjs)

WarriorJS-ja は、インタラクティブな方法で JavaScript と人工知能を楽しく教えるために設計されたゲーム [WarriorJS](https://github.com/olistic/warriorjs) の日本語版です。

あなたは、巨塔の最上階で`＜ここにあなたのモチベーションとなる何かを挿入＞`するため、それを登る戦士としてプレーします。各階で敵と戦い、仲間を救出し、次の階につながる階段へ到達するためには、JavaScript (ES2015 フルサポート) を書いて戦士に指示を与える必要があります。

## インストール

```bash
$ npm install -g warriorjs-ja
```

> **インストール上の注意**

> `npm`コマンドを実行する前に Node.js をコンピュータにインストールする必要があります。[公式インストーラ](https://nodejs.org)からのインストールを推奨します。

## 使い方

```bash
$ warriorjs-ja
```

以上！現在の場所に warriorjs ディレクトリが作成され、その中のプロファイルディレクトリで Player クラスを含んだ Player.js ファイルを見つけることができます。

```javascript
class Player {
  playTurn(warrior) {
    // Cool code goes here
  }
}
```

> **CLI メモ**

> `$ warriorjs-ja --help`を実行することで、ゲームをカスタマイズするために使用できるオプションを表示できます。

## 目的

あなたの目的は、`playTurn`メソッドに命令を記入することで、戦士に何をすべきか指示を与えることです。各階で、戦士のアビリティは難易度とともに成長します。現在の階で戦士が利用できるアビリティの詳細については、プロファイルディレクトの README を参照してください。

下記は、敵に触れた場合は攻撃し、それ以外の場合は前進する指示を戦士に与える簡単な例です：

```javascript
class Player {
  playTurn(warrior) {
    if (warrior.feel().isEnemy()) {
      warrior.attack();
    } else {
      warrior.walk();
    }
  }
}
```

## 遊び方

Player.js の編集が完了したら、ファイルを保存し、その階をプレーするには再度`warriorjs-ja`コマンドを実行してください。一連のターンの再生が始まります。各ターンでは、プレイヤーと敵の`playTurn`メソッドが同時に実行されます。

階の途中でコードを変更することはできません。初めからその階で起こりえるすべてのことを考慮に入れ、戦士に適切な指示を与える必要があります。

体力 (health) を全て失うとその階は失敗になります。失敗による罰はありません。Player.js ファイルに戻り、コードを改善して再試行してください。

成功（階段に到達）すると、プロファイルの README は次の階のために更新されます。次の階をプレーするには、Player.js ファイルを変更し、再度`warriorjs-ja`を実行してください。

## 得点

あなたの目標は階段に到達することだけではなく、最高得点を獲得することでもあります。多くの方法でポイントを得るができます。

* 敵を倒すことで、その体力の最大値が得点に加算されます

* 仲間を救出することで、20ポイントを獲得できます

* ボーナスタイム内で成功することで、ボーナスタイムの残量を得点として得ることが出来ます

* すべての敵を倒し、すべての仲間を救出することで、得点の20%をボーナスとして獲得することができます

登ってきた階の合計得点は記録されます。階を通過するごとにその階の得点は合計得点に加算されます。

## 見方

このゲームはテキストベースではあるが、上空から俯瞰している2Dゲームと考えてください。すべての階は長方形であり、複数のマスによって構成されています。1マスには一つの要素しか存在することができません。あなたの目標は階段のあるマスを見つけることです。下記はフロアマップと要素の一例です：

```
╔════╗
║C s>║
║ S s║
║C @ ║
╚════╝

> = 階段
@ = 戦士 (20 HP)
s = スライム (12 HP)
S = 強スライム (24 HP)
C = 仲間 (1 HP)
```

## アビリティ

はじめは、戦士はわずかなアビリティしか持っていないが、階が上がるにつれてアビリティは成長します。大きく分けて二種類のアビリティがあります：[アクション](#アクション-行動) と [センス](#センス-感知)。

> **アビリティメモ**

> 多くのアビリティは、下記の向きで行うことができます：forward (前) 、backward (後) 、left (左) 、right (右) 。最初の引数として向きの文字列を渡す必要があります。例：`warrior.walk('backward');`。

### アクション (行動)

*アクション*は何かしらの影響をゲームに与えます。**ターンごとに実行できるアクションは一つのみです**ので、賢い選択をしてください。

下記はアクションの一覧です：

#### `warrior.walk([direction])`:

与えられた方向 (デフォルトで前方) に移動します。

#### `warrior.attack([direction])`:

与えられた方向 (デフォルトで前方) の要素を攻撃します。

#### `warrior.rest()`:

最大体力の10%を回復します。最大体力を超えることはありません。

#### `warrior.rescue([direction])`:

与えられた方向 (デフォルトで前方) の仲間を救出します (20ポイントを獲得) 。

#### `warrior.pivot([direction])`:

左、右、もしくは後方 (デフォルト) に向きを変えます。

#### `warrior.shoot([direction])`:

与えられた方向 (デフォルトで前方) に弓矢を放つ。

#### `warrior.bind([direction])`:

与えられた方向 (デフォルトで前方) の要素を縛って動けないようにする。

#### `warrior.detonate([direction])`:

与えられた方向 (デフォルトで前方) の爆弾を爆発させる。周囲4マス (自分自身も含む) にダメージを与えます。

#### `warrior.explode()`:

自分自身と周囲すべての要素を爆発させる。おそらく意図的にこれをしたくはないでしょう。

### センス (感知)

*センス*とは、現在の階の情報を収集するアビリティです。周囲の確認や適切なアクションの選択のため、必要なときにセンスを行うことができます。

下記はセンスの一覧です：

#### `warrior.feel([direction])`:

与えられた方向 (デフォルトで前方) の1[スペース](#スペース)を返します。

#### `warrior.health()`:

現在の体力の数値を返します。

#### `warrior.look([direction])`:

与えられた方向 (デフォルトで前方) の最大3[スペース](#スペース)の配列を返します。

#### `warrior.directionOfStairs()`:

現在の位置から見た階段のある方向を返します。

#### `warrior.directionOf(space)`:

引数として[スペース](#スペース)を渡すことで、そのスペースの方向が返されます。

#### `warrior.distanceOf(space)`:

引数として[スペース](#スペース)を渡すことで、そのスペースまでの距離を表す数値を返します。

#### `warrior.listen()`:

要素を持つすべてのスペースを配列で返します。

> **センスメモ**

> 感知した情報はターンごとに変化するので、次のターンで使用するため集めた情報は、記録しておく必要があります。例えば、体力が前のターンよりも減っているかどうかで、攻撃されているかどうかを確認することができます。

## スペース

*スペース*はマップのマスを表します。あるエリアに対してセンスを行った場合、通常一つまたは複数の (一つの配列で) スペースが返されます。

あるスペースに対してメソッドを呼び出すことで、そこに何があるかについての情報を収集することができます。下記は利用可能なメソッド一覧です：

#### `space.isEmpty()`:

`true`の場合、この場所には何もなく (階段を除く) 、ここまで歩けることを意味します。

#### `space.isStairs()`:

そこの場所に階段があるかどうかを確認します。

#### `space.isEnemy()`:

そこの場所に敵がいるかどうかを確認します。

#### `space.isCaptive()`:

そこの場所に捕まっている仲間がいるかどうかを確認します。

#### `space.isWall()`:

ここが階の端であれば`true`を返します。ここまで歩くことはできません。

#### `space.isTicking()`:

このスペースにやがて爆発する爆弾が含まれている場合`true`を返します。

> **スペースメモ**

> 多くの場合、センスの直後にこれらのメソッドを呼び出すことになります。例えば、`feel`センスは1`Space`を返します。これに対して`isCaptive()`を呼び出すことで、目の前に捕まっている仲間がいるかどうかを確認することができます。

> `warrior.feel().isCaptive();`

## エピックモード

一度塔の頂上に到達すると、エピックモードに突入します。再び`warriorjs-ja`を実行すると、止まることなく塔内のすべての階に対して現在の Player.js を実行します。

ほとんどの場合最初は失敗しますので、問題がある、あるいは得点を改善したい階に対して、`-l`オプションを使ってください。

```bash
$ warriorjs-ja -l 4
```

戦士が再び頂上に到達すると、すべての階を合わせた平均成績を受け取ることができます。成績の最高から最低は S、A、B、C、D、そして F です。最高得点を獲得して、すべての階で S をゲットしてみてください。

## ティップスとヒント

### 全体

* もしある階で行き詰まった場合は、README ドキュメンテーションを見直し、すべてのアビリティを試しているかどうかを確認してください。もし体力を維持できない場合は、周りに敵がいないときに (体力に注意しながら) 回復してください。また、可能なかぎり遠距離武器 (例えば弓など) を使用してください。

* JavaScript で作業していることを忘れないでください。ただ単に多くのコードで`playTurn`メソッドを埋めないでください。メソッドやクラスを使って整理しましょう。

* センスは気軽に使用できます。感知された情報を保存しておくことで、この先何のアクションを行うべきかについてのよりよい判断に役立ててください。

* プロファイルディレクトリにいる間は、`warriorjs-ja`を実行すると、プロファイルを自動的に選択します。そのため、実行のたびに選択する必要はありません。

* もし高得点を狙っているなら、忘れずにエリア全体を見渡してください。たとえ階段近くにいたとしても、 (体力があるならば) すべてのものを入手するまで中に入らないでください。遠距離センス (例えば look と listen) を使って、敵が残っているかどうかを確認してください。

### ES2015+

* 各階の最初でいくつかのコードを実行したい場合は、下記のように`Player`クラス内で [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) を定義してください：

```javascript
class Player {
  constructor() {
    // このコードは、階の最初に一度だけ実行されます
    this._health = 20;
  }

  playTurn(warrior) {
    // Cool code goes here
  }
}
```

* 上記の例の`_health`のように、プロパティのみを初期設定したい場合は、[クラスインスタンスフィールド](https://github.com/jeffmo/es-class-fields-and-static-properties#part-1-class-instance-fields)を利用することができます。

```javascript
class Player {
  _health = 20;

  playTurn(warrior) {
    // Cool code goes here
  }
}
```

* いくつかのセンス (look と listen など) はスペースの配列を返します。[配列プロトタイプメソッド](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype#Methods)の多くは役に立つかもしれません。下記は [Array.prototype.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) メソッドの一例です：

```javascript
isEnemyInSight(warrior) {
  const unit = warrior.look().find(space => !space.isEmpty());
  return unit && unit.isEnemy();
}
```

* 上記の例では、[アロー関数](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Functions/Arrow_functions)が使われています。必ず確認しておいてください！

## Credits

This game was originally developed by [@ryanb](https://github.com/ryanb) to teach the Ruby language. Special thanks to him for the original [ruby-warrior](https://github.com/ryanb/ruby-warrior).

## License

MIT
