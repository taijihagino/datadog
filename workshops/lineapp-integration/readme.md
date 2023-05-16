## はじめに
こちらは、<a href="https://zenn.dev/arahabica/books/d4373bd4401d6c" target="_blank" rel="noopener noreferrer">LINE APIを使ったサンプルアプリケーション</a>をDatadogで監視するためのハンズオンワークショップです。

対象のアプリケーションは、LIFFアプリ・ミニアプリで会員証発行・入退場管理を行うアプリです。具体的には、LIFFアプリでQRコードを発行し、店舗側のQRコードリーダーでそれを読み込むシーンを想定しています。このアプリは、AWS上にデプロイされているものとなります。

最初にインテグレーションを行い、まずはDatadog全体の機能として、インテグレーションしたAWSのリソースに対してDatadog上でどのように情報が見えて監視できるのかを操作していきます。基本的にはDatadogの使い方をチュートリアル形式で説明通り操作していくワークショップになります。

その後、Dashboardを新たに作成し、ビジネスKPIを設定した上でそこに対しての実際のデータを可視化し、設定したKPIに対するActualを管理できる形にモディファイしていくということを行います。これは、Datadogの使い方の一つであり、DatadogがBIとしての側面を持っていることを体感していただくものになります。

## Datadogのアカウント作成

## DatadogとAWSのインテグレーション

## Dashboardの確認

![Screenshot 2023-05-16 at 15 34 58](https://github.com/taijihagino/datadog/assets/12064399/a7b99fcf-d080-4e1a-945c-ea1bd7b0cfa4)

## インフラモニタリング

### インフラホストマップの確認

![Screenshot 2023-05-16 at 15 37 22](https://github.com/taijihagino/datadog/assets/12064399/7dcf0c38-20fc-4040-84d6-9f5186d344eb)

### 表示グループをリージョンでカテゴライズ

![Screenshot 2023-05-16 at 15 38 36](https://github.com/taijihagino/datadog/assets/12064399/67d1dedc-129a-4f74-a3bd-dbe1113864da)

### 表示されているホストのAWSを見てみる

![Screenshot 2023-05-16 at 15 39 27](https://github.com/taijihagino/datadog/assets/12064399/3415e07d-f8c6-4ff9-bd4c-6764db9c1dac)


## アプリケーションパフォーマンスモニタリング（APM）

### サービスマップの確認

![Screenshot 2023-05-16 at 15 42 13](https://github.com/taijihagino/datadog/assets/12064399/a5c4de0d-7313-4b57-8f6c-e8b333af96c6)

### 特定のサービスにフォーカスし依存関係を確認

![Screenshot 2023-05-16 at 15 44 29](https://github.com/taijihagino/datadog/assets/12064399/d4b04d5b-da3c-4f03-9c05-f2f5010da66a)

### サービスをドリルダウン

![Screenshot 2023-05-16 at 15 45 38](https://github.com/taijihagino/datadog/assets/12064399/ba5c55e2-30d1-4e69-8985-2dc6760a71c0)

### 障害が発生した場合に影響を受けるサービスと与えるサービスを確認

![Screenshot 2023-05-16 at 15 46 27](https://github.com/taijihagino/datadog/assets/12064399/241fdda7-3f52-45e7-8279-8b3a29b26688)

### サービスのオーバービューへの遷移

![Screenshot 2023-05-16 at 15 49 53 copy 3](https://github.com/taijihagino/datadog/assets/12064399/2a825600-f414-48da-ade6-6d447442a578)


### サービスの関連トレース情報への遷移

![Screenshot 2023-05-16 at 15 49 53](https://github.com/taijihagino/datadog/assets/12064399/d310ed6c-2936-4222-b27a-16fb79b838f4)


### サービスの関連ログ情報への遷移

![Screenshot 2023-05-16 at 15 49 53 copy](https://github.com/taijihagino/datadog/assets/12064399/e327120e-8c1a-472e-8f23-50e373da63da)


### サービスの関連プロセス情報への遷移

![Screenshot 2023-05-16 at 15 49 53 copy 2](https://github.com/taijihagino/datadog/assets/12064399/56a4dcdd-9080-46d5-b97e-f9649dfe3418)


## リアルユーザーモニタリング（RUM）

### セッション＆リプレイ
![Screenshot 2023-05-16 at 15 55 50](https://github.com/taijihagino/datadog/assets/12064399/9f6aa06e-3ada-4c72-9ded-f6f4143f7e4e)

### シンセティックテスト

![Screenshot 2023-05-16 at 15 57 31](https://github.com/taijihagino/datadog/assets/12064399/926b819e-7ede-460f-9263-0c8d51e0c74c)


## モニター

### 新しいモニターの作成

![Screenshot 2023-05-16 at 16 03 06](https://github.com/taijihagino/datadog/assets/12064399/5014f9b5-3167-4099-824b-033d04bad77c)

![Screenshot 2023-05-16 at 16 04 40](https://github.com/taijihagino/datadog/assets/12064399/14e398fc-fb30-45ea-85de-36861378f084)

![Screenshot 2023-05-16 at 16 05 30](https://github.com/taijihagino/datadog/assets/12064399/d739879f-b73e-4da7-8855-8b13a9b11305)

![Screenshot 2023-05-16 at 16 05 49](https://github.com/taijihagino/datadog/assets/12064399/fb7242db-eafa-4175-8478-cf2214e9f640)


## BIとしてのDatadog活用

### Dashbordの作成


### ウィジェットの追加


### KPIの設定


### 完成








