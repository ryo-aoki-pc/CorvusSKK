# CorvusSKK 設定

[CorvusSKK](https://github.com/nathancorvussolis/corvusskk)（Windows 用 SKK 風日本語入力メソッド）の個人設定リポジトリです。

## ファイル

| ファイル | 内容 |
|---|---|
| `config.xml` | CorvusSKK の設定一式（辞書・キー設定・表示・かな変換テーブル） |

## 適用方法

1. [CorvusSKK](https://github.com/nathancorvussolis/corvusskk/releases) をインストールする
2. `config.xml` を `%APPDATA%\CorvusSKK\config.xml` にコピーする
3. IME をオフ → オンに切り替えると設定が反映される

> フォントに [HackGen Console NF](https://github.com/yuru7/HackGen) を使用しているため、未インストールの場合は別途インストールする。

## 辞書

URL 指定（自動ダウンロード）の SKK 辞書と、ローカルの SKK 辞書サーバーを併用。

| 辞書 | 内容 |
|---|---|
| SKK-JISYO.L | 標準大辞書 |
| SKK-JISYO.jinmei / fullname | 人名・フルネーム |
| SKK-JISYO.geo / station / propernoun | 地名・駅名・固有名詞 |
| SKK-JISYO.assoc | 連想変換 |
| SKK-JISYO.edict | 英和変換 |
| zipcode | 郵便番号 → 住所 |
| SKK-JISYO.JIS2 / JIS3_4 / JIS2004 | JIS 第 2〜第 4 水準の漢字 |
| SKK-JISYO.emoji | 絵文字（例: ▽えがお → 😊） |
| SKK-JISYO.itaiji | 異体字・旧字体（例: 萬・邊） |
| SKK-JISYO.lisp | 日付・時刻・年号・単位変換（プログラム実行変換） |
| SKK サーバー | localhost:1178 |

## 日付・時刻入力

SKK-JISYO.lisp のプログラム実行変換（CorvusSKK 内蔵の Lua 拡張で動作）により、
以下の見出し語を日付・時刻に変換できます。

| 入力 | 変換例 |
|---|---|
| ▽きょう | 令和8年6月2日(火) ・ 2026年6月2日(火) など |
| ▽あした ・ ▽きのう ・ ▽あさって ・ ▽おととい | 前後の日付 |
| ▽ことし ・ ▽らいねん ・ ▽きょねん | 年 |
| ▽いま ・ ▽now | 現在時刻（15時30分 など） |
| ▽へいせい#ねん（数値変換） | 西暦 ⇔ 年号の変換 |

## 主要キーバインド

### モード切替

| キー | 機能 |
|---|---|
| Ctrl+Space | IME オン / オフ |
| Q / Ctrl+I | かな ⇔ カナ 切替 |
| L | ASCII モード |
| Shift+L | 全英モード |
| / | abbrev モード（英字見出し変換） |
| Ctrl+Q | 半角カナ変換 |
| Ctrl+Shift+F10 | プライベートモード オン / オフ |

### 変換・編集

| キー | 機能 |
|---|---|
| Space / Ctrl+N | 変換開始・次候補 |
| Ctrl+P | 前候補 |
| Tab / Shift+Tab | 次補完 / 前補完 |
| Ctrl+F | 補完と変換 |
| Ctrl+D | ユーザー辞書から候補を削除 |
| Enter | 確定 |
| Esc / Ctrl+G | 取消 |
| Home / End | 先頭 / 末尾へ移動 |
| Ctrl+V | 貼り付け |
| < / > | 接辞変換 |

### 候補選択

候補一覧はホームポジションのキーで選択（縦表示・7 件 / ページ）。

```
A S D F J K L G H
```

## 動作設定の要点

- **補完**: 動的補完（▽モードで入力中に自動表示）・Tab 補完とも複数候補をリスト表示（最大 5 件）
- **カタカナ候補の自動追加**: 変換候補にカタカナ変換を追加
- **送り仮名**: 一致した候補を優先、送りあり変換で送りなし候補も検索
- **候補一覧**: 2 回目の変換から表示、注釈付き
- **数字キー (0–9)**: かなモードでも直接入力
- **配色**: ダークテーマ（One Dark 系。候補一覧・入力モード表示とも暗背景）

## かな拡張入力（z プレフィックス）

| 入力 | 出力 | 入力 | 出力 |
|---|---|---|---|
| `z ` | 　（全角空白） | `zh` | ← |
| `z-` | ～ | `zj` | ↓ |
| `z.` | … | `zk` | ↑ |
| `z,` | ‥ | `zl` | → |
| `z/` | ・ | `zL` | ⇒ |
| `z*` | ※ | `z[` `z]` | 『 』 |
| `z0` | ○ | `z{` `z}` | 【 】 |
| `z@` | ◎ | `z(` `z)` | （ ） |
| `z1`〜`z9` | ①〜⑨ | `z;` `z:` | ゛ ゜ |
