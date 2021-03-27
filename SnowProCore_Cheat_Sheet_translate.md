
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

### // Connecting to SF //
```
- Web-based UI (see above for usage,capabilities, restriction)
- Command line client (SnowSQL)
- ODBC and JDBC (you have to download the
driver!)
- Native Connectors (Python, Spark & Kafka)
- Third party connectors (e.g. Matilion)
- Others (Node.js, .Net)

Snowflake Address
https://account_name_region_{provider}.snowflakecomputing.com
Account_name either:
- AWS = account_name.region
- GCP/Azure = account_name.region.gcp/azure
e.g. https://pp12345.ap-southeast-2.snowflakecomputing.com
```
<img width="730" alt="スクリーンショット 2021-03-27 19 47 54" src="https://user-images.githubusercontent.com/56843306/112718571-3def5380-8f37-11eb-90bf-07f082cb945b.png">

- Web ベースの UI (使用方法、機能、制限については上記を参照)
    - コマンドラインクライアント(SnowSQL)
    - ODBCおよびJDBC（ドライバーをダウンロードする必要があります。)
    - ネイティブコネクタ（Python、Spark & Kafka）
    - サードパーティのコネクタ（Matilionなど）
    - その他 (Node.js, .Net)

### Snowflakeのアドレス
    - https://account_name_region_{provider}.snowflakecomputing.com
- アカウント名
    - AWS = アカウント名.リージョン
    - GCP/Azure = アカウント名.リージョン.gcp/azure
例： https://pp12345.ap-southeast-2.snowflakecomputing.com


### // Architecture //
### Overview
```
- Hybrid of shared-disk db and shared-nothing db
- Uses central data repo for persisted storage - All compute nodes have data access
- Using MPP clusters to process queries - Each node stores portion of data locally Architectural layers
```
- Shared-Disk DB and Shared-Nothing DBのハイブリッド
  - Shared-Disk DB and Shared-Nothing DBについて
    - 違い　https://www.publickey1.jp/blog/09/ibm_db2oracle_rac.html
    - shared-noting https://e-words.jp/w/%E3%82%B7%E3%82%A7%E3%82%A2%E3%83%BC%E3%83%89%E3%83%8A%E3%83%83%E3%82%B7%E3%83%B3%E3%82%B0.html
    - Snowflake公式：https://docs.snowflake.com/ja/user-guide/intro-key-concepts.html

- 永続的なストレージとして中央のデータリポジトリを使用 - すべてのコンピュートノードがデータにアクセス可能
- MPPクラスターを使用してクエリを処理 - 各ノードはローカル設計レイヤーデータの一部を保存
    - MPPアーキテクチャ
        - Snowflake公式：https://docs.snowflake.com/ja/user-guide/intro-key-concepts.html

<img width="622" alt="スクリーンショット 2021-03-27 22 24 07" src="https://user-images.githubusercontent.com/56843306/112722167-2e7a0580-8f4b-11eb-8818-faf0e369cc08.png">

### Storage
```
- All data stored as an internal, optimised, compressed columnar format (micro-partitions - represents logical structure of table)
- Can’t access the data directly, only through Snowflake (SQL etc)
```
- すべてのデータは、内部で最適化され、圧縮されたカラムナフォーマット（マイクロパーティション - テーブルの論理構造を表す）で保存されます。
- データに直接アクセスすることはできず、Snowflakeを介してのみアクセス可能（SQLなど）

### Query Processing
```
- Where the processing of query happens
- Uses virtual warehouses - an MPP compute cluster
- Each cluster = Independent - doesn’t share resources with other vwh and doesn’t impact others
```
- クエリの処理を行う場所
- 仮想ウェアハウスを使用 - MPPコンピュートクラスター
    - 各クラスタ = 独立している - 他のVirtual ウエアハウスとリソースを共有せず、他に影響を与えない

### Cloud Services
```
- Command centre of SF - coordinates and ties all activities together within SF
- Gets provisioned by Snowflake and within AWS, Azure or GCP
- You don’t have access to the build of this or to do any modifications
- Instance is shard to other SF accounts unless you have VPS SF Edition
- Services that this layer takes care of:
    - Authentication
    - Infrastructure management
    - Metadata management
    - Query parsing and optimisation
    - Access control
```
- SFの司令塔 - SF内のすべての活動を調整し、結びつける
- Snowflakeによって、AWS、Azure、GCP内でプロビジョニングされます。
    - プロビジョニング（provisioning） 
         - https://www.idcf.jp/words/provisioning.html
- お客様には構築や変更の権限はありません。
- インスタンスは、VPS Snowflake Editionでない限り、他のSnowflakeアカウントにシャードされます。
     - シャード
        - https://azure.microsoft.com/ja-jp/overview/what-is-database-sharding/
- このレイヤーが担当するサービス
    - 認証
    - インフラ管理
    - メタデータの管理
    - クエリの解析と最適化
    - アクセス制御

### Caches
```
- Snowflake Caches different data to improve query performance and assist in reducing cost 

>Metadata Cache - Cloud Services layer
    - Improves compile time for queries against commonly used tables

>Result Cache - Cloud Services layer
    - Holds the query results
    - If Customers run the exact same query within 24 hours, result cache is used and no warehouse is required to be active
>Local Disk Cache or Warehouse Cache - Storage Layer
    - Caches the data used by the SQL query in its local SSD and memory
    - This improves query performance if the same data was used (less time to fetch remotely)
    - Cache is deleted when the Warehouse is suspended
```
- Snowflake 様々なデータをキャッシュすることで、クエリのパフォーマンスを向上させ、コスト削減を支援します。
    - （大）メタデータキャッシュ - クラウドサービス層
        - よく使われるテーブルに対するクエリのコンパイル時間を短縮します。
    - （中）リザルトキャッシュ - クラウドサービス層
        - クエリの結果を保持
        - お客様が24時間以内に全く同じクエリを実行した場合、結果キャッシュが使用され、ウェアハウスをアクティブにする必要はありません。
    - （小）ローカルディスクキャッシュまたはウェアハウスキャッシュ - ストレージ層
        - SQLクエリで使用されるデータをローカルのSSDとメモリにキャッシュします。
        - これにより、同じデータが使用された場合、クエリのパフォーマンスが向上します（リモートでのフェッチにかかる時間が短くなります
        - キャッシュはウェアハウスが停止すると削除されます。
