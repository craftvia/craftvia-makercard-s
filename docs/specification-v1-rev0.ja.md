# CraftVia MakerCard-S v1 Rev0 仕様書

> [!WARNING]
> **状態: 設計凍結済み、ハードウェア未検証**  
> Rev0は発注済みですが、実装、通電、電気的検証はまだ行われていません。プロトタイプ領域の接続、機械的適合性、内蔵Core回路はいずれも初品検証が必要です。

この文書は英語正本 [`specification-v1-rev0.md`](specification-v1-rev0.md) の日本語翻訳版です。解釈に差異がある場合は英語版を優先します。

## 1. 識別情報と正本

| 項目 | 内容 |
| --- | --- |
| 製品 | CraftVia MakerCard-S |
| 製品Version | v1 |
| PCB Revision | Rev0 |
| MCU | STM32G031K8T6 |
| CAD | EasyEDA |
| 凍結済み編集可能ソース | [`../source/CraftVia-MakerCard-S-v1-Rev0.eprj2`](../source/CraftVia-MakerCard-S-v1-Rev0.eprj2) |
| 製造正本 | [`../fabrication/Gerber_PCB1_2026-09-30.zip`](../fabrication/Gerber_PCB1_2026-09-30.zip) |
| Release記録 | [`../RELEASE_NOTES.md`](../RELEASE_NOTES.md) |

Rev0では、凍結済みソース、製造正本、正規BOM、Release記録を優先します。回路図・組立PDFは人が読むための参考資料です。

## 2. 用途

MakerCard-Sは、Core-SクラスのMCU回路を実用的な試作・紹介用フォーマットへ展開するためのReference実装です。次の要素を一枚にまとめています。

- Core-S G031 Rev0 MCU回路
- Core-S接点ヘッダと直接接続されたブレイクアウト列
- 5穴単位で導通したブレッドボード形式のストリップ
- 任意接続用リンクを持つ、初期状態で絶縁された試作電源レール
- 独立した大面積2.54 mm試作領域
- Maker Profile領域とCraftViaプロジェクトQRコード

MakerCard-Sは統合方法を示すものであり、Carrierに別基板のCoreを装着した構成ではありません。

## 3. 物理構造

| 項目 | Rev0実装 |
| --- | --- |
| 基板外形 | 91.0 mm x 55.0 mm、角丸長方形 |
| 角半径 | 3.0 mm |
| 銅箔層数 | 4 |
| 取付穴 | 3.2 mm NPTH、4か所 |
| 穴中心 | `(5.5,5.0)`、`(85.5,5.0)`、`(5.5,50.0)`、`(85.5,50.0)` mm |
| グリッド | 試作・ブレイクアウトとも2.54 mm |
| 裏面SMT | なし |

基板外形がCore-Sホストから張り出す場合があります。電気的な接点対応だけではStation-Sとの機械的互換性は保証されないため、嵌合とクリアランスを検証します。

## 4. 内蔵Core-S G031回路

MCU、電源、SWD/BOOT、Reset、クロックオプション回路は、Core-S G031 v1 Rev0の設計意図と同じです。

| 機能 | Rev0構成 |
| --- | --- |
| 電源 | `K-SW`に3.3 V、`K-NW`と`K-NE`にGND |
| MCU | STM32G031K8T6、LQFP32 |
| Debug | N1 NRST、N2 SWDIO、N3 SWCLK、N4 Boot要求 |
| 標準クロック | HSI16。外付け8 MHz発振回路はDNP |
| Decoupling | C1/C3=100 nF、C2=4.7 uF |
| 電源リンク | R7=0 ohm、実装 |
| PC14/PC15リンク | R3およびR10、実装 |

### 接点対応

| 接点 | MCU／Node | 接点 | MCU／Node |
| --- | --- | --- | --- |
| N1 | PF2-NRST | E1 | PA0 |
| N2 | PA13/SWDIO | E2 | PA1 |
| N3 | PA14/SWCLK/BOOT0、R1経由 | E3 | PA2 |
| N4 | PA14/BOOT0、R2経由 | E4 | PA3 |
| N5 | PA11 | E5 | PA4 |
| N6 | PA12 | E6 | PA5 |
| N7 | PB8 | E7 | PA6 |
| N8 | ブレイクアウトNetのみ。MCU接続なし | E8 | PA7 |
| S1 | PA8 | W1 | PA9／推奨USART1_TX |
| S2 | PB0 | W2 | PA10／推奨USART1_RX |
| S3 | PB1 | W3 | PB6／推奨I2C1_SCL |
| S4 | PC6 | W4 | PB7／推奨I2C1_SDA |
| S5 | PB2 | W5 | PB3／推奨SPI1_SCK |
| S6 | PB9 | W6 | PB4／推奨SPI1_MISO |
| S7 | PC14、R3経由 | W7 | PB5／推奨SPI1_MOSI |
| S8 | PC15、R10経由 | W8 | PA15／推奨SPI1_NSS |

N3とN4は独立したMCU Pinではありません。Boot要求を能動的に駆動する前にSWCLKを切り離してください。

## 5. Core接点とブレイクアウト

J1-J4はN、E、S、Wの1x8ヘッダ、J5-J7は3つのKey／電源ヘッダです。これらは正規BOMに含まれますが、通常はTHT実装サービスまたは手実装が必要です。

J8-J11は4辺の8接点を複製した未実装ブレイクアウト列、J12-J14は3つのKey接点の複製です。これらのブレイクアウトDesignatorは正規BOMから除外されています。

N8はヘッダとブレイクアウト位置の間だけ配線され、内蔵G031回路のMCUには接続されません。

## 6. 試作領域と電源レール

### 6.1 ブレッドボード形式ストリップ

BB1-BB12は、それぞれ5穴が導通したストリップです。同じストリップ内は一つのNetで、異なるストリップ間は絶縁されています。これらは銅箔パターン／Footprintであり、実装コネクタではありません。

### 6.2 電源レール

BBP1-BBP4は、互いに独立した8穴のレールです。

| レール | 任意接続リンク | 接続元 | Rev0初期状態 |
| --- | --- | --- | --- |
| `BB_PWR_A` | SJ1 | +3V3 | Open／DNP |
| `BB_PWR_B` | SJ2 | GND | Open／DNP |
| `BB_PWR_C` | SJ3 | +3V3 | Open／DNP |
| `BB_PWR_D` | SJ4 | GND | Open／DNP |

導通、意図した極性、接続負荷を確認するまでSJ1-SJ4をBridgeしないでください。レールの表示は任意接続時の接続元を示し、製造時点で接続済みであることを意味しません。

### 6.3 Universal領域

Universal 2.54 mm領域は、使用者が配線を追加しない限り各Plated Through Holeが独立しています。初品では領域全体の絶縁と、4本の電源レールからの絶縁を確認します。

## 7. 実装対象

正規BOMにはCore-S G031 Rev0と同じ16 Designator、`C1`、`C2`、`C3`、`J1-J7`、`R1`、`R2`、`R3`、`R7`、`R10`、`U1`が含まれます。

次はDNPまたはBOM除外です。

- クロック／Resetオプション: `C4`、`R4`、`R5`、`R6`、`R8`、`R9`、`R11`、`X1`
- Bare Test Point: `TP1-TP5`
- ブレイクアウト／試作機能: `BB1-BB12`、`BBP1-BBP4`、`J8-J14`、`SJ1-SJ4`

未フィルタのCPLには56 Designatorが含まれるため、16個のBOM実装対象と照合してフィルタします。J1-J7はThrough Hole部品であり、通常のSMT実装サービスでは実装されません。

## 8. Maker Profileとブランド領域

裏面のMaker Profile領域は、第三者が個人用基板を製造・配布する前にカスタマイズすることを想定しています。例示された個人のプロフィール情報を、配布者自身が使用権限を持つ情報へ置き換えてください。

ReferenceのQRコードは `https://craftvia.org/mc` を指しており、そのまま使用できます。Open Hardwareとしての許諾は、他者へのなりすまし、推奨を示唆する表示、改変品や第三者製造品をCraftVia公式製品として表示することを許可しません。リポジトリのTrademark Policyに従ってください。

## 9. Rev0の既知の制約

- 実装・通電済みSampleはまだ検証されていません。
- 大きな基板外形を含むCore-S／Station-Sの機械的嵌合は未検証です。
- J1-J7のヘッダ寸法と実装方法を確認する必要があります。
- Prototype Stripの導通、Universal領域の絶縁、SJの初期Open状態は未検証です。
- 消費電流、Boot動作、SWD、全ブレイクアウト対応は未検証です。
- 任意の発振回路は未検証かつDNPです。
- 回路図PDFには過去のDraft表記が残っています。

## 10. 検証

[`verification-plan-v1-rev0.ja.md`](verification-plan-v1-rev0.ja.md)を使用してください。Maker Profile Artwork、QRコード、穴領域、電源レール表示はRev0の識別・使用性確認の対象であるため、実装前後の両面写真を保存します。
