# 参考資料

確認日: 2026-10-31

## 製作検討の出発点

ユーザー所有のGoogle Drive「自動操舵（AgOpenGPS）」フォルダにある次の資料を確認した。

- AgOpenGPS_製作・構築手順書
- AgOpenGPS_All_In_One_Master_BOM_v5.xlsx
- AgOpenGPS_DirectDrive_Smartphone_BOM.xlsx

これらにはMassey Fergusonの取付前提、複数の基板・受信機候補、スマホを利用できるという前提、仮単価、手持ち品の記載が含まれる。本プロジェクトではISEKI向けに見直し、手持ち部品はゼロとして扱う。

元ファイルは再配布せず、必要な前提と検討項目を整理している。部品の適合や取付寸法の根拠として、そのまま利用しない。

## 公式情報

- [AgOpenGPS 入門](https://docs.agopengps.com/getting-started/): 構成要素と従来のWindows端末構成。
- [AIO基板の選定・製作](https://docs.agopengps.com/hardware/boards/all-in-one-boards/aio-board-versions/): Standard / Microの区別、製造ファイルと別途調達部品。
- [DIY操舵モーター](https://docs.agopengps.com/hardware/steering-options/diy-motor/): 12V / 24Vモーター、取付例、手動時の離脱・抵抗対策。
- [Cytron MD13S](https://my.cytron.io/p-13a-6v-30v-dc-motor-driver): 電圧・電流・制御のメーカー仕様。
- [AgOpenGPSソース](https://github.com/AgOpenGPS-Official/AgOpenGPS): 従来版AgOpenGPSとAgIO。
- [AgOpenWeb](https://github.com/AgOpenGPS-Official/AgOpenWeb): 別系統のクロスプラットフォーム実装。採用は未決定。

採用時には参照したコミット・リリース・基板版を追記する。3Dモデル、基板設計、コードを取り込む際は、取得元とライセンスを同時に記録する。
