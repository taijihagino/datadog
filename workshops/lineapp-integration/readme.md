## はじめに
こちらは、<a href="https://zenn.dev/arahabica/books/d4373bd4401d6c" target="_blank" rel="noopener noreferrer">LINE APIを使ったサンプルアプリケーション</a>をDatadogで監視するためのハンズオンワークショップです。

対象のアプリケーションは、LIFFアプリ・ミニアプリで会員証発行・入退場管理を行うアプリです。具体的には、LIFFアプリでQRコードを発行し、店舗側のQRコードリーダーでそれを読み込むシーンを想定しています。このアプリは、AWS上にデプロイされているものとなります。

最初にインテグレーションを行い、まずはDatadog全体の機能として、インテグレーションしたAWSのリソースに対してDatadog上でどのように情報が見えて監視できるのかを操作していきます。基本的にはDatadogの使い方をチュートリアル形式で説明通り操作していくワークショップになります。

その後、Dashboardを新たに作成し、ビジネスKPIを設定した上でそこに対しての実際のデータを可視化し、設定したKPIに対するActualを管理できる形にモディファイしていくということを行います。これは、Datadogの使い方の一つであり、DatadogがBIとしての側面を持っていることを体感していただくものになります。

## Datadogのアカウント作成
<a href="https://www.datadoghq.com/ja/" target="_blank">DatadogのWebサイト</a>へアクセスします。

右上の「無料で試す」ボタンをクリックします。
<img width="1480" alt="Screenshot 2023-05-27 at 19 07 39" src="https://github.com/taijihagino/datadog/assets/12064399/7d0fa859-d73e-47f1-9d71-c0c94b289c21">

必要事項を入力してアカウントを作成します。リージョン、メールアドレス、氏名、会社名（所属団体名）、パスワード、が必須項目になります。
<img width="539" alt="Screenshot 2023-05-27 at 19 08 57" src="https://github.com/taijihagino/datadog/assets/12064399/1ede41f8-dc60-4f20-9118-ceeb6b8b8b9a">

次の画面で、どのサービスやソフトウェアを使ってるか？所属してる組織で管理してるサーバーの数や、サービスプロバイダーかどうか、Datadogを使ってまずは何をやりたいか、などを聞かれますが空欄のまま「Next」をクリックして次へ進みます。

次に「Install your first Datadog Agent」という画面に遷移します。手っ取り早くDatadogの機能を試してみたい場合は、Mac OS XやWindowsを選択して、自分のローカルマシンとインテグレーションしてみると良いと思います。本ハンズオンワークショップではここの手順はスキップします。
<img width="1475" alt="Screenshot 2023-05-27 at 19 19 21" src="https://github.com/taijihagino/datadog/assets/12064399/b079f445-7585-4196-b76a-7c023e9b9b8d">

左上のDatadogロゴ（犬のマーク）をクリックしてメインのプラットフォーム画面へ移りましょう。
<img width="963" alt="Screenshot 2023-05-27 at 19 20 41" src="https://github.com/taijihagino/datadog/assets/12064399/16133fd4-e60c-420e-89c9-84a020914006">

このような画面になればOKです！
<img width="1478" alt="Screenshot 2023-05-27 at 19 21 55" src="https://github.com/taijihagino/datadog/assets/12064399/ef2f717c-aa2e-4393-9b7d-c14d57c03996">

## DatadogとAWSのインテグレーション
AWSとのインテグレーションを行うことでAWSへのサービスが作成され、わずかながらも課金が発生する可能性がありますので、自己責任にて十分ご注意の上実施ください。

左側のメニューから **Integrations** → **Integrations** を選択します。検索窓で「AWS」と入力するとAWSのパネルが表示されるので、それを選択します。
<img width="1479" alt="Screenshot 2023-05-27 at 19 22 38" src="https://github.com/taijihagino/datadog/assets/12064399/4cb559c1-9297-44aa-8718-9dc6b012f366">

Amazon Web Servicesの画面で「+ Add AWS Account(s)」をクリックします。
<img width="1321" alt="Screenshot 2023-05-27 at 19 25 00" src="https://github.com/taijihagino/datadog/assets/12064399/14263bea-9860-4fb4-93a0-bb2d276c9c53">

自分のAWSの環境に合わせ、リージョンを選択します。「Add Datadog API Key」セクションでは「Select API Key」をクリックしてください。
<img width="1330" alt="Screenshot 2023-05-27 at 19 30 35" src="https://github.com/taijihagino/datadog/assets/12064399/4bc94bc0-2fb4-4818-ad8f-cbb83f21b4fa">

Datadogのアカウント作成時に生成されているAPI Keyが表示されます。ラジオボタンを選択して、右下の「Use API Key」ボタンをクリックします。
<img width="1040" alt="Screenshot 2023-05-27 at 19 31 25" src="https://github.com/taijihagino/datadog/assets/12064399/90cbb17f-5f4f-4de3-8528-c00ff9d132dd">

内容に問題がなければ、右下の「Launch CloudFormation Template」ボタンをクリックしてください。AWSのCloudFormation スタック作成の画面が開きます。
<img width="1314" alt="Screenshot 2023-05-27 at 19 32 02" src="https://github.com/taijihagino/datadog/assets/12064399/30cc8477-0b6f-46dd-88ba-c69624e4a50e">

デフォルトでテンプレートやパラメーターがセットされた状態ですので、内容を確認して右下の「スタックの作成」ボタンをクリックします。
<img width="1231" alt="Screenshot 2023-05-27 at 19 39 35" src="https://github.com/taijihagino/datadog/assets/12064399/adf42c86-07fe-4ffb-bb38-ea0ddd8c3191">

## Datadog機能紹介
ここからは、一旦ハンズオンワークショップから離れて、Datadogの機能を講師が紹介していきます。すでにいくつかのサービスやソフトウェアとインテグレーションが完了している方は、同様に自分の画面も動かしてみてくだい。そうでない方は、講師が紹介する画面を見てどのような機能があるのかを学んでみてください。

### Dashboardの確認

![Screenshot 2023-05-16 at 15 34 58](https://github.com/taijihagino/datadog/assets/12064399/a7b99fcf-d080-4e1a-945c-ea1bd7b0cfa4)

### インフラモニタリング

#### インフラホストマップの確認

![Screenshot 2023-05-16 at 15 37 22](https://github.com/taijihagino/datadog/assets/12064399/7dcf0c38-20fc-4040-84d6-9f5186d344eb)

#### 表示グループをリージョンでカテゴライズ

![Screenshot 2023-05-16 at 15 38 36](https://github.com/taijihagino/datadog/assets/12064399/67d1dedc-129a-4f74-a3bd-dbe1113864da)

#### 表示されているホストのAWSを見てみる

![Screenshot 2023-05-16 at 15 39 27](https://github.com/taijihagino/datadog/assets/12064399/3415e07d-f8c6-4ff9-bd4c-6764db9c1dac)


### アプリケーションパフォーマンスモニタリング（APM）

#### サービスマップの確認

![Screenshot 2023-05-16 at 15 42 13](https://github.com/taijihagino/datadog/assets/12064399/a5c4de0d-7313-4b57-8f6c-e8b333af96c6)

#### 特定のサービスにフォーカスし依存関係を確認

![Screenshot 2023-05-16 at 15 44 29](https://github.com/taijihagino/datadog/assets/12064399/d4b04d5b-da3c-4f03-9c05-f2f5010da66a)

#### サービスをドリルダウン

![Screenshot 2023-05-16 at 15 45 38](https://github.com/taijihagino/datadog/assets/12064399/ba5c55e2-30d1-4e69-8985-2dc6760a71c0)

#### 障害が発生した場合に影響を受けるサービスと与えるサービスを確認

![Screenshot 2023-05-16 at 15 46 27](https://github.com/taijihagino/datadog/assets/12064399/241fdda7-3f52-45e7-8279-8b3a29b26688)

#### サービスのオーバービューへの遷移

![Screenshot 2023-05-16 at 15 49 53 copy 3](https://github.com/taijihagino/datadog/assets/12064399/2a825600-f414-48da-ade6-6d447442a578)


#### サービスの関連トレース情報への遷移

![Screenshot 2023-05-16 at 15 49 53](https://github.com/taijihagino/datadog/assets/12064399/d310ed6c-2936-4222-b27a-16fb79b838f4)


#### サービスの関連ログ情報への遷移

![Screenshot 2023-05-16 at 15 49 53 copy](https://github.com/taijihagino/datadog/assets/12064399/e327120e-8c1a-472e-8f23-50e373da63da)


#### サービスの関連プロセス情報への遷移

![Screenshot 2023-05-16 at 15 49 53 copy 2](https://github.com/taijihagino/datadog/assets/12064399/56a4dcdd-9080-46d5-b97e-f9649dfe3418)


### リアルユーザーモニタリング（RUM）

#### セッション＆リプレイ
![Screenshot 2023-05-16 at 15 55 50](https://github.com/taijihagino/datadog/assets/12064399/9f6aa06e-3ada-4c72-9ded-f6f4143f7e4e)

#### シンセティックテスト

![Screenshot 2023-05-16 at 15 57 31](https://github.com/taijihagino/datadog/assets/12064399/926b819e-7ede-460f-9263-0c8d51e0c74c)


### モニター

#### 新しいモニターの作成

![Screenshot 2023-05-16 at 16 03 06](https://github.com/taijihagino/datadog/assets/12064399/5014f9b5-3167-4099-824b-033d04bad77c)

![Screenshot 2023-05-16 at 16 04 40](https://github.com/taijihagino/datadog/assets/12064399/14e398fc-fb30-45ea-85de-36861378f084)

![Screenshot 2023-05-16 at 16 05 30](https://github.com/taijihagino/datadog/assets/12064399/d739879f-b73e-4da7-8855-8b13a9b11305)

![Screenshot 2023-05-16 at 16 05 49](https://github.com/taijihagino/datadog/assets/12064399/fb7242db-eafa-4175-8478-cf2214e9f640)


## BIとしてのDatadog活用
ここまでで、Datadogがログ、トレース、メトリクスの各データを統合することで、単なるモニタリングツールではなく統合的なObservabilityツールとして効率的に使えることを体感いただけたと思います。

ここからは、Datadogのもう一つの側面の使い方として、監視対象のWebアプリケーションの利用実態がビジネスKPIに対してどのような実態を示すのかをを可視化する、いわゆるBIツール的な使い方を見ていきましょう。

### RUMのインスツルメント
AWSであれば、分析できる機能としてCloudWatchが使いやすいかもしれないですが、Datadog RUMを使えばインフラ監視、アプリ性能モニタ、ユーザインタラクション（相互作用、交互作用）をひとつのツールでそれぞれのメトリクスを関連付けて可視化と分析ができます。

ここでは、前のセクションで各自のAWS環境（Cloud9）へデプロイしたLINE APIを使った会員証アプリに、RUMを仕込んで行きましょう。

［UX Monitoring］→［Application Summary］→［+ New Application］
![Screenshot 2023-05-16 at 17 22 31](https://github.com/taijihagino/datadog/assets/12064399/af2d38b6-dd3b-45e7-b8ae-bd8aa2b77543)

![Screenshot 2023-05-16 at 17 24 31](https://github.com/taijihagino/datadog/assets/12064399/83573ee6-a342-4714-a6dc-cf418927a5a9)

![Screenshot 2023-05-16 at 17 26 27](https://github.com/taijihagino/datadog/assets/12064399/be74b8b6-7517-45b1-90fa-0a61bd73ca5e)

RUM Application Summaryで、対象のアプリケーションを選択し、Performance Monitoringを選択すると、対象のアプリの各テレメトリデータが可視化されて表示されていることが確認できます。
<img width="1479" alt="Screenshot 2023-05-27 at 20 10 27" src="https://github.com/taijihagino/datadog/assets/12064399/9dbd9ae2-9ea0-4728-8b41-87625e3d06a5">

### Dashbordの作成
Dashboardメニューから「New Dashboard」を選択します。
<img width="1475" alt="Screenshot 2023-05-27 at 20 19 32" src="https://github.com/taijihagino/datadog/assets/12064399/c6ab73fe-4ed2-4766-bf8f-0f4ebc8551a8">

Dashboadの名前を付けて「New Dashboard」をクリックします。
<img width="803" alt="Screenshot 2023-05-27 at 20 21 43" src="https://github.com/taijihagino/datadog/assets/12064399/54f003fd-26db-4733-a1e4-db1e17f032df">

### ウィジェットの追加


### KPIの設定


### 完成








