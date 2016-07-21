![banner](https://s3.amazonaws.com/f.cl.ly/items/2z1v1z1v0i1j0y192k30/WarriorJS%20Banner.png)

*[guillecura](https://dribbble.com/guillecura) さんのロゴ制作に心から感謝します。*

[![Travis](https://img.shields.io/travis/olistic/warriorjs.svg?style=flat-square)](https://travis-ci.org/olistic/warriorjs)
[![npm](https://img.shields.io/npm/v/warriorjs.svg?style=flat-square)](https://www.npmjs.com/package/warriorjs)
[![Gitter](https://img.shields.io/gitter/room/olistic/warriorjs.svg?style=flat-square)](https://gitter.im/olistic/warriorjs)

これは JavaScript と人工知能を、インタラクティブで楽しい方法で教えるために設計されたゲームです。

あなたは巨塔の最上階で`＜ここにあなたのモチベーションとなる何かを挿入＞`するため、それを登る戦士としてプレーします。各階で敵と戦い、捕虜を救出し、階段に到達するためには、JavaScript (ES2015 フルサポート) を書いて戦士に指示を与える必要があります。

## インストール

```bash
$ npm install -g warriorjs-ja
```

> **インストール上の注意**

> `npm`コマンドを実行する前に Node.js をコンピュータにインストールする必要なあります。[公式インストーラ](https://nodejs.org)からのインストールを推奨します。

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

あなたの目的は、`playTurn`メソッドに命令を記入することで、戦士に何をすべきか指示を与えることです。各階で、戦士の能力は難易度とともに成長します。現在の階で戦士が利用できる能力の詳細については、プロファイルディレクトの README を参照してください。

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

階の途中でコードを変更することはできません。初めから、その階で起こりえるすべてのことを考慮に入れ、戦士に適切な指示を与える必要があります。

体力 (health) を全て失うとその階は失敗になります。失敗による罰はありません。Player.js ファイルに戻り、コードを改善して再試行してください。

成功（階段に到達）すると、プロファイルの README は次の階のために更新されます。次の階をプレーするには、Player.js ファイルを変更し、再度`warriorjs-ja`を実行してください。

## 得点

あなたの目標は階段に到達することだけではなく、最高得点を獲得することでもあります。多くの方法でポイントを得るができます。

* 敵を倒すことで、その体力の最大値が得点に加算されます

* 捕虜を救出することで、20ポイントを獲得できます

* ボーナスタイム内で成功することで、ボーナスタイムの残量を得点として得ることが出来ます

* すべての敵を倒し、すべての捕虜を救出することで、得点の20%をボーナスとして獲得することができます

登ってきた階の合計得点は保存されます。階を通過するごとにその階の得点は合計得点に加算されます。

## Perspective

Even though this is a text-based game, think of it as two-dimensional where you are viewing from overhead. Each level is always rectangular in shape and is made up of a number of squares. Only one unit can be on a given square at a time, and your objective is to find the square with the stairs. Here is an example level map and key:

```
╔════╗
║C s>║
║ S s║
║C @ ║
╚════╝

> = Stairs
@ = Warrior (20 HP)
s = Sludge (12 HP)
S = Thick Sludge (24 HP)
C = Captive (1 HP)
```

## Abilities

When you first start, your warrior will only have a few abilities, but with each level your abilities will grow. A warrior has two kinds of abilities: [Actions](#actions) and [Senses](#senses).

> **Note on Abilities**

> Many abilities can be performed in the following directions: forward, backward, left and right. You have to pass a string with the direction as the first argument, e.g. `warrior.walk('backward');`.

### Actions

An *action* is something that affects the game in some way. **Only one action can be performed per turn**, so choose wisely.

Here is the complete list of actions:

#### `warrior.walk([direction])`:

Move in given direction (forward by default).

#### `warrior.attack([direction])`:

Attack a unit in given direction (forward by default).

#### `warrior.rest()`:

Gain 10% of max health back, but do nothing more.

#### `warrior.rescue([direction])`:

Rescue a captive from his chains (earning 20 points) in given direction (forward by default).

#### `warrior.pivot([direction])`:

Rotate left, right or backward (default).

#### `warrior.shoot([direction])`:

Shoot your bow & arrow in given direction (forward by default).

#### `warrior.bind([direction])`:

Bind a unit in given direction to keep him from moving (forward by default).

#### `warrior.detonate([direction])`:

Detonate a bomb in a given direction (forward by default) which damages that space and surrounding 4 spaces (including yourself).

#### `warrior.explode()`:

Kills you and all surrounding units. You probably don't want to do this intentionally.

### Senses

A *sense* is something which gathers information about the floor. You can perform senses as often as you want per turn to gather information about your surroundings and to aid you in choosing the proper action.

Here is the complete list of senses:

#### `warrior.feel([direction])`:

Return a [Space](#spaces) for the given direction (forward by default).

#### `warrior.health()`:

Return an integer representing your current health.

#### `warrior.look([direction])`:

Return an array of up to three [Spaces](#spaces) in the given direction (forward by default).

#### `warrior.directionOfStairs()`:

Return the direction the stairs are from your location.

#### `warrior.directionOf(space)`:

Pass a [Space](#spaces) as an argument, and the direction to that space will be returned.

#### `warrior.distanceOf(space)`:

Pass a [Space](#spaces) as an argument, and it will return an integer representing the distance to that space.

#### `warrior.listen()`:

Return an array of all spaces which have units in them.

> **Note on Senses**

> Since what you sense will change each turn, you should record what information you gather for use on the next turn. For example, you can determine if you are being attacked if your health has gone down since the last turn.

## Spaces

A *space* is an object representing a square in the level. Whenever you sense an area, often one or multiple spaces (in an array) will be returned.

You can call methods on a space to gather information about what is there. Here are the various methods that are available to you:

#### `space.isEmpty()`:

If `true`, this means that nothing (except maybe stairs) is at this location and you can walk here.

#### `space.isStairs()`:

Determine if stairs are at that location.

#### `space.isEnemy()`:

Determine if an enemy unit is at this location.

#### `space.isCaptive()`:

Determine if a captive is at this location.

#### `space.isWall()`:

Return `true` if this is the edge of the level. You can't walk here.

#### `space.isTicking()`:

Return `true` if this space contains a bomb which will explode in time.

> **Note on Spaces**

> You will often call these methods directly after a sense. For example, the `feel` sense returns one `Space`. You can call `isCaptive()` on this to determine if a captive is in front of you:

> `warrior.feel().isCaptive();`

## Epic mode

Once you reach the top of the tower, you will enter Epic mode. When running `warriorjs` again, it will run your current Player.js through all levels in the tower without stopping.

Your warrior will most likely not succeed the first time around, so use the -l option on levels you are having difficulty or want to fine-tune the scoring.

```bash
$ warriorjs-ja -l 4
```

Once your warrior reaches the top again, you will receive an average grade, along with a grade for each level. The grades from best to worst are S, A, B, C, D and F. Try to get S on each level for the ultimate score!

## Tips & hints

### General

* If you ever get stuck on a level, review the README documentation and be sure you're trying each ability out. If you can't keep your health up, be sure to rest when no enemy is around (while keeping an eye on your health). Also, try to use far-ranged weapons whenever possible (such as the bow).

* Remember, you're working in JavaScript here. Don't simply fill up the `playTurn` method with a lot of code. Organize it with methods and classes.

* Senses are cheap, so use them liberally. Store the sensed information to help you better determine what actions to take in the future.

* Running `warriorjs` while you are in your profile directory will auto-select that profile so you don't have to each time.

* If you're aiming for points, remember to sweep the area. Even if you're close to the stairs, don't go in until you've gotten everything (if you have the health). Use far-ranged senses (such as look and listen) to determine if there are any enemies left.

### ES2015+

* If you want some code to be executed at the beginning of each level, define a [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) in the `Player` class, like this:

```javascript
class Player {
  constructor() {
    // This code will be executed only once, at the beginning of the level
    this._health = 20;
  }

  playTurn(warrior) {
    // Cool code goes here
  }
}
```

* If you just want to initialize a property, like `_health` in the example above, you can make use of [Class Instance Fields](https://github.com/jeffmo/es-class-fields-and-static-properties#part-1-class-instance-fields):

```javascript
class Player {
  _health = 20;

  playTurn(warrior) {
    // Cool code goes here
  }
}
```

* Some senses (like look and listen) return an array of spaces, so you might find many of the [Array prototype methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype#Methods) really useful. Here is an example of the [Array.prototype.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) method:

```javascript
isEnemyInSight(warrior) {
  const unit = warrior.look().find(space => !space.isEmpty());
  return unit && unit.isEnemy();
}
```

* In the example above, you can see the [Arrow functions](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in use. Make sure to take a look at them too!

## Credits

This game was originally developed by [@ryanb](https://github.com/ryanb) to teach the Ruby language. Special thanks to him for the original [ruby-warrior](https://github.com/ryanb/ruby-warrior).

## License

MIT
