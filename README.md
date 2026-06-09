# CorvusSKK 設定

[CorvusSKK](https://github.com/nathancorvussolis/corvusskk)（Windows 用 SKK 風日本語入力メソッド）の個人設定リポジトリです。

- 🎨 **ダークテーマ** — One Dark 系の候補一覧・入力モード表示
- 📚 **15 の SKK 辞書** — 絵文字・異体字・人名・地名・駅名・郵便番号など（URL 指定で自動取得）＋ ローカル SKK サーバー（yaskkserv2）
- 📅 **日付・時刻入力** — ▽きょう → 令和8年6月2日(火)
- ⚡ **強力な補完** — Tab で全辞書から見出しを補完（前方／後方一致・最大 7 件）
- ⌨️ **ホームポジション重視** — 候補選択は A S D F J K L G H

## 目次

- [セットアップ](#セットアップ)
- [SKK の基本操作](#skk-の基本操作)
  - [補完候補の削除](#補完候補の削除)
- [設定内容](#設定内容)
  - [辞書](#辞書)
  - [日付・時刻入力](#日付時刻入力)
  - [キーバインド](#キーバインド)
  - [動作設定](#動作設定)
  - [配色](#配色)
  - [かな拡張入力](#かな拡張入力)
- [設定リファレンス（config.xml）](#設定リファレンスconfigxml)
- [ローカル SKK サーバー（yaskkserv2）](#ローカル-skk-サーバーyaskkserv2)
  - [導入手順](#導入手順)
  - [自動起動（常駐）](#自動起動常駐)
  - [コマンドラインリファレンス](#コマンドラインリファレンス)
- [設定の変更とリポジトリへの反映](#設定の変更とリポジトリへの反映)
- [ファイル配置](#ファイル配置)
- [トラブルシューティング](#トラブルシューティング)
- [関連リンク](#関連リンク)

## セットアップ

### 必要なもの

| ソフトウェア | 用途 | 必須 |
|---|---|---|
| [CorvusSKK](https://github.com/nathancorvussolis/corvusskk/releases) | SKK 風 IME 本体 | ✅ |
| [HackGen Console NF](https://github.com/yuru7/HackGen/releases) | 候補一覧・入力モード表示のフォント | ✅ |
| SKK サーバー（[yaskkserv2](https://github.com/wachikun/yaskkserv2) 推奨・[google-ime-skk](https://github.com/hitode909/google-ime-skk) など） | `127.0.0.1:1178` の辞書サーバー。変換を高速化したい場合に併用（[ローカル SKK サーバー](#ローカル-skk-サーバーyaskkserv2)） | 任意 |

### 手順

1. CorvusSKK をインストールし、Windows の「設定 → 時刻と言語 → 言語と地域」で日本語キーボードに CorvusSKK が追加されていることを確認する
2. HackGen Console NF フォントをインストールする
3. この `config.xml` を CorvusSKK の設定フォルダにコピーする

   ```powershell
   Copy-Item config.xml "$env:APPDATA\CorvusSKK\config.xml"
   ```

4. IME をオフ → オン（Ctrl+Space を 2 回）して設定を反映する
5. （任意）SKK サーバーを `127.0.0.1:1178` で起動する（→ [ローカル SKK サーバー（yaskkserv2）](#ローカル-skk-サーバーyaskkserv2)）。使わない場合は設定ダイアログの「辞書」タブで SKK 辞書サーバーを無効にする

> **初回起動時**: URL 指定の辞書が `%TMP%\CorvusSKK` に自動ダウンロードされます。ネットワーク環境によっては時間がかかります。

## SKK の基本操作

SKK は「変換開始位置を自分で指定する」方式の日本語入力です。変換したい語の先頭を**大文字**で打つのが基本で、
形態素解析による誤変換が起きないため、慣れると修正の手間なく高速に入力できます。

| やりたいこと | 操作 | 例 |
|---|---|---|
| ひらがなを入力 | そのままローマ字入力 | `nihongo` → にほんご |
| 漢字に変換 | 先頭を大文字で入力 → Space | `Kanji` Space → ▼漢字 |
| 送り仮名つきで変換 | 送り仮名の頭も大文字 | `OkuRu` → ▼送る |
| 次 / 前の候補 | Space / Ctrl+P | |
| 候補一覧から選択 | A S D F J K L G H | 変換 2 回目から一覧表示 |
| 確定 | Enter（または次の入力を続ける） | |
| 変換を取消 | Esc / Ctrl+G | |
| カタカナモードに切替 | `q` | もう一度 `q` でひらがなに戻る |
| ▽の読みをカタカナに変換 | ▽モードで `q` | ▽とうきょう `q` → トウキョウ |
| 英単語から変換（abbrev） | `/` のあとに英字 → Space | `/file` Space → ▼ファイル |
| ASCII モード | `l`（小文字エル） | 戻るには Ctrl+Space を 2 回 |
| 全角英数モード | `L`（Shift+L） | |
| 半角カナに変換 | ▽モードで Ctrl+Q | |
| 接頭辞・接尾辞の変換 | `>` / `<` | ▽ちょう`>` → 超 ・ `>`てき → 〜的 |
| 数値変換 | 数字を含む読みを変換 | `Dai5` Space → 第5 ・ 第五 など |
| 単語登録 | 候補が尽きると ▼[登録：よみ] に入る → 単語を入力して Enter | |
| 登録した単語の削除 | その候補を表示中に Ctrl+D | |
| プライベートモード（学習しない） | Ctrl+Shift+F10 で切替 | |

### 補完候補の削除

補完候補（▽モードで Tab を押すと出る読み）には、過去に変換・確定した読み＝**ユーザー辞書の見出し語**が含まれます。
ユーザー辞書由来の不要な読みが補完に出る場合は、次の手順でユーザー辞書から削除します（取込済 SKK 辞書由来の補完はこの方法では消えません）。

1. 消したい読みを変換する（`▽よみ` → Space で ▼候補 を表示）
2. 候補が表示されている状態で **Ctrl+D**（辞書削除キー）を押す
3. 削除確認が表示されるので確定する
4. その読みの**すべての候補**を同様に削除すると、見出し語ごとユーザー辞書から消え、補完に表示されなくなる

> **注意**: 補完の表示中には削除できません。必ず ▼変換の候補表示中に Ctrl+D を押します。
> 候補が多い場合は、[ファイル配置](#ファイル配置)の `userdict.txt` を直接編集する方が早いことがあります
> （`taskkill /im imcrvmgr.exe` で辞書管理プロセスを停止してから編集し、IME をオフ → オンで反映）。

## 設定内容

### 辞書

URL 指定（自動ダウンロード）の SKK 辞書と、ローカルの SKK 辞書サーバーを併用しています。

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
| SKK サーバー | `127.0.0.1:1178`（yaskkserv2 など。[ローカル SKK サーバー](#ローカル-skk-サーバーyaskkserv2)） |

### 日付・時刻入力

SKK-JISYO.lisp のプログラム実行変換（CorvusSKK 内蔵の Lua 拡張で動作）により、
以下の見出し語を日付・時刻に変換できます。

| 入力 | 変換例 |
|---|---|
| ▽きょう | 令和8年6月2日(火) ・ 2026年6月2日(火) など |
| ▽あした ・ ▽きのう ・ ▽あさって ・ ▽おととい | 前後の日付 |
| ▽ことし ・ ▽らいねん ・ ▽きょねん | 年 |
| ▽いま ・ ▽now | 現在時刻（15時30分 など） |
| ▽へいせい#ねん（数値変換） | 西暦 ⇔ 年号の変換 |

### キーバインド

#### モード切替

| キー | 機能 |
|---|---|
| Ctrl+Space | IME オン / オフ |
| Q / Ctrl+I | かな ⇔ カナ 切替 |
| L | ASCII モード |
| Shift+L | 全英モード |
| / | abbrev モード（英字見出し変換） |
| Ctrl+Q | 半角カナ変換 |
| Ctrl+Shift+F10 | プライベートモード オン / オフ |

#### 変換・編集

| キー | 機能 |
|---|---|
| Space / Ctrl+N | 変換開始・次候補 |
| Ctrl+P | 前候補 |
| Tab / Shift+Tab | 次補完 / 前補完 |
| Ctrl+F | 補完と変換 |
| Ctrl+D | ユーザー辞書から候補を削除 |
| Enter | 確定 |
| Esc / Ctrl+G | 取消 |
| Backspace / Delete | 後退 / 削除 |
| ← / → | カーソル移動 |
| Home / End | 先頭 / 末尾へ移動 |
| Ctrl+V | 貼り付け |
| < / > | 接辞変換 |

#### 候補選択

候補一覧はホームポジションのキーで選択します（縦表示・7 件 / ページ）。

```
A S D F J K L G H
```

### 動作設定

- **補完**: Tab 補完で全辞書（ユーザー辞書＋取込済 SKK 辞書）から複数候補をリスト表示（前方／後方一致・最大 7 件）。入力ごとの動的補完はオフ（負荷回避のため Tab 補完中心）
- **カタカナ候補の自動追加**: 変換候補にカタカナ変換を追加
- **送り仮名**: 一致した候補を優先、送りあり変換で送りなし候補も検索
- **候補一覧**: 2 回目の変換から表示、注釈付き
- **数字キー (0–9)**: かなモードでも直接入力
- **ユーザー辞書**: `%APPDATA%\CorvusSKK` に 7 世代バックアップ

### 配色

候補一覧・入力モード表示は One Dark 系のダークテーマです。

| 項目 | 設定キー | 値 (COLORREF) | 画面上の色 |
|---|---|---|---|
| 背景 | colorbg / colormc | 0x1E1E1E | #1E1E1E |
| 文字 | colorca / colormf | 0xD4D4D4 | #D4D4D4 |
| 枠 | colorfr | 0x6E6E6E | #6E6E6E |
| 選択中の候補 | colorse | 0xD69C56 | #569CD6 |
| 注釈・番号・選択キー・コメント | coloran / colorno / colorsc / colorco | 0x9D9D9D | #9D9D9D |
| ひらがなモード | colorhr | 0x756CF0 | #F06C75 |
| カタカナモード | colorkt | 0x79C398 | #98C379 |
| 半角カナモード | colorka | 0xDD78C6 | #C678DD |
| 全英モード | colorjl | 0xEFAF61 | #61AFEF |
| ASCII モード | colorac | 0xC2B656 | #56B6C2 |
| 直接入力 | colordr | 0x9D9D9D | #9D9D9D |

> 設定値は COLORREF 形式（0xBBGGRR）のため、画面上の色（RGB）とはバイト順が逆です。
> アプリ内の変換中文字列の表示属性（displayattr）はアプリ側の配色に依存するため変更していません。

### かな拡張入力

`z` で始まる組み合わせで記号類を直接入力できます。

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

## 設定リファレンス（config.xml）

`config.xml` の各セクション・キーの意味と、このリポジトリでの設定値です。説明文は CorvusSKK 設定ダイアログのラベルに対応します（チェック系は `1`=有効 / `0`=無効）。

### server — SKK 辞書サーバー

| キー | 値 | 意味 |
|---|---|---|
| serv | 1 | SKK サーバーを使用する |
| host | 127.0.0.1 | サーバーのホスト |
| port | 1178 | サーバーのポート |
| encoding | 0 | サーバーとの文字コード（0=EUC-JP / 1=UTF-8） |
| timeout | 1500 | 応答待ちの上限（ms）。詳細は[ローカル SKK サーバー](#ローカル-skk-サーバーyaskkserv2) |

### userdict — ユーザー辞書

| キー | 値 | 意味 |
|---|---|---|
| backupdir | `%APPDATA%\CorvusSKK` | ユーザー辞書のバックアップ先 |
| backupgen | 7 | バックアップの世代数 |
| privateonvkey / privateonmkey | 0x79 / 6 | プライベートモード ON キー（0x79=F10・6=Ctrl+Shift → **Ctrl+Shift+F10**） |
| privateoffvkey / privateoffmkey | 0x79 / 6 | プライベートモード OFF キー（同上・トグル） |
| privatemodeauto | 1 | アプリ切替時にプライベートモードを自動解除する |

> `mkey` は修飾キーのビットマスク（**Ctrl=2 / Shift=4**、合算で Ctrl+Shift=6）。`vkey` は仮想キーコード（F10=0x79）。

### behavior — 動作

| キー | 値 | 意味（設定ダイアログのラベル） |
|---|---|---|
| defaultmode | 0 | 初期入力モード（0=ひらがな） |
| defmodeascii | 0 | 直接入力モードを ASCII にする（0=無効） |
| begincvokuri | 1 | 送り仮名が決定したとき変換を開始する |
| precedeokuri | 1 | 送り仮名が一致した候補を優先する |
| shiftnnokuri | 0 | 送り仮名で撥音（ん）を送り出す |
| srchallokuri | 1 | 送りあり変換で送りなし候補も検索する |
| delcvposcncl | 0 | 取消のとき変換位置を削除する |
| delokuricncl | 1 | 取消のとき送り仮名を削除する |
| backincenter | 1 | 後退（Backspace）に確定を含める |
| addcandktkn | 1 | 候補に片仮名変換を追加する |
| entogglekana | 0 | かな／カナ入力モードをトグルする |
| setbydirect | 0 | キー設定「直接入力」を確定入力で使用する |
| compmultinum | 7 | 複数補完の候補数 |
| stacompmulti | 1 | 複数補完を使用する（Tab 補完で複数列挙） |
| dynamiccomp | 0 | 動的補完を使用する（▽入力中に自動表示）。この設定ではオフ（全辞書補完による per-keystroke 負荷を避け Tab 補完中心に） |
| dyncompmulti | 1 | 複数動的補完を使用する（`dynamiccomp=0` のため現状は無効） |
| compuserdic | 1 | 補完された見出し語の候補を表示する |
| compincback | 1 | 前方一致と後方一致で補完する |
| compwithall | 1 | 全ての辞書ファイルで補完する（ユーザー辞書＋取込済 SKK 辞書） |

### font — フォント

| キー | 値 | 意味 |
|---|---|---|
| name | HackGen Console NF | 候補一覧・入力モード表示のフォント |
| size | 12 | フォントサイズ |
| weight | 400 | 太さ（400=標準 / 700=太字） |
| italic | 0 | 斜体（0=無効） |

### display — 表示

色のキー（`colorbg` ほか）は[配色](#配色)を参照。その他の表示キーは次の通りです。

| キー | 値 | 意味 |
|---|---|---|
| maxwidth | 320 | 候補一覧の最大幅（px） |
| drawapi | 1 | 描画 API（1=Direct2D） |
| colorfont | 1 | 彩色（カラーフォント・絵文字を色付き表示） |
| untilcandlist | 2 | 候補一覧表示に要する変換回数（0=表示なし。2=2 回目から一覧） |
| pagecandnum | 7 | 候補一覧のページ当たり候補数 |
| dispcandno | 0 | 候補一覧が表示なしのとき候補数を表示する |
| verticalcand | 1 | 候補一覧を縦に表示する |
| annotation | 1 | 注釈を表示する |
| annotatlst | 1 | 注釈を候補一覧にも表示する |
| showmodemark | 1 | ▽▼* マークを表示する |
| showroman | 1 | 入力中のローマ字を表示する |
| showromanjlat | 0 | 全英モードでローマ字を表示する（0=無効） |
| showmodeinl | 1 | 入力モードをインライン表示する |
| showmodeinltm | 3000 | インライン入力モード表示の時間（ms） |

### その他のセクション

| セクション | 内容 |
|---|---|
| dictionary | 使用する SKK 辞書の一覧（[辞書](#辞書)を参照） |
| displayattr | 変換中文字列の表示属性（下線・色）をアプリ種別ごとに指定。値は内部形式のため通常は変更しない |
| selkey | 候補一覧の選択キー（1〜9 に割当 → 実質 A S D F J K L G H） |
| preservedkeyon / preservedkeyoff | IME オン／オフのシステム予約キー（`vkey=0x20`(Space)・`mkey=2`(Ctrl) → **Ctrl+Space**） |
| keymap / vkeymap | 機能ごとのキー割り当て。`keymap`=文字ベース、`vkeymap`=仮想キーベース。空欄は CorvusSKK 既定を使用（実際の割り当ては[キーバインド](#キーバインド)） |
| convpoint | 変換開始位置（送り・接辞）に使う文字の対応表 |
| kana | ローマ字 → かな（ひらがな／カタカナ／半角カナ）変換テーブル（[かな拡張入力](#かな拡張入力)の `z` 系を含む） |
| jlatin | 半角 → 全角英数（全英モード）の対応表 |

## ローカル SKK サーバー（yaskkserv2）

> このセクションは **任意**です。SKK サーバーを使わない場合は読み飛ばして構いません。

変換を高速化したい場合は、[yaskkserv2](https://github.com/wachikun/yaskkserv2) を `127.0.0.1:1178` で常駐させます。yaskkserv2 は辞書を事前にバイナリ化してローカルで引くため応答がサブミリ秒です。本リポジトリでは、**ローカル辞書で見つからない語のみ Google 日本語入力にフォールバックし、その結果をキャッシュする**構成を前提に設定しています。

> **この `config.xml` の設定**: `serv=1` / `host=127.0.0.1` / `port=1178` / `encoding=0`（EUC-JP）/ `timeout=1500`（ms）で設定済みです。
>
> `timeout=1500` は **Google フォールバックの応答を取りこぼさない**ための値です。yaskkserv2 は未登録語を Google に問い合わせる際、内部で最大 `--google-timeout-milliseconds`（既定 1000ms）待ってから応答します。CorvusSKK 側のタイムアウトがこれより短いと Google 候補が返る前に打ち切られてしまうため、1000ms ＋ 通信・処理の余裕を見て 1500ms にしています。
>
> ローカル辞書でヒットする語はサブミリ秒で応答するため、**通常の入力が 1500ms 待たされることはありません**。待ちが伸びるのは Google 問い合わせが発生する未登録語のみで、一度引けばキャッシュされ次回以降は高速です。Google フォールバックを使わない（純オフライン）場合は、`timeout` を `100`〜`300` 程度に下げても構いません。

### 導入手順

#### 1. 入手

> **注意**: yaskkserv2 は **crates.io に公開されていません**。そのため `cargo install yaskkserv2` や `cargo binstall yaskkserv2` は `yaskkserv2 is not found` で失敗します。また**リリースのビルド済みバイナリは Linux / macOS 用のみ**で Windows 版はありません。Windows では **Rust ツールチェーンで自前ビルド**するのが基本です。

事前に [rustup](https://rustup.rs/) で Rust（`cargo`）を導入しておきます（Windows では Visual Studio Build Tools / MSVC が必要です）。

**方法 A: `cargo install --git`（推奨・一行）**

GitHub リポジトリから直接ビルドして導入します。単一パッケージのため、`yaskkserv2` と `yaskkserv2_make_dictionary` の両方が `%USERPROFILE%\.cargo\bin`（既定で PATH 済み）に入ります。

```powershell
cargo install --git https://github.com/wachikun/yaskkserv2
```

導入後、`yaskkserv2 --version` で確認できます。

**方法 B: クローンしてビルド（公式手順）**

```powershell
git clone https://github.com/wachikun/yaskkserv2
cd yaskkserv2
cargo build --release
```

ビルド済みバイナリは `target\release\yaskkserv2.exe` と `target\release\yaskkserv2_make_dictionary.exe` に生成されます。任意のフォルダ（例: `C:\tools\yaskkserv2\`）へコピーして使います。

> **Windows でビルドが通らない場合**: yaskkserv2 は主に Linux / macOS 向けに開発されています。MSVC で素直にビルドできないときは、**WSL2** 上で `cargo build --release` してサーバーを起動する方法もあります（WSL2 でリッスンしたポートは Windows 側から `localhost:1178` で到達できます）。

#### 2. 辞書の用意とビルド

辞書ソースは、`config.xml` で自動ダウンロード済みの辞書（`%TMP%\CorvusSKK`）か、[skk-dev/dict](https://github.com/skk-dev/dict) から `SKK-JISYO.L` などを取得します（`.gz` は展開しておく）。

`dictionary.yaskkserv2` の置き場所は任意で、**起動時に `yaskkserv2` へ渡すパスと一致していれば**どこでも動きます。`--dictionary-filename` に**絶対パス**を指定して、固定フォルダへ直接出力するのが確実です。入手方法によって自然な置き場所が変わります。

**方法 A（`cargo install --git`）の場合** — バイナリは PATH 済みなので、辞書はユーザー専用フォルダ `%LOCALAPPDATA%\yaskkserv2\` に置きます（`.cargo\bin` はバイナリ置き場なので辞書は入れない）。

```powershell
New-Item -ItemType Directory -Force -Path "$env:LOCALAPPDATA\yaskkserv2" | Out-Null

yaskkserv2_make_dictionary --dictionary-filename "$env:LOCALAPPDATA\yaskkserv2\dictionary.yaskkserv2" `
  SKK-JISYO.L SKK-JISYO.jinmei SKK-JISYO.fullname SKK-JISYO.geo `
  SKK-JISYO.station SKK-JISYO.propernoun SKK-JISYO.assoc
```

**方法 B（クローンしてビルド）の場合** — `yaskkserv2.exe` を置いたフォルダ（例 `C:\tools\yaskkserv2\`）に辞書も一緒に置きます。

```powershell
New-Item -ItemType Directory -Force -Path C:\tools\yaskkserv2 | Out-Null

yaskkserv2_make_dictionary --dictionary-filename C:\tools\yaskkserv2\dictionary.yaskkserv2 `
  SKK-JISYO.L SKK-JISYO.jinmei SKK-JISYO.fullname SKK-JISYO.geo `
  SKK-JISYO.station SKK-JISYO.propernoun SKK-JISYO.assoc
```

辞書は既定で EUC-JP です（UTF-8 で運用する場合は[コマンドラインリファレンス](#コマンドラインリファレンス)のエンコーディングの注記を参照）。

> **避けるべき置き場所**: `%TMP%` / `Downloads` などの一時的な場所、クローンした `target\release\` 内（再ビルドや `cargo clean` で消える）。
>
> 以降の手順は方法 B の `C:\tools\yaskkserv2\dictionary.yaskkserv2` を例に記載します。方法 A の場合は `%LOCALAPPDATA%\yaskkserv2\dictionary.yaskkserv2` に読み替えてください（自動起動の `schtasks` では `%LOCALAPPDATA%` を展開した実パス、例 `C:\Users\<ユーザー名>\AppData\Local\yaskkserv2\dictionary.yaskkserv2` を使う）。

#### 3. サーバーの起動

`127.0.0.1:1178` で待ち受けます。本リポジトリの推奨構成（**未登録語のみ Google 日本語入力にフォールバックし、結果をキャッシュ**）では次のように起動します。

```powershell
yaskkserv2 --listen-address 127.0.0.1 --port 1178 `
  --google-japanese-input notfound `
  --google-cache-filename C:\tools\yaskkserv2\google-cache.json `
  C:\tools\yaskkserv2\dictionary.yaskkserv2
```

- `--google-japanese-input notfound`: ローカル辞書で見つからない語のときだけ Google に問い合わせ（ローカルの速さを保ちつつ語彙を補う）
- `--google-cache-filename ...`: Google の変換結果をファイルにキャッシュし、次回以降は再問い合わせせず高速化

> Google フォールバックを使わず**完全オフライン**で運用する場合は、`--google-*` を外して `yaskkserv2 --listen-address 127.0.0.1 --port 1178 C:\tools\yaskkserv2\dictionary.yaskkserv2` だけで起動し、`config.xml` の `timeout` も `100`〜`300` に下げて構いません。各オプションの詳細は[コマンドラインリファレンス](#コマンドラインリファレンス)を参照。

#### 4. 動作確認

```powershell
Test-NetConnection 127.0.0.1 -Port 1178
```

接続できたら CorvusSKK で `▽` 変換し、ローカル辞書の語が即座に表示されることを確認します。辞書に無い語を変換すると Google フォールバックが働き（初回はネット往復のぶん待ちますが、`google-cache.json` にキャッシュされ次回以降は高速）、サーバーを停止している間は `timeout=1500` 後にローカル辞書のみで変換されます。

### 自動起動（常駐）

サインインのたびに yaskkserv2 を自動起動するには、**タスクスケジューラ**で登録するのが確実です（コンソールウィンドウを出さずにバックグラウンド常駐できます）。以下では `C:\tools\yaskkserv2\` に `yaskkserv2.exe` と `dictionary.yaskkserv2` を置いた前提で、推奨構成（Google フォールバック＋キャッシュ）の起動引数で説明します（パスや要否は環境に合わせて読み替えてください）。

#### 方法 A: `schtasks` コマンドで一括登録（推奨）

管理者権限の PowerShell で以下を実行します。`/RL HIGHEST`（最上位の特権）でログオン時に起動します。

```powershell
schtasks /Create /TN "yaskkserv2" /SC ONLOGON /RL HIGHEST /F `
  /TR "C:\tools\yaskkserv2\yaskkserv2.exe --listen-address 127.0.0.1 --port 1178 --google-japanese-input notfound --google-cache-filename C:\tools\yaskkserv2\google-cache.json C:\tools\yaskkserv2\dictionary.yaskkserv2"
```

- 登録の確認: `schtasks /Query /TN "yaskkserv2"`
- すぐに起動して動作確認: `schtasks /Run /TN "yaskkserv2"`
- 解除（削除）: `schtasks /Delete /TN "yaskkserv2" /F`

> **コンソールが一瞬見える場合**: `yaskkserv2.exe` はコンソールアプリのため、`/TR` で直接起動するとログオン時に一瞬ウィンドウが表示されることがあります。完全に非表示にしたいときは、後述の VBScript ラッパーを `/TR` に指定してください。

#### 方法 B: タスクスケジューラ GUI で登録

1. `taskschd.msc` を起動 →「基本タスクの作成」
2. 名前: `yaskkserv2`
3. トリガー: **「ログオン時」**
4. 操作: **「プログラムの開始」**
   - プログラム/スクリプト: `C:\tools\yaskkserv2\yaskkserv2.exe`
   - 引数の追加: `--listen-address 127.0.0.1 --port 1178 --google-japanese-input notfound --google-cache-filename C:\tools\yaskkserv2\google-cache.json C:\tools\yaskkserv2\dictionary.yaskkserv2`
5. 作成後、タスクのプロパティを開き
   - 「全般」タブ → **「最上位の特権で実行する」**にチェック
   - 「設定」タブ → **「タスクを停止するまでの時間」のチェックを外す**（常駐し続けるため）
   - 「条件」タブ → 「コンピューターを AC 電源で使用している場合のみ…」のチェックを外す（ノート PC でも常駐させる場合）

#### コンソールを完全に隠す（任意）

ウィンドウを一切出さずに常駐させたい場合は、VBScript のラッパー経由で起動します。`C:\tools\yaskkserv2\start-hidden.vbs` を作成:

```vbscript
' start-hidden.vbs — yaskkserv2 をウィンドウ非表示で起動
CreateObject("WScript.Shell").Run _
  """C:\tools\yaskkserv2\yaskkserv2.exe"" --listen-address 127.0.0.1 --port 1178 --google-japanese-input notfound --google-cache-filename ""C:\tools\yaskkserv2\google-cache.json"" ""C:\tools\yaskkserv2\dictionary.yaskkserv2""", 0, False
```

タスクの `/TR`（または GUI のプログラム）を次のように差し替えます:

```powershell
schtasks /Create /TN "yaskkserv2" /SC ONLOGON /RL HIGHEST /F `
  /TR "wscript.exe C:\tools\yaskkserv2\start-hidden.vbs"
```

#### 方法 C: スタートアップフォルダ（手軽だが非推奨）

`Win+R` → `shell:startup` で開くフォルダに上記 `start-hidden.vbs` のショートカットを置くだけでもログオン時に起動できます。ただし特権昇格やウィンドウ制御が効きにくいため、基本は方法 A / B を推奨します。

> **更新・再起動**: 辞書を作り直したときや設定を変えたときは、`schtasks /End /TN "yaskkserv2"`（または タスクマネージャーで `yaskkserv2.exe` を終了）してから `schtasks /Run /TN "yaskkserv2"` で再起動します。

### コマンドラインリファレンス

> オプションはバージョンで増減することがあります。最終的には実環境の `yaskkserv2 --help` /
> `yaskkserv2_make_dictionary --help` を正としてください。以下は `master` 系の値です。

#### サーバー本体 `yaskkserv2`

```
yaskkserv2 [オプション] <dictionary>
```

`<dictionary>` は `yaskkserv2_make_dictionary` で作った辞書ファイル（必須・位置引数）です。
設定はコマンドライン引数のほか、`--config-filename` で指定する設定ファイルでも与えられます。

| オプション | 既定値 | 説明 |
|---|---|---|
| `<dictionary>` | （必須） | 変換に使う辞書ファイルのパス |
| `--config-filename=<FILE>` | ビルド既定パス | 設定ファイルを指定（[etc/yaskkserv2.conf](https://github.com/wachikun/yaskkserv2/blob/master/etc/yaskkserv2.conf) が雛形） |
| `--port=<PORT>` | `1178` | 待ち受けポート。CorvusSKK の `config.xml` の `port` と一致させる |
| `--listen-address=<ADDR>` | `0.0.0.0` | 待ち受けアドレス。**ローカル専用なら `127.0.0.1` を推奨**（外部公開を避ける） |
| `--max-connections=<N>` | `16` | 同時接続数の上限 |
| `--max-server-completions=<N>` | `64` | サーバーが返す補完候補の最大数（Tab 補完で利用） |
| `--midashi-utf8` | （無効=EUC） | 見出し語（検索キー）を UTF-8 として受け取る。辞書を `--utf8` で作った場合に合わせる |
| `--no-daemonize` | — | デーモン化しない（フォアグラウンド実行）。Unix 向けの概念で、Windows では実質フォアグラウンド動作 |
| `--google-japanese-input=<TIMING>` | `notfound` | Google 日本語入力フォールバックのタイミング：`notfound`（辞書に無いときのみ）/ `disable`（使わない）/ `last`（辞書の後に必ず）/ `first`（辞書より先に） |
| `--google-suggest` | （`etc` 雛形では有効） | Google サジェスト API を併用 |
| `--google-use-http` | （無効=HTTPS） | Google へのアクセスを HTTP にする（既定は HTTPS） |
| `--google-timeout-milliseconds=<MS>` | `1000` | Google アクセスのタイムアウト（ms） |
| `--google-cache-filename=<FILE>` | （無効） | Google 変換結果のキャッシュファイル。指定すると再利用で高速化 |
| `--google-cache-entries=<N>` | `1024` | キャッシュ件数の上限 |
| `--google-cache-expire-seconds=<SEC>` | `2592000`（30日） | キャッシュの有効期間（秒） |
| `--google-max-candidates-length=<N>` | `25` | マージする候補の最大長 |
| `--hostname-and-ip-address-for-protocol-3=<HOST:ADDR>` | `localhost:127.0.0.1` | skkserv プロトコル 3（サーバーバージョン/ホスト応答）で返すホスト名と IP |

Google フォールバックも併用する起動例（未登録語だけネット変換・結果はキャッシュ）:

```powershell
yaskkserv2 --listen-address 127.0.0.1 --port 1178 `
  --google-japanese-input notfound `
  --google-cache-filename C:\tools\yaskkserv2\google-cache.json `
  C:\tools\yaskkserv2\dictionary.yaskkserv2
```

#### 辞書ビルド `yaskkserv2_make_dictionary`

```
yaskkserv2_make_dictionary --dictionary-filename <OUT> [--utf8] <SKK-JISYO...>
```

| オプション | 既定値 | 説明 |
|---|---|---|
| `<SKK-JISYO...>` | （複数可） | 入力する SKK 辞書（EUC または UTF-8）。位置引数で並べる |
| `--dictionary-filename=<FILE>` | （必須） | 出力するサーバー辞書のパス（`--cache-filename` と排他） |
| `--cache-filename=<FILE>` | （無効） | Google キャッシュファイルから辞書を作る（`--dictionary-filename` と排他） |
| `--output-jisyo-filename=<FILE>` | （無効） | サーバー辞書から SKK 辞書（テキスト）へ逆変換して出力 |
| `--utf8` | （無効=EUC） | UTF-8 形式の辞書を作る |
| `--verbose` | （無効） | 詳細ログを出力 |

> **エンコーディングは 3 点セットで揃える**: 「辞書ビルド `--utf8`」「サーバー `--midashi-utf8`」「CorvusSKK `encoding`（0=EUC-JP / 1=UTF-8）」は必ず一致させます。いずれか 1 つだけ変えると候補が文字化け／表示されません。この設定の既定はすべて **EUC-JP 側**（`--utf8` なし・`encoding=0`）です。

## 設定の変更とリポジトリへの反映

設定の変更方法は 2 つあります。

1. **設定ダイアログ**: タスクバーの入力モードアイコンを右クリック → 「設定」。変更は `%APPDATA%\CorvusSKK\config.xml` に保存される
2. **config.xml の直接編集**: 編集後、IME をオフ → オンで反映

変更をこのリポジトリに反映するには:

```powershell
Copy-Item "$env:APPDATA\CorvusSKK\config.xml" .\config.xml
git add config.xml
git commit -m "変更内容の説明"
```

## ファイル配置

| パス | 内容 |
|---|---|
| `%APPDATA%\CorvusSKK\config.xml` | 設定ファイル（このリポジトリで管理） |
| `%APPDATA%\CorvusSKK\userdict.txt` | ユーザー辞書（変換の学習結果） |
| `%APPDATA%\CorvusSKK\userdict.txt.*.bak` | ユーザー辞書のバックアップ（7 世代） |
| `%APPDATA%\CorvusSKK\init.lua` | Lua 拡張スクリプト（任意。なければ標準実装を使用） |
| `%TMP%\CorvusSKK\` | URL 辞書のダウンロードキャッシュ |

## トラブルシューティング

| 症状 | 対処 |
|---|---|
| 設定が反映されない | IME をオフ → オン。それでも反映されない場合はサインアウト → サインイン |
| 候補がほとんど出ない | 辞書のダウンロード失敗の可能性。ネットワークを確認し、`%TMP%\CorvusSKK` を削除して IME をオフ → オン |
| 変換が遅い・引っかかる | SKK サーバー（`127.0.0.1:1178`）が起動していない場合は、[ローカル SKK サーバー](#ローカル-skk-サーバーyaskkserv2)で常駐させるか、設定ダイアログ「辞書」タブでサーバーを無効化。サーバー停止中は毎変換で `timeout=1500`（ms）待つため、使わないなら無効化を推奨 |
| Google フォールバックの候補が出ない | CorvusSKK の `timeout`（1500ms）がサーバーの `--google-timeout-milliseconds`（既定 1000ms）より短いと打ち切られる。Google 側を変えたら `timeout` も見直す。完全オフライン運用なら `--google-*` を外し `timeout` を下げる |
| サーバーの候補が文字化けする / 出ない | エンコーディング不一致。[コマンドラインリファレンス](#コマンドラインリファレンス)の「3 点セットで揃える」を確認 |
| 絵文字が変換できない | Direct2D ＋ カラーフォントの表示設定が必要（この設定では有効済み） |
| 候補一覧のフォントがおかしい | HackGen Console NF がインストールされているか確認 |
| ASCII モードから戻れない | Ctrl+Space を 2 回押して IME を入れ直す（この設定では jmode キーを割り当てていないため） |
| 不要な読みが補完に表示される | [補完候補の削除](#補完候補の削除)の手順でユーザー辞書から削除 |

## 関連リンク

- [CorvusSKK](https://github.com/nathancorvussolis/corvusskk) — 本体・公式ドキュメント
- [CorvusSKK Releases](https://github.com/nathancorvussolis/corvusskk/releases) — インストーラー
- [skk-dev/dict](https://github.com/skk-dev/dict) — SKK 辞書（この設定の辞書取得元）
- [yaskkserv2](https://github.com/wachikun/yaskkserv2) — ローカル SKK サーバー
- [HackGen](https://github.com/yuru7/HackGen) — 候補一覧用フォント
- [ddskk](https://github.com/skk-dev/ddskk) — Emacs 版 SKK（SKK 操作体系の本家）
