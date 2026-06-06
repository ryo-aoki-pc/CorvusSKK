# CorvusSKK 設定

[CorvusSKK](https://github.com/nathancorvussolis/corvusskk)（Windows 用 SKK 風日本語入力メソッド）の個人設定リポジトリです。

- 🎨 **ダークテーマ** — One Dark 系の候補一覧・入力モード表示
- 📚 **15 の SKK 辞書** — 絵文字・異体字・人名・地名・駅名・郵便番号など（URL 指定で自動取得）＋ ローカル SKK サーバー（yaskkserv2）
- 📅 **日付・時刻入力** — ▽きょう → 令和8年6月2日(火)
- ⚡ **動的補完** — ▽モードで入力中に候補を自動表示
- ⌨️ **ホームポジション重視** — 候補選択は A S D F J K L G H

## 目次

- [セットアップ](#セットアップ)
  - [ローカル SKK サーバー（yaskkserv2）の導入](#ローカル-skk-サーバーyaskkserv2の導入)
- [SKK の基本操作](#skk-の基本操作)
  - [補完候補の削除](#補完候補の削除)
- [設定内容](#設定内容)
  - [辞書](#辞書)
  - [日付・時刻入力](#日付時刻入力)
  - [キーバインド](#キーバインド)
  - [動作設定](#動作設定)
  - [配色](#配色)
  - [かな拡張入力](#かな拡張入力)
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
| SKK サーバー（[yaskkserv2](https://github.com/wachikun/yaskkserv2) 推奨・[google-ime-skk](https://github.com/hitode909/google-ime-skk) など） | 127.0.0.1:1178 の辞書サーバー。変換を高速化したい場合は完全オフラインの **yaskkserv2** を推奨（[導入手順](#ローカル-skk-サーバーyaskkserv2の導入)） | 任意 |

### 手順

1. CorvusSKK をインストールし、Windows の「設定 → 時刻と言語 → 言語と地域」で日本語キーボードに CorvusSKK が追加されていることを確認する
2. HackGen Console NF フォントをインストールする
3. この `config.xml` を CorvusSKK の設定フォルダにコピーする

   ```powershell
   Copy-Item config.xml "$env:APPDATA\CorvusSKK\config.xml"
   ```

4. IME をオフ → オン（Ctrl+Space を 2 回）して設定を反映する
5. （任意）SKK サーバーを 127.0.0.1:1178 で起動する（→ [ローカル SKK サーバー（yaskkserv2）の導入](#ローカル-skk-サーバーyaskkserv2の導入)）。使わない場合は設定ダイアログの「辞書」タブで SKK 辞書サーバーを無効にする

> **初回起動時**: URL 指定の辞書が `%TMP%\CorvusSKK` に自動ダウンロードされます。ネットワーク環境によっては時間がかかります。

### ローカル SKK サーバー（yaskkserv2）の導入

変換を高速化したい場合は、完全オフラインで動作する [yaskkserv2](https://github.com/wachikun/yaskkserv2) を `127.0.0.1:1178` で常駐させます。yaskkserv2 は辞書を事前にバイナリ化してローカルで引くため応答がサブミリ秒で、ネットワーク越しの google-ime-skk のような遅延が乗りません。

> **設定との対応**: この `config.xml` は `serv=1` / `host=127.0.0.1` / `port=1178` / `encoding=0`（EUC-JP）/ `timeout=100`（ms）で設定済みです。`timeout=100` は「応答が間に合わなければ候補が欠けてもよいので遅延を最短にする」方針で、サーバー停止・遅延時も待ちは最大 100ms で頭打ちになります。
>
> コマンドのフラグはバージョンで変わることがあるため、必ず `yaskkserv2 --help` / `yaskkserv2_make_dictionary --help` で確認してください。

1. **入手**

   [リリースページ](https://github.com/wachikun/yaskkserv2/releases) から `yaskkserv2` / `yaskkserv2_make_dictionary` を入手するか、Rust 環境で `cargo build --release` でビルドします。

2. **辞書ソースの用意**

   `config.xml` で自動ダウンロード済みの辞書（`%TMP%\CorvusSKK`）か、[skk-dev/dict](https://github.com/skk-dev/dict) から `SKK-JISYO.L` などを取得します（`.gz` は展開しておく）。

3. **サーバー辞書のビルド**

   ```powershell
   yaskkserv2_make_dictionary --dictionary-filename dictionary.yaskkserv2 `
     SKK-JISYO.L SKK-JISYO.jinmei SKK-JISYO.fullname SKK-JISYO.geo `
     SKK-JISYO.station SKK-JISYO.propernoun SKK-JISYO.assoc
   ```

   既定は EUC-JP です。UTF-8 で運用する場合は `--utf8` を付け、`config.xml` の `encoding` も `1`（UTF-8）に合わせます。

4. **サーバー起動**（`127.0.0.1:1178` で待ち受け）

   ```powershell
   yaskkserv2 --listen-address 127.0.0.1 --port 1178 dictionary.yaskkserv2
   ```

   未登録語のみ Google 予測変換にフォールバックさせたい場合は `--google-japanese-input=notfound` 等を併用すると、ローカルの速さと予測変換を両取りできます（変換の往復がネット経由になる語のみ遅延します）。

5. **常駐（自動起動）設定**

   サインインのたびに yaskkserv2 を自動起動するには、**タスクスケジューラ**で登録するのが確実です（コンソールウィンドウを出さずにバックグラウンド常駐できます）。以下では `C:\tools\yaskkserv2\` に `yaskkserv2.exe` と `dictionary.yaskkserv2` を置いた前提で説明します（パスは環境に合わせて読み替えてください）。

   #### 方法 A: `schtasks` コマンドで一括登録（推奨）

   管理者権限の PowerShell で以下を実行します。`/RL HIGHEST`（最上位の特権）と `/IT` の組み合わせで、ログオン時にウィンドウ非表示で起動します。

   ```powershell
   schtasks /Create /TN "yaskkserv2" /SC ONLOGON /RL HIGHEST /F `
     /TR "C:\tools\yaskkserv2\yaskkserv2.exe --listen-address 127.0.0.1 --port 1178 C:\tools\yaskkserv2\dictionary.yaskkserv2"
   ```

   - 登録の確認: `schtasks /Query /TN "yaskkserv2"`
   - すぐに起動して動作確認: `schtasks /Run /TN "yaskkserv2"`
   - 解除（削除）: `schtasks /Delete /TN "yaskkserv2" /F`

   > **コンソールが一瞬見える場合**: `yaskkserv2.exe` はコンソールアプリのため、`/TR` で直接起動するとログオン時に一瞬ウィンドウが表示されることがあります。完全に非表示にしたい場合は、後述の起動用ラッパー（VBScript）を `/TR` に指定してください。

   #### 方法 B: タスクスケジューラ GUI で登録

   1. `taskschd.msc` を起動 →「基本タスクの作成」
   2. 名前: `yaskkserv2`
   3. トリガー: **「ログオン時」**
   4. 操作: **「プログラムの開始」**
      - プログラム/スクリプト: `C:\tools\yaskkserv2\yaskkserv2.exe`
      - 引数の追加: `--listen-address 127.0.0.1 --port 1178 C:\tools\yaskkserv2\dictionary.yaskkserv2`
   5. 作成後、タスクのプロパティを開き
      - 「全般」タブ → **「最上位の特権で実行する」**にチェック
      - 同タブ → **「ユーザーがログオンしているかどうかにかかわらず実行する」**は外したまま（ログオン中のみ常駐させる場合）
      - 「設定」タブ → **「タスクを停止するまでの時間」のチェックを外す**（常駐し続けるため）
      - 「条件」タブ → 「コンピューターを AC 電源で使用している場合のみ…」のチェックを外す（ノート PC でも常駐させる場合）

   #### コンソールを完全に隠す（任意）

   ウィンドウを一切出さずに常駐させたい場合は、VBScript のラッパー経由で起動します。`C:\tools\yaskkserv2\start-hidden.vbs` を作成:

   ```vbscript
   ' start-hidden.vbs — yaskkserv2 をウィンドウ非表示で起動
   CreateObject("WScript.Shell").Run _
     """C:\tools\yaskkserv2\yaskkserv2.exe"" --listen-address 127.0.0.1 --port 1178 ""C:\tools\yaskkserv2\dictionary.yaskkserv2""", 0, False
   ```

   タスクの `/TR`（または GUI のプログラム）を次のように差し替えます:

   ```powershell
   schtasks /Create /TN "yaskkserv2" /SC ONLOGON /RL HIGHEST /F `
     /TR "wscript.exe C:\tools\yaskkserv2\start-hidden.vbs"
   ```

   #### 方法 C: スタートアップフォルダ（手軽だが非推奨）

   `Win+R` → `shell:startup` で開くフォルダに上記 `start-hidden.vbs` へのショートカットを置くだけでもログオン時に起動できます。ただし特権昇格やウィンドウ制御が効きにくいため、基本は方法 A / B を推奨します。

   > **更新・再起動**: 辞書を作り直したときや設定を変えたときは、`schtasks /End /TN "yaskkserv2"`（または タスクマネージャーで `yaskkserv2.exe` を終了）してから `schtasks /Run /TN "yaskkserv2"` で再起動します。

6. **動作確認**

   ```powershell
   Test-NetConnection 127.0.0.1 -Port 1178
   ```

   接続できたら CorvusSKK で `▽` 変換し、候補が即座に表示されることを確認します。サーバーを停止すると `timeout=100` 後にローカル辞書のみで変換されます（候補が一部欠けることがありますが遅延は最小です）。

> **エンコーディング整合の注意**: サーバーの配信エンコーディングと `config.xml` の `encoding` を必ず一致させてください（不一致だと候補が文字化け／表示されません）。

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

補完候補（▽モードで自動表示される読み・Tab で出る読み）は、過去に変換・確定した読み＝**ユーザー辞書の見出し語**です。
不要な読みが補完に出る場合は、次の手順でユーザー辞書から削除します。

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
| SKK サーバー | 127.0.0.1:1178（yaskkserv2 など。[導入手順](#ローカル-skk-サーバーyaskkserv2の導入)） |

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

- **補完**: 動的補完（▽モードで入力中に自動表示）・Tab 補完とも複数候補をリスト表示（最大 5 件）
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
| 変換が遅い・引っかかる | SKK サーバー（127.0.0.1:1178）が起動していない場合は、[ローカル SKK サーバー（yaskkserv2）の導入](#ローカル-skk-サーバーyaskkserv2の導入)でサーバーを常駐させるか、設定ダイアログ「辞書」タブでサーバーを無効化。`config.xml` は `timeout=100`（ms）で応答待ちを短く設定済み |
| 絵文字が変換できない | Direct2D ＋ カラーフォントの表示設定が必要（この設定では有効済み） |
| 候補一覧のフォントがおかしい | HackGen Console NF がインストールされているか確認 |
| ASCII モードから戻れない | Ctrl+Space を 2 回押して IME を入れ直す（この設定では jmode キーを割り当てていないため） |
| 不要な読みが補完に表示される | [補完候補の削除](#補完候補の削除)の手順でユーザー辞書から削除 |

## 関連リンク

- [CorvusSKK](https://github.com/nathancorvussolis/corvusskk) — 本体・公式ドキュメント
- [CorvusSKK Releases](https://github.com/nathancorvussolis/corvusskk/releases) — インストーラー
- [skk-dev/dict](https://github.com/skk-dev/dict) — SKK 辞書（この設定の辞書取得元）
- [HackGen](https://github.com/yuru7/HackGen) — 候補一覧用フォント
- [ddskk](https://github.com/skk-dev/ddskk) — Emacs 版 SKK（SKK 操作体系の本家）
