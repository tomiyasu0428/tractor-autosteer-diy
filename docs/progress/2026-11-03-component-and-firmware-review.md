# 2026-11-03 進捗：部品構成とソフト適合

状態: 調査・暫定候補。部品は未選定・未購入。ビルド、ファームウェア書込み、配線変更、モーター駆動、車体試験は未実施。

## 対象と今回進んだこと

新規ISEKI95への導入を対象とする。正式型式・電源・操舵負荷は未確認。現在の別トラクターは現在の構成で完結させ、本案の変更対象にしない。

- [元のプランA](../proposals/plan-a-wired-lan.md)のUM982二アンテナ・屋根バー・すっきりしたキャビンという目的を維持した。
- 完成自動操舵キットを前提にせず、単品と必要な部品小セットを組み合わせ、初回支出を抑えることを目標に候補を整理した。最安や総額削減を確認したものではない。
- メーカーの測位部品価格、Windows版AOG/AgIOと公開ファームウェアの経路・課題を確認した。
- Codexの一次資料確認と、実際のClaude Code相談の意見を分けて記録した。
- 今回の更新は既存PRの文書追記のみ。プランAや過去レビューを保存し、採用済み・検証済みへ状態を進めない。

## 暫定構成と比較を残す部分

| 部分 | 暫定候補・比較方針 | 採用前に必要な確認 |
|---|---|---|
| 操舵制御 | AIO v4.5 Micro＋Ethernet付きTeensy4.1を第一候補 | 正確な基板版・実装内容・対応FW。Standardも基板形状と初回実装費を揃えて比較 |
| DC駆動 | Cytron MD13Sを軸に比較 | モーターの定格・拘束電流、箱内温度、電源・配線・保護。BTS7960は実モジュールの定格・品質・FW対応未確認の対案 |
| 操舵機構 | 純正ハンドル＋ブラシ付きDCギアモーター＋歯車 | 操舵負荷、回転数、歯車比、取付・干渉、手動離脱。モーター品番は保留 |
| WAS | 採用FWに合うアナログ舵角センサー | 正確な型番、電源・出力、全舵角・車軸揺動時の可動範囲、断線時検出 |
| 測位案1 | F9P単アンテナ＋BNO085 | 既存FW経路と基板の組合せ、時刻・方位・ロール処理。今回の機体で検証済みではない |
| 測位案2 | UM982二アンテナ＋必要に応じIMU | 品質・時刻・軸・オフセット・age処理の検証。IMU完全撤去は未決定 |
| キャビン収納 | 足元の一つの箱へ基板と必要なLANスイッチを同居 | 熱、モーター電力部と信号部の配置、整備・ペダル・操舵との干渉 |
| 屋根分離／PoE | 測位方式とは別の追加設計 | 屋根通信MCU、PSE/PD・電源変換、起動・瞬断復旧、防水・固定・基線剛性、追加費用 |

Microは対応する測位部品セットと小型化の点で暫定優先とするが、常に最安とは限らない。Standard/MicroでGNSS受信ボードの形状・ピンは異なるため混ぜない。AIO基板上に実装済みのADCやEthernet接続部品も確認し、ADS1115・RJ45部品等の二重購入を避ける。Nano／ESP32／Teensyを任意に交換できる構成とは扱わない。

低背アンテナはGNSS用の対応帯域・能動給電・グランドプレーン・位相中心・耐候性を確認して選ぶ。外観が小さいことだけで同等性能を保証しない。二アンテナは主→副の基線方位が基準で、左右配置では車両前方への補正と傾斜の符号確認が必要。一つの基線で三軸姿勢すべてを独立観測するものではない。

## 測位部品の価格確認

2026-11-03のメーカー掲載価格・在庫表示。注文・到着・実機適合の確認ではない。送料、税、関税、為替差は別途未見積。

| 商品・SKU | メーカー掲載内容 | 表示価格・在庫 |
|---|---|---|
| [F9P用RTK Items Set][P1] / AS-STARTKIT-AOGPS-AIOF9P-SMA-00 | simpleRTK2B Micro F9P×1、ANN-MB-00×1、設定用USB-C carrier×1 | 252 EUR・在庫あり表示 |
| [UM982用RTK Items Set][P2] / AS-STARTKIT-AOGPS-AIOUM982-SMA-00 | simpleRTK3B Micro UM982 dual SMA×1、ANN-MB2×2、設定用USB-C carrier×1 | 324 EUR・在庫あり表示 |

メーカーは両商品をAIO v4.5 Micro向けと記載している。これは測位部品の小セットで、AIO PCB、Teensy、モータードライバー、WAS、操舵モーター、タブレットは含まない。基板への適合表示も、今回の全ソフト経路の動作保証ではない。

72 EURはこの部品群の価格差だけ。F9P側のIMU、UM982側の追加ソフト検証、屋根分離・PoEや構造材を含む総額差ではない。単体のF9P Budget／UM982 Compassは形状が異なる別比較枠に置く。

[既存BOM](../bom.md)の46,400円／54,400円は、不完全な過去の仮単価合計。現在の総予算や購入実績に使わない。AIO初回製造・実装枚数、端末、RTK通信、WAS、机上電源、工具、フィラメント、昇圧、ケース、配線・ヒューズ・遮断・離脱、送料税を未確定費用として残す。

| 総額見積に必要な費目 | 金額の状態 |
|---|---|
| AIO PCBの初回全枚数・実装・別途実装部品 | 未見積 |
| Teensy・ドライバー・WAS・必要なIMU | 未見積 |
| モーター・必要な昇圧・歯車・離脱・取付部品 | 未見積 |
| タブレット・給電・取付・RTK補正通信 | 未見積 |
| 屋根筐体・アンテナ固定・MCU・LANスイッチ・PoE | 未見積。採用する構成のみ計上 |
| 配線・端子・ヒューズ・遮断・保護・防水 | 未見積 |
| 机上電源・工具・フィラメント・送料・税 | 未見積 |

未見積は0円ではない。掲載価格と調達先を揃え、同じ構成範囲で初回支払総額を比較する。

## ソフト経路：コードで確認した範囲

| 対象 | 固定して確認した版 | 確認範囲 |
|---|---|---|
| Windows版AgOpenGPS／AgIO | [6.8.6 / 21be26aa58d09b9b5abacc715d0abcf4a021f1db][S1] | UDP受信・NMEA解析・NTRIP転送・AOGへの連携 |
| UM982屋根MCU候補 | [chriskinal/AOG_Teensy_UM982 / 7ab01065bfd3ce34c46a20c5898ffae65b11b60e][S2] | Teensy4.1、KSXT透過／HPR変換、EthernetとRTCM戻し |
| 足元操舵候補 | [AgOpenGPS-Official/Boards / c8a3da731059fe27af254af68d6dc0db7dc03939][S3] の通常AIO_v4_Firmware | WAS ADC・UDP操舵指令・DC駆動・watchdog |
| ESP32比較 | [TiagoK-777/AOG_esp32-eth0_UM982 / 3c197b89bd034e7dbd3a91b285855282be046185][S4] | 実装は存在するが、作者READMEは二アンテナの圃場試験未実施と記載 |

```text
補正サービス → タブレット内AgIO NTRIP
  → UDP 2233 → 屋根MCU → UART → UM982

UM982 → UART → 屋根MCU（KSXT透過 または HPR等を変換）
  → UDP 9999 → AgIO → AOG

AOG → AgIO → UDP 8888 → 足元操舵MCU → WAS閉ループ／ドライバー
```

確認したUM982実装では5120は送信元ポート、AgIOへの宛先は9999。AOGとAgIOのローカル通信は15555／17777。実際の選定ネットワークでサブネット、屋根と操舵ノードのIP重複、二重GPS送信を確認する。

UM982固有のGNHPRをそのままAgIOへ投げる構成ではない。6.8.6のAgIOはKSXT／PAOGI／PANDA等のパーサーを持つがGNHPR直接解析分岐はなく、HPR経路はMCUで変換する。PAOGIは方位・rollを度の実数として、PANDAは0.1度単位の整数として扱う。通常AIO FWのF9P UBX/RELPOSNED経路とUM982 HPR経路も同一ではない。[S5]

## 採用前に残る具体的な課題

1. **KSXTの解釈:** 最新Unicore N4 R1.15と照合すると、AgIO 6.8.6はword6のpitchをrollへ転用し、word20のreserved欄を補正ageとして読む。heading品質不良でもheading値を取り込む経路がある。KSXT透過は最短の疎通確認候補であり、無改修の操舵採用決定ではない。[S5][S6]
2. **HPR変換の品質・時刻:** chriskinal版は品質判定の前にreadyを立て、`quality >= 4`を使用する。Float等も含み、GGA/HPRの同一epoch照合も確認できない。QF=0は無効、4はRTK Fix、5はFloat、6は推測航法。QF=4だけでも独立した方位解の有効性を証明するものではない。[S2][S6]
3. **傾斜・単位・補正:** 横基線のpitchを車両rollとして利用する前提、主副アンテナの向き、符号・単位、前後傾斜、位置の補正基準を確認する。heading offsetの補正場所を受信機・MCU・AOGの一か所に定め、二重補正しない。
4. **RTCM受信長:** Teensy候補の512バイト配列への上限確認は不足する。ただしAgIO 6.8.6内蔵NTRIPは通常256バイトに分割して転送するため、既定経路で実際に溢れるとは断定しない。RTCMフレーム長とUDPデータグラム長を混同せず、他送信元やserial-passは別に確認する。[S2][S7]
5. **停止・再開:** 確認した通常操舵FWは25ms周期・閾値100から約2.5秒で駆動停止する静的推定。実測値ではなく「即停止」と記載しない。短い通信断から有効指令が戻った際の自動再開排除も保証されない。手動解除、MCUに依存しない電源遮断、停止後の明示的な再エンゲージを確認する。[S3]
6. **版と機種ロジック:** HPR対応の別AIO FWにはKeya処理が混在する。DCモーター向けの名前・設定・入力・出力が合うか確認し、通常FWや他MCU版と無条件に組み合わせない。ツールチェーン・依存ライブラリも固定してビルド再現性を確認する。

Unicore R1.15は初回評価では取得がタイムアウトしていたが、後の調査でメーカーPDFを取得してHPR・KSXT表を照合した。従来Windows 6.8.6の確認結果であり、別系統のAgValonia等まで対応確認したものではない。

## 次の机上検証計画（未実施・モーター非接続）

| 段階 | 記録する内容と完了条件 |
|---|---|
| 版・入力の固定 | AOG/AgIO、基板版、MCU/FWコミット、依存ライブラリ、UM982ボード/FW、アンテナ、出力形式・周期を一組で記録する |
| 記録再生・疎通 | 正常ログから9999受信とAOG表示を確認。heading境界、単位、roll符号、オフセットと補正位置を照合する |
| 無効・古い入力 | 品質低下、時刻不一致、片側データ欠落、古い値、破損メッセージで、誤って有効な新しい測位として扱わないことを確認する |
| 補正の戻り道 | UDP2233→UARTのバイト列と欠落・切詰めを確認し、補正ageとRTK状態の扱いを照合する |
| 操舵ノード出力の測定 | モーターは接続せず、WAS相当入力・有効無効指令・断線／通信断・再接続を用いて出力だけを測定する。許容停止時間は先に定め、復旧のみで再開しないことを確認する |
| 屋根／PoEの追加 | 上記の経路確認後、選定したPSE/PD、車載電源保護、起動・瞬断復旧と追加費用を別に評価する |

購入や車体試験へ進む判断は、上記結果と正式型式・電源・操舵負荷・取付空間を揃えてから行う。「既存コードがある」「相互接続できそう」「今回の実機で合格した」を別々の状態として管理する。

将来の試験記録には、日付、全構成の版、入力ログ・条件、期待値・事前に定めた許容値、実測値、判定、ログ／パケット記録、未解決事項を残す。上表は検証計画であり、合格済み項目はない。

## Claude Codeとの検討の位置づけ

部品構成とソフト成立性について、既存の認証済みClaude Codeへ実際に追加相談した。機能要約・公開資料の読取結果だけを渡し、ツール・MCP・ファイル参照を無効にした。Claude自身による外部資料の独立検証ではなく、Codexの一次資料調査に対する別レビューである。

追加相談での意見は「Micro対応の測位部品小セットを経済比較に残す」「F9PかUM982かは未決定」「MD13Sを軸に共通操舵系を整理」「屋根分離とPoEを別段階にする」。

今回の進捗文面も2026-11-03にClaude Code 2.1.306でレビューし、終了コード0で回答を得た。Claudeは価格・コード等を独自確認していないと明記し、「経済性は目標」「72 EURは測位セットだけの差」「約2.5秒は静的推定・未実測」「部品は暫定候補」「モーター非接続の計画と総額費目を明示」と指摘した。これらを反映した。過去の円額と原提案は履歴として保持し、現在の予算不可との注記を加える方法を採った。マージや部品採用を促す過去の意見は今回の実行判断に含めない。

Claudeの提案をそのまま採用済み判断にしない。特に「F9P通常FWをそのまま検証済み」「HPR QF=4だけで方位有効」「PRをマージしてBOM固定」という扱いにはしない。KSXTも疎通検証と操舵採用を区別する。元の[Claude初回記録](../proposals/plan-a-claude-review.md)と[Codex初回評価](../proposals/plan-a-codex-review.md)は履歴として残す。

## 参照資料

[P1]: https://www.ardusimple.com/product/agopengps-all-in-one-aio-board-rtk-items-set-u-blox-zed-f9p/
[P2]: https://www.ardusimple.com/product/agopengps-all-in-one-aio-board-rtk-items-set/
[S1]: https://github.com/AgOpenGPS-Official/AgOpenGPS/releases/tag/6.8.6
[S2]: https://github.com/chriskinal/AOG_Teensy_UM982/tree/7ab01065bfd3ce34c46a20c5898ffae65b11b60e
[S3]: https://github.com/AgOpenGPS-Official/Boards/blob/c8a3da731059fe27af254af68d6dc0db7dc03939/TeensyModules/AIO%20v4%20Firmware/AIO_v4_Firmware/Autosteer.ino
[S4]: https://github.com/TiagoK-777/AOG_esp32-eth0_UM982/blob/3c197b89bd034e7dbd3a91b285855282be046185/README_EN.md
[S5]: https://github.com/AgOpenGPS-Official/AgOpenGPS/blob/21be26aa58d09b9b5abacc715d0abcf4a021f1db/SourceCode/AgIO/Source/Forms/NMEA.Designer.cs
[S6]: https://en.unicore.com/uploads/file/Unicore%20Reference%20Commands%20Manual%20For%20N4%20High%20Precision%20Products_V2_EN_R1.15.pdf
[S7]: https://github.com/AgOpenGPS-Official/AgOpenGPS/blob/21be26aa58d09b9b5abacc715d0abcf4a021f1db/SourceCode/AgIO/Source/Forms/NTRIPComm.Designer.cs

- [P1 F9P測位部品セット][P1]
- [P2 UM982測位部品セット][P2]
- [S1 Windows版AOG/AgIO 6.8.6][S1]
- [S2 UM982用Teensy候補][S2]
- [S3 通常操舵FW][S3]
- [S4 ESP32比較候補][S4]
- [S5 AgIOパーサー][S5]
- [S6 Unicore N4 R1.15 §7.4.9／§7.5.49][S6]
- [S7 AgIO NTRIP転送][S7]
