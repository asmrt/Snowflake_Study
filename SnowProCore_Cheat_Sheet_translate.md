
# SnowProCore Cheatsheet（*）を翻訳してみた

## 概要
- 以下SnowProCore Cheatsheetを日本語翻訳してみて、SnowflakeとDB関連の英語知識を
深めるためのドキュメント
    - https://www.slideshare.net/JenoYamma/snowflake-snowpro-certification-exam-cheat-sheet

- ＋自分で不明だった点を調べたメモおよび参考URLの掲載

注意：日本語翻訳は誤っている可能性があります。画像は元資料のものを引用しました。

## 翻訳 

### // Snowflake - General //

### Introduction
```
- Analytic data warehouse
- SaaS offering
- No hardware (or installation or updates/patch)
- No ongoing maintenance or tuning
- Can’t run privately (on-prem or hosted)
- Runs completely on the cloud
- Either AWS, Azure or GCP (NO ON-PREM or HYBRID!)
- Has its own VPC!
- Decoupled compute and storage (scaled compute does not need scaled storage)
``` 
- 分析用のデータウエアハウス
- SaaSで提供している
- ハードウェア（またはインストール、アップデート、パッチ）が不要　　
- 継続的なメンテナンスやチューニングが不要
- プライベート（オンプレやホステッド）環境では動かない
- 完全にクラウド環境で動く
- AWS,Azure,GCPでの利用が可能    
- 独自のVPC環境を持っている！ 
- コンピュートとストレージの切り離し（コンピュートのスケールアップにストレージのスケールアップは必要ない)

### Pricing
```
- Unit costs for Credits and data storage determined by region (not cloud platform)
- Cost of Warehouse used
- Minimum 60s on resuming, per 1s after
- Cost of storage (Temp, Transient, Perm Tables & time-travel data & fail-safe)
- Calcs based on daily average
- Pricing model = on demand or discounted upfront
- Just remember:
- >= Enterprise = 90 days time travel (default: 1 day for all) + materialized view +
multi-cluster warehouse
- >= Business Critical = lots more security (HIPAA, SOC 1&2, PCI DSS)
- VPS edition = own cloud service layer (not shared with accounts)
```

- （クラウドプラットフォームではなく）regionごとに決定されるクレジットとデータストレージの単価
- 使用するウエアハウスのコスト
- 再開時は最低60秒、再開後は1秒ごと
- ストレージのコスト
   -Temp, Transient, Perm Tables（永久テーブル）、Time-Travelデータ、Fail-Safe)
- 1日の平均値に基づいて計算
- 価格モデル = オンデマンドまたは前払い割引
- Snowflakeエディションごとの違い
    - >= Enterprise =
       -  90日間のタイムトラベル (デフォルト: 1日) 
       - マテリアライズド・ビューの使用
       - マルチクラスターウェアハウス

    - Business Critical 
       - より多くのセキュリティ機能 (HIPAA、SOC 1&2、PCI DSS)
    - VPS edition
        - 独自のクラウドサービス層 (アカウントと共有しない)

## Supported Regions
```
- Multi-region account isn’t supported, each SF in single region
- 17 Regions in total
- AWS - 9 regions (Asia Pacific-3, EU-2, North America-4)
- GCP - 1 (Asia Pacific-0, EU-0, North America-1) >> in preview
- Azure - 7 (Asia Pacific-2, EU-1, North America-4)
```
- マルチリージョンには対応していませんが、各SFは1つのリージョンになります。
- 全部で17リージョン利用可
    -  AWS
        - 9リージョン（アジアパシフィック-3、EU-2、北米-4)
    - GCP
        - 1（アジア太平洋地域-0、EU-0、北米-1） >> プレビュー中
    - Azure
        - 7（アジア太平洋地域-2、EU-1、北米-4）
