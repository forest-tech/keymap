[日本語](README.md) | [English](README_en.md)

# Kanata keymap: MacBook Air JIS

これは、Vim で使いやすい ASCII 記号配置にした Kanata の `.kbd` ファイルです。
[`forest-tech/my-karabiner-elements`](https://github.com/forest-tech/my-karabiner-elements)
をもとにしたドラフトで、実際の MacBook キーボードではまだテストしていません。

## 設定

- `mac-jis.kbd`: Apple の内蔵 JIS キーボードのみを対象とします。
- 3 つの親指キー操作: `eisu` はタップで Eisu、ホールドで Command、
  `Space` はタップで Space、ホールドで Control、`kana` はタップで Kana、
  ホールドで Shift になります。
- `Caps` はタップで Escape、ホールドで Symbol1/Numbers、
  `Caps` をホールドしながら `kana` で Symbol2 になります。
- Home Row Mods と Navigation レイヤーはありません。
- 以前の Karabiner 設定にあった Emacs Control のリマップは、ターミナル、
  tmux、Neovim のショートカットを上書きしてしまうため有効にしていません。

## 各機能の有効化・無効化

`mac-jis.kbd` の先頭にある `FEATURE SWITCHES` ブロックを編集します。

| 変数 | ON（デフォルト） | OFF |
| --- | --- | --- |
| `feature-eisu` | `@eisu-cmd` | `eisu` |
| `feature-space` | `@space-ctrl` | `spc` |
| `feature-kana` | `@kana-shift` | `kana` |
| `feature-symbol1` | `@caps-symbol1` | `esc` |
| `feature-symbol2` | `@hold-symbol2` | `XX` |

Space/Ctrl の反応を速くするには、`feature-space` を
`@space-ctrl-fast` に変更します。通常のキー入力を素早く行ったときに、
意図しない Ctrl ショートカットが発生することがあります。
`feature-symbol1 = esc` にすると Symbol2 も使用できなくなります。
これらのフラグは、Kanata を再起動するか設定を再読み込みしたときに適用されます。

タイミング変数も同じ `defvar` セクションにあります。まずはデフォルト値から始め、
意図しないショートカットや過度な遅延がある場合にホールド時間を調整してください。

## 記号レイヤーの出力

各記号の上に表示されているキーは、物理的なアルファベットキーです。
ドット（この表では `-`）は、元のキーに専用の記号マッピングがないことを示します。

| 物理キー | Q | W | E | R | T | Y | U | I | O | P |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Caps = Symbol1 | - | @ | % | ` | ! | { | 7 | 8 | 9 | , |
| Caps + kana = Symbol2 | - | @ | % | ~ | & | } | 7 | 8 | 9 | . |

| 物理キー | A | S | D | F | G | H | J | K | L | ; |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Symbol1 | - | ' | / | : | * | ( | 4 | 5 | 6 | < |
| Symbol2 | - | " | ? | ; | # | ) | 4 | 5 | 6 | > |

| 物理キー | Z | X | C | V | B | N | M | , | . | / | Space |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Symbol1 | - | $ | = | - | \| | [ | 1 | 2 | 3 | / | 0 |
| Symbol2 | - | ^ | + | _ | \ | ] | 1 | 2 | 3 | ? | 0 |

これら 2 つのレイヤーで、印字可能な ASCII 記号 32 個と数字 `0-9` を網羅しています。
JIS 配列は macOS の入力ソース側で使用します。このファイルが送信するのはキーコードであり、
Unicode のグリフではありません。

## macOS でのセットアップ・テスト

1. 使用する Kanata のバージョンに対応する Karabiner-DriverKit-VirtualHIDDevice
   ドライバーがインストールされていることを確認します。
   [macOS 公式セットアップ手順](https://github.com/jtroo/kanata/blob/main/docs/setup-macos.md)
   に従ってください。
2. Kanata を起動する前に、干渉を避けるため **Karabiner-Elements の有効なリマップや
   キーボードの取得を無効にします**。共有ドライバーはアンインストールしないでください。
3. 内蔵キーボードの正確な名前を確認します: `kanata --list`。
   現在、このファイルでは `"Apple Internal Keyboard / Trackpad"` を使用しています。
   デバイスが別の名前で表示される場合は `macos-dev-names-include` を編集してください。
4. 設定の構文を確認します（デバイスの取得は不要です）。
   `kanata --check --cfg ./mac-jis.kbd`
5. macOS の公式セットアップ手順に従って Kanata を起動します。例:
   `sudo kanata --cfg ./mac-jis.kbd`
6. プレーンテキストエディター（ABC 入力ソース）でテストし、その後 Ghostty と Neovim
   でもテストします。

### スモークテスト

- `Caps` をタップ -> Escape、`Caps + F` をホールド -> `:`
- `Caps + D` -> `/`、`Caps + kana + D` -> `?`
- `Caps + X` -> `$`、`Caps + kana + X` -> `^`
- `Caps + G` -> `*`、`Caps + kana + G` -> `#`
- `Caps + H` -> `(`、`Caps + kana + H` -> `)`
- `Caps + U/I/O` -> `7/8/9`、`Caps + Space` -> `0`
- `Caps + B` -> `|`、`Caps + kana + B` -> バックスラッシュ
- `Space` を約 190 ms ホールドしてから `F` を押す -> Control+F
- Neovim で `j` を連続入力する（通常の `j` の連続入力として動作すること）
- ABC と日本語の両方の入力モードで、円記号/バックスラッシュキーと IME の動作を確認する

**既知の制限:** macOS 上での実機キーボードテストはまだ行っていません。
Apple JIS の記号出力は、アクティブな入力ソースに依存します。円記号/バックスラッシュ、
パイプ、アンダースコア、波括弧は特に確認が必要です。Symbol2 のトリガーは Caps と
kana を同時にホールドする必要があり、押しにくい可能性があります。これは意図的に
初期のエルゴノミクス実験として残しています。

## 参照

- [Kanata 設定ガイド](https://jtroo.github.io/config.html)
- [Kanata macOS セットアップ](https://github.com/jtroo/kanata/blob/main/docs/setup-macos.md)
- [以前の Karabiner マップ](https://github.com/forest-tech/my-karabiner-elements)
