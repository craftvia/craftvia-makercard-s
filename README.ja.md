# CraftVia MakerCard-S

> [!WARNING]
> **未検証のRev0ハードウェアです**  
> Rev0基板は発注済みで、現在は到着待ちです。未実装・未通電・未検証であり、製造・組立は自己責任となります。

[English](README.md) · [CraftVia](https://craftvia.org/ja/) · [Rev0リリースノート](RELEASE_NOTES.ja.md)

[Rev0仕様書](docs/specification-v1-rev0.ja.md) · [Rev0検証計画](docs/verification-plan-v1-rev0.ja.md) · [CraftVia共通文書](https://github.com/craftvia/craftvia-docs/blob/main/README.ja.md)

![CraftVia MakerCard-S v1 Rev0 CADレンダリング](docs/images/makercard-s-v1-rev0-3d.webp)

*Rev0 CADレンダリング — [表面](docs/images/makercard-s-v1-rev0-front.webp) · [裏面](docs/images/makercard-s-v1-rev0-back.webp)。画像は説明用であり、凍結済み設計SourceとGerber Packageを正とします。*

CraftVia MakerCard-Sは、Core-SクラスのMCU回路を実用的な試作形式へ持ち出すためのReference実装です。Core-S G031回路と、ブレイクアウト、プロトタイプ配線領域、電源レールを一枚にまとめ、一品物、治具、初期アプリケーション試作での利用を想定しています。

Station上の開発から、専用CarrierまたはMCU直載せ基板へ進む途中の実装経路を示します。

## 現在の状態

| 項目 | 状態 |
| --- | --- |
| 製品Version | v1 |
| PCB Revision | Rev0 |
| 基板製造 | 発注済み・到着待ち |
| 実装 | 未着手 |
| 通電 | 未実施 |
| 電気的検証 | 未実施 |
| Release tag | `v1-rev0`を予定 |

## Rev0概要

- 基板外形: 91.0 mm × 55.0 mm
- PCB: 4層
- 3.2 mm NPTH取付穴4か所
- STM32G031K8T6 Core-S回路を内蔵
- Core-Sブレイクアウト・プロトタイプ配線領域
- SJ1-SJ4による設定可能な試作電源レール
- 標準クロック: 内蔵HSI。外付け発振器オプションはDNP
- CADソース: EasyEDA

## Repo構成

```text
source/                      正本のEasyEDA編集可能ソース
fabrication/                 正本のGerberパッケージ
assembly/                    BOM、CPL、組立参考資料
docs/                        回路図PDFと製品固有文書
product.json                 Web参照用の機械可読製品Metadata
RELEASE_NOTES.md / RELEASE_NOTES.ja.md
                             Rev0の詳細と初品確認項目（英語正本／日本語翻訳）
SHA256SUMS.txt               整合性確認用manifest
```

## Rev0の重要条件

基板発注・実装前に[`v1-rev0`のリリースノート](RELEASE_NOTES.ja.md)を必ず確認してください。

- Gerber ZIPだけがPCB製造の正本です。
- 出力済みCPLは未フィルタであり、BOMとの照合なしに実装サービスへ使用できません。
- J1-J7はスルーホール部品であり、通常はTHT実装サービスまたは手実装が必要です。
- ヘッダ寸法とCore-S／Station-Sの機械的スタックを確認する必要があります。
- SJ1-SJ4は、電源レールの接続を理解・検証するまでオープンのままにします。
- 回路図PDFには過去のDraft表記が残っており、製造正本ではありません。

## Maker Profileと配布用途

MakerCard-Sの裏面は、Maker Profileとして使用できるデザインになっています。試作基板として使用するだけでなく、イベント、展示、ワークショップ、対面での交流などにおいて、名刺やプロジェクト紹介として配布することを想定しています。

Rev0のReference設計には、想定用途の例としてKohei Ikedaの公開プロフィール情報が含まれています。自身のMakerCard-Sとして製造・配布する場合は、裏面のプロフィール文を自身の情報へ置き換えてください。Reference設計のQRコードは[CraftVia.org](https://craftvia.org)へのリンクであり、プロフィールを変更する場合もそのまま使用できます。

主なカスタマイズ対象は裏面のプロフィールですが、適用されるハードウェアライセンスの範囲で表面の配置や回路を変更することもできます。改変した基板にもCraftVia Trademark Policyが適用され、CraftVia公式製品であるかのように表示することはできません。

氏名やプロフィール情報が設計に含まれていることは、本人へのなりすましや、本人による推奨・承認を示す表示を許可するものではありません。

## 初品検証

外形・取付穴、プロトタイプ領域の導通・絶縁、電源レールの初期状態、ヘッダ方向、電流制限付き通電、電源電圧・電流、SWD、BOOT動作、ブレイクアウト対応を確認します。検証後も`v1-rev0`タグは移動せず、設計変更が必要な場合は新しいRevisionを作成します。

## ライセンスとブランド

個別の表示がない限り、本リポジトリのハードウェア設計ファイルは[CERN-OHL-P-2.0](LICENSE)で提供します。区分と帰属表示については[CraftVia Licensing（英語正本）](LICENSES/README.md)および[参考訳](LICENSES/README.ja.md)を参照してください。

CraftViaの名称・ロゴはKohei Ikedaが所有し、ハードウェアライセンスの対象外です。[CraftVia Trademark Policy（英語正本）](LICENSES/TRADEMARK_POLICY.md)および[参考訳](LICENSES/TRADEMARK_POLICY.ja.md)を参照してください。改変品や第三者製造品を公式CraftVia製品として表示することはできません。
