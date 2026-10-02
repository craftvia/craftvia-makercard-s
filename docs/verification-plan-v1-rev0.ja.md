# CraftVia MakerCard-S v1 Rev0 検証計画

> 状態: 未検証Rev0ハードウェア用の検証計画Draftです。結果が公開されるまで、動作実績を示すものではありません。

この文書は英語正本 [`verification-plan-v1-rev0.md`](verification-plan-v1-rev0.md) の日本語翻訳版です。解釈に差異がある場合は英語版を優先します。

## 1. 試験記録

| 項目 | 記録 |
| --- | --- |
| 基板Serial／Sample ID |  |
| 基板表示 | `CraftVia MakerCard-S Rev0` |
| 実装元・実装日 |  |
| BOM差異・Rework |  |
| 試験日・担当者 |  |
| 測定器・校正状態 |  |
| 電源の電流制限値 |  |
| Firmware Commit／Build |  |
| Station／Fixture Revision |  |
| Maker Profileのカスタマイズ状態 |  |

各試験は **Pass**、**Fail**、**Blocked**、**Not run** のいずれかで記録し、測定値とEvidenceを添付します。

## 2. 安全条件と中止条件

- 初期検査とCore Bring-upの間はSJ1-SJ4をOpenに保ちます。
- Hostへ挿入・取り外しするときは必ず無通電にします。
- 初回通電では電流制限付き3.3 V電源を使用します。
- 想定外の電流、電圧低下、発熱、発煙、異臭、試作領域の短絡があれば中止します。
- 各試作電源レールは、測定で確認するまで無通電として扱います。

## 3. 必要機材

- Microscopeまたは拡大鏡
- Digital MultimeterとContinuity Fixture
- 電流制限付き3.3 V電源
- SWD Probeまたは検証済みStation-S
- OscilloscopeまたはLogic Analyzer
- Core全接点、ブレイクアウト列、Strip、Rail、試験対象Universal Pad用のFixtureまたはLead
- 基本Bring-up Firmware

## 4. 試験項目

| ID | 試験 | 方法 | 合格基準 | Evidence |
| --- | --- | --- | --- | --- |
| `MCS-R0-001` | Release識別 | 基板表示、Source Release、BOM、Gerber、SHA-256 Manifestを照合する。 | 全識別情報がv1 Rev0を示し、FileがManifestと一致する。 | 写真、Checksum Log |
| `MCS-R0-002` | Artwork・外観 | 両面、QRコード、Profile文、はんだ付け、極性、DNP Site、Hole Platingを確認する。 | Artworkが判読可能で使用権限上問題がなく、実装がBOMと一致し、目視欠陥がない。 | 高解像度写真 |
| `MCS-R0-003` | 外形・取付穴 | 91 x 55 mm外形、3 mm角半径、4つの3.2 mm NPTHを測定する。 | 寸法と穴位置が製造公差内でRelease仕様と一致する。 | 測定表 |
| `MCS-R0-004` | ヘッダ機構 | J1-J7の向き、Pin寸法、突出量、整列、Clearanceを確認する。 | 意図した向きであり、衝突や過大な力なく嵌合する。 | 写真、測定値 |
| `MCS-R0-005` | 無通電電源検査 | `+3V3`／`VDD_CORE`からGNDへの抵抗とR7導通を測定する。 | 短絡がなく、R7電源経路が導通する。 | 抵抗値 |
| `MCS-R0-006` | Core接点対応 | J1-J7を公開済みN/E/S/W/Key対応と照合し、N8のMCUからの絶縁も確認する。 | 全接点が正しく、N8は対応Breakout Netのみに接続し、隣接短絡がない。 | 接点Matrix |
| `MCS-R0-007` | ブレイクアウト複製 | J8-J14をJ1-J7と照合する。 | 全Breakout接点が対応するCore接点だけに導通する。 | 導通Matrix |
| `MCS-R0-008` | 5穴Strip | BB1-BB12について、Group内とGroup間を確認する。 | 各Stripの5穴が導通し、異なるStrip間は絶縁される。 | Strip Matrix |
| `MCS-R0-009` | 電源レール・リンク | LinkがOpenの状態でBBP1-BBP4とSJ1-SJ4を確認する。 | 各8穴Rail内が導通し、Rail間が絶縁され、初期状態で+3V3/GNDに接続されない。 | Rail Matrix |
| `MCS-R0-010` | Universal領域 | 全独立Padまたは代表PadをFixtureで、隣接Pad、Rail、GND、+3V3に対して確認する。 | 独立設計のPadが絶縁され、意図しない短絡がない。 | Fixture Log |
| `MCS-R0-011` | 初回通電 | Prototype負荷なしで、意図した入力から保守的な電流制限を設定して3.3 Vを印加する。 | 電源が安定し、電流制限に到達せず、異常発熱がない。 | 電流Trace、温度記録 |
| `MCS-R0-012` | 電源電圧 | `+3V3`、`VDD_CORE`、U1 VDD/VDDAを測定する。 | 各Nodeが3.135–3.465 Vに収まり、相互に整合する。 | 電圧表 |
| `MCS-R0-013` | SWD・Reset | N1-N3またはTest Pointを介して、検出、Erase、Program、Debug、Reset、Connect-under-resetを行う。 | 正しいDevice IDで全操作が繰り返し成功する。 | Tool Log |
| `MCS-R0-014` | Boot選択 | SWCLKを切り離し、N1/N4 Boot Sequenceを実行する。 | 競合なくNormal BootとSystem Memory Bootを選択できる。 | Option Byte、Boot Log |
| `MCS-R0-015` | 標準通信 | W1/W2 UART、W3/W4 I2C、W5-W8 SPIを試験する。 | 双方向転送が成功し、接点対応が正しい。 | Bus Capture |
| `MCS-R0-016` | ADC・Timer・GPIO | Device制限内でE1-E8、S1-S8、N5-N7を動作させる。 | 各接点対応が正しく、ADC値とWaveformが記録される。 | 測定Log |
| `MCS-R0-017` | 任意Rail Link | 初期Open試験の合格後、管理された条件でSJを1本ずつBridgeする。 | 選択Railだけが意図した+3V3またはGNDへ接続し、Bridge除去後に絶縁へ戻る。 | Before/After Matrix |
| `MCS-R0-018` | Station嵌合 | 機械的に安全な場合、無通電でStation-Sへ装着し、電源、SWD、Reset、UARTを再試験する。 | 衝突がなく、装着状態と向きが適切で、同等の電気動作をする。 | 写真、再試験Log |
| `MCS-R0-019` | QR／Profile使用性 | QRコードをScanし、通常の閲覧距離でProfile文を確認する。 | QRが意図したCraftVia URLを開き、権限のない個人情報が含まれない。 | Scan結果、写真 |
| `MCS-R0-020` | 代表試作回路 | 1つのStrip、独立Pad領域、必要に応じて1つの接続済みRailを使い、小規模な回路を記録付きで製作する。 | 回路が設計どおり動作し、意図しない接続が見つからない。 | 製作記録、写真 |

## 5. 検証判定

Rev0を **Verified** と表示できるのは、内蔵Core、全Breakout構造、初期状態で絶縁されたRail、Universal領域、意図する機械的使用方法が合格した場合だけです。SJ接続時の任意構成とStation嵌合は、それぞれ別の検証済み構成として報告できます。PCBまたは実装対象を修正する場合は新しいRevisionが必要です。
