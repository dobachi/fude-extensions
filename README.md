# fude-extensions

[Fude](https://github.com/dobachi/fude) エディタのダウンロード型拡張機能カタログです。
本体を軽量に保つため、重い描画エンジンなどはここから**オンデマンドで配布**します。

Fude の設定 →「拡張機能」から有効化すると、`manifest.json` を取得し、各ファイルを
SHA-256 で検証してダウンロード・キャッシュします。

## カタログ (`manifest.json`)

`manifest.json` はリポジトリ root に置かれ、Fude 本体は
`https://raw.githubusercontent.com/dobachi/fude-extensions/main/manifest.json`
を参照します。各拡張のファイル実体はリリースアセットとして配布します。

## 提供中の拡張

### PlantUML Preview (`plantuml`)

- PlantUML 図をローカル（サーバー不要・初回DL後はオフライン）で描画。
- 実体は PlantUML 公式の JavaScript ビルド（TeaVM）と Graphviz の WebAssembly ビルド（Viz.js）。
  - `plantuml.js` — PlantUML エンジン（ESモジュール。`renderToString` をエクスポート）
  - `viz-global.js` — クラス図・コンポーネント図などの dot レイアウト用
- 取得元: https://plantuml.github.io/plantuml/js-plantuml/
- バージョン: 1.2026.6beta1

### Mermaid Preview (`mermaid`)

- Mermaid 図をローカル（サーバー不要・初回DL後はオフライン）で描画。
- `.mmd` / `.mermaid` ファイル、および Markdown 内の ` ```mermaid ` フェンスに対応。
- 実体は Mermaid 公式の UMD ビルド（`window.mermaid` を公開・自己完結／動的import無し）。
  - `mermaid.min.js` — Mermaid エンジン
- 取得元: https://cdn.jsdelivr.net/npm/mermaid@11.16.0/dist/mermaid.min.js
- バージョン: 11.16.0

ライセンス・著作権は各上流に従います。[NOTICE.md](NOTICE.md) を参照してください。

## リリース手順（メンテナ向け）

1. 公式 js ビルドを取得（または `plantuml` リポジトリを `./gradlew clean teavm -Pfast` でビルド）:
   ```
   curl -LO https://plantuml.github.io/plantuml/js-plantuml/plantuml.js
   curl -LO https://plantuml.github.io/plantuml/js-plantuml/viz-global.js
   sha256sum plantuml.js viz-global.js   # manifest.json の sha256 と size を更新
   ```
2. 上記2ファイルを `plantuml-<version>` タグのリリースにアップロード。
3. `manifest.json` の version / url / sha256 / size を更新して main にコミット。

### Mermaid

1. 公式 UMD ビルドを取得（`window.mermaid` を公開する自己完結ビルド）:
   ```
   curl -LO https://cdn.jsdelivr.net/npm/mermaid@<version>/dist/mermaid.min.js
   sha256sum mermaid.min.js   # manifest.json の sha256 と size を更新
   ```
2. `mermaid.min.js` を `mermaid-<version>` タグのリリースにアップロード。
3. `manifest.json` の version / url / sha256 / size を更新して main にコミット。
