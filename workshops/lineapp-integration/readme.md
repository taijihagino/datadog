## はじめに
こちらは、<a href="https://zenn.dev/arahabica/books/d4373bd4401d6c" target="_blank" rel="noopener noreferrer">LINE APIを使ったサンプルアプリケーション</a>をDatadogで監視するためのハンズオンワークショップです。

対象のアプリケーションは、LINEミニアプリで会員証発行・入退場管理を行うアプリです。具体的には、ミニアプリでQRコードを発行し、店舗側のQRコードリーダーでそれを読み込むシーンを想定しています。このアプリは、AWS上にデプロイされているものとなります。

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

ここでは **Send AWS Logs to Datadog** と **Enable Cloud Security Posture Management** は 「No」を選択しておいてください。右下の「Launch CloudFormation Template」ボタンをクリックすると、AWSのCloudFormation スタック作成の画面が開きます。
![Screenshot 2023-06-01 at 14 02 23 copy](https://github.com/taijihagino/datadog/assets/12064399/dd2830f9-e293-4ef7-88fc-c38fc29f8d01)

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

［UX Monitoring］→［Real User Monitoring］→［+ New Application］
![Screenshot 2023-07-06 at 10 29 02](https://github.com/taijihagino/datadog/assets/12064399/bc05eb0b-d0da-4c72-b393-2f32ddeade93)

![Screenshot 2023-05-16 at 17 24 31](https://github.com/taijihagino/datadog/assets/12064399/83573ee6-a342-4714-a6dc-cf418927a5a9)

コードスニペットが表示されるので、これを今回は会員証アプリに差し込みます。ここで「dd.env」という環境変数に、対象の環境を識別できる文字列を設定しておくと、対象の環境からDatadogへ送られてくるテレメトリデータにこの値がタグとして付与されるので、Datadog上でデータを整理する際に役に立ちます。
<img width="1479" alt="Screenshot 2023-05-27 at 20 42 13" src="https://github.com/taijihagino/datadog/assets/12064399/1c13e1fc-e34b-41c9-9168-5997cfe91ab2">

#### Datadogのチュートリアル通りの方法
RUMの対象としたいファイルにコードスニペットを記述します。
対象のファイルは、QRコード表示画面と管理画面です。それぞれ以下のように差し込んでください。この方法で実装する場合、コードを埋め込んだページを起点としてトレースが行われることに注意してください。

■QRコード画面

``/line-mini-app-hands-on/frontend/pages/index.vue``
<img width="1446" alt="Screenshot 2023-05-27 at 20 33 23" src="https://github.com/taijihagino/datadog/assets/12064399/912cfa0f-ce54-42aa-85b2-54ed802e65c9">

■管理画面

``/line-mini-app-hands-on/frontend/components/admin/visit-list/VisitList.vue``
<img width="1444" alt="Screenshot 2023-05-27 at 20 36 24" src="https://github.com/taijihagino/datadog/assets/12064399/45139d8b-e3e7-452d-9c60-eb1919ff745a">


#### アレンジバージョン
上位レイヤーとなるアプリケーションのファイルにコードスニペットを記述します。ここでは、デフォルトファイルに、コンポーネントがマウントされたタイミングで呼び出されるコールバック関数として記述しています。
``frontend/layouts/default.vue`` に以下のように入れることで全部のファイルでDDへのデータ送信が行われます。

```
import { onMounted } from 'vue';
import { datadogRum } from '@datadog/browser-rum';

export default {
  setup(){
    onMounted(()=>{
      datadogRum.init({
          applicationId: '039b702e-69e8-4e96-96ba-07740385befc',
          clientToken: 'pub9c0af3b4f5e43f3b97c47185c0e7667a',
          site: 'datadoghq.com',
          service:'taijihagino-line-member-card01',
          env:'lineapp',
          // Specify a version number to identify the deployed version of your application in Datadog 
          // version: '1.0.0',
          sessionSampleRate: 100,
          sessionReplaySampleRate: 20,
          trackUserInteractions: true,
          trackResources: true,
          trackLongTasks: true,
          defaultPrivacyLevel:'mask-user-input'
      });
          
      datadogRum.startSessionReplayRecording();
    });
  }
}
```

<img width="1098" alt="Screenshot 2023-06-19 at 13 25 25" src="https://github.com/taijihagino/datadog/assets/12064399/0dbc874f-21d2-4c7d-accc-b3a76c15304f">

#### 共通部分
``package.json`` へ ``@datadog/browser-rum`` を追加します。

npmの場合： ``npm install @datadog/browser-rum`` <br>
yarnの場合： ``yarn add @datadog/browser-rum`` <br><br>
![Screenshot 2023-06-26 at 19 52 21](https://github.com/taijihagino/datadog/assets/12064399/36b7a215-8c26-490c-8bce-ef440df3bf92)

コードの編集が終えたら、再度 ``yarn deploy (npm run deploy)`` を行います。

デプロイ完了後に会員証アプリを動かすと、データがDatadogへ流れ始めます。 Real User MonitoringからPerformance Monitoringを選択すると、対象のアプリの各テレメトリデータが可視化されて表示されていることが確認できます。
<img width="1479" alt="Screenshot 2023-05-27 at 20 10 27" src="https://github.com/taijihagino/datadog/assets/12064399/9dbd9ae2-9ea0-4728-8b41-87625e3d06a5">

また、アプリケーションの編集画面上の「③Verify your installation」でも連携状態（データの受信状態）が確認できます。
![Screenshot 2023-07-06 at 10 35 32](https://github.com/taijihagino/datadog/assets/12064399/f9db9876-2456-4023-9811-84d440218d75)

### Dashbordの作成
Dashboardメニューから「New Dashboard」を選択します。
<img width="1475" alt="Screenshot 2023-05-27 at 20 19 32" src="https://github.com/taijihagino/datadog/assets/12064399/c6ab73fe-4ed2-4766-bf8f-0f4ebc8551a8">

Dashboadの名前を付けて「New Dashboard」をクリックします。
<img width="803" alt="Screenshot 2023-05-27 at 20 21 43" src="https://github.com/taijihagino/datadog/assets/12064399/54f003fd-26db-4733-a1e4-db1e17f032df">

### ウィジェットの追加
ウィジェットは画面右側から追加できます。たくさんの種類のチャートやグラフが選べますが、今回はスタンダードな折れ線グラフを選んでみましょう。
<img width="1315" alt="Screenshot 2023-05-27 at 20 29 12" src="https://github.com/taijihagino/datadog/assets/12064399/87db1f7f-4f01-45a8-b066-28a53fcdda81">

今回の会員証アプリは非常にシンプルな作りになっているため、RUMでのアクションはほとんど発生しません。（RUMはユーザーの画面操作がメインとなるため）ですので、今回は対象アプリの画面の表示処理に対してのエラー発生数を指標としてチャートを作成したいと思います。
<img width="1365" alt="Screenshot 2023-05-27 at 20 53 22" src="https://github.com/taijihagino/datadog/assets/12064399/64944953-9a67-4b6a-bf16-ff07694e7c66">
<img width="1448" alt="Screenshot 2023-05-27 at 21 04 07" src="https://github.com/taijihagino/datadog/assets/12064399/389f4a89-6932-4fa5-8b6b-4c65d704c092">

### KPIの設定
今回はエラー発生数を指標としたいので、KPIとしてエラー発生件数のしきい値を、マーカーとして設定します。意図的にエラーを発生させるのはちょっと大変なので、ここでは数値は適当な値で良いです。例では`2`を設定しています。
<img width="1435" alt="Screenshot 2023-05-27 at 21 06 02" src="https://github.com/taijihagino/datadog/assets/12064399/f2dec310-ec1b-43b8-93f2-ca0ff2a5e236">

### 完成
最後に、ウィジェット（チャート）の見出しを設定します。もちろん日本語で大丈夫です。設定したら右下の「Save」ボタンをクリックします。
<img width="1439" alt="Screenshot 2023-05-27 at 21 06 49" src="https://github.com/taijihagino/datadog/assets/12064399/1487887a-44f8-4ad0-90c9-f5ad45ea9b8b">

ダッシュボードにウィジェットが追加され、アプリの実行結果のデータが表示されています。
<img width="1319" alt="Screenshot 2023-05-27 at 21 07 53" src="https://github.com/taijihagino/datadog/assets/12064399/fd515c68-9619-4b69-9154-6cbfe89c6424">

拡大するとこんな感じに見えます。
<img width="1465" alt="Screenshot 2023-05-27 at 21 08 19" src="https://github.com/taijihagino/datadog/assets/12064399/dc54b24e-e73d-42c5-9693-5c1cc941faf5">

#### （参考）実際にBIのビューを作った場合
今回は、LINEミニアプリでQRコードを読み取るだけのアプリを対象にしたので、デフォルトで取ってこられるセッション、アクション、ビューの情報などが限られてしまい、非常にシンプルなKPIチャートを作成しました。実際に必要なタグやメトリクスを埋め込んで、KPI管理にほしいデータを意図的に取ってくることにより、よりリッチなBIのビューを作成することができます。以下に、一つの例を画像で掲載しておきます。これは、Datadogが有するデモサイトであるEコマースのWebサイトを対象にしたBIのビューになります。
<img width="1482" alt="Screenshot 2023-05-27 at 21 24 47" src="https://github.com/taijihagino/datadog/assets/12064399/c35ddbdd-f064-47fc-8c94-a0e3e8fa22bb">

Datadogはあくまでプラットフォームですので、どのようなデータを引っ張ってくるか、そのデータをどのように見せるかは、ユーザー次第で色々できます。ぜひ、これを機にDatadogの各機能を試してみてください。
以上です。おつかれさまでした！




