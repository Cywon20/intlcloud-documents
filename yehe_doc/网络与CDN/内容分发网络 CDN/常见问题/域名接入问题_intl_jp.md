[](id:q1)
###ドメイン名にアクセスする方法とは。
CDNのコンソールでドメイン名にアクセスできます。詳細については、[ドメイン名アクセス](https://intl.cloud.tencent.com/document/product/228/5734)をご参照ください。

[](id:q2)
### CDNのドメイン名にアクセスするための要件は何ですか。
CDNでアクセラレーションを行うドメイン名にアクセスする場合、次の要件があります。
1. ドメイン名の長さは81文字以下とします。
2. 中国本土のドメイン名は工信部でのICP登録済みとします。
3. ドメイン名は`a.test.com`、`a.b.test.com`などの形式のサブドメイン名、または`*.test.com`、`*.a.test.com`の形式の汎用ドメイン名とします。
4. アクセスするドメイン名が汎用ドメイン名か、または他のユーザーからすでにアクセスされている場合、もしくは新しいドメイン名に初めてアクセスする場合は、ドメイン名所有権の検証が必要です。

[](id:q3)
### CDNは汎用ドメイン名アクセスをサポートしていますか。
CDNはすでに汎用ドメイン名アクセスをサポートしていますが、ドメイン名所有権の確認を行う必要があり、検証にパスしてからドメイン名にアクセスまたはドメイン名を取得することができます。
その他：
1. 汎用ドメイン名（例：`*.test.com`）がTencent Cloudにすでにアクセスしている場合、その汎用ドメイン名のサブドメイン名はいずれも、他のアカウントからアクセスできません。
2. 汎用ドメイン名 `*.test.com`にすでにアクセスしている場合、そのアカウントにおいて`*.path.test.com`などの形式の汎用ドメイン名にアクセスすることはできません。

[](id:q4)
### CDNの設定にはどれくらい時間がかかりますか。
通常、CDNの設定時間は30分以内です。設定後の待ち時間が30分以上の場合は、[チケットをサブミット]（https://console.cloud.tencent.com/workorder/category）で問い合わせして処理を依頼してください。

[](id:q5)
###オリジンサーバーIPは複数設定できますか。
複数のオリジンサーバーIPを設定できます。複数のIPを設定している場合、CDNはback to originリクエストを受信したときに、入力したIPの任意1つにランダムにアクセスします。あるIPのback to origin失敗回数がしきい値を超えた場合、デフォルトで当該IPは300秒間隔離され、オリジンサーバーにback to originしません。

[](id:q6)
###ドメイン名がCDNにアクセスされた後、CNAMEをバインディングするにはどうすればよいですか。
[CNAMEの設定](https://intl.cloud.tencent.com/document/product/228/3121) ドキュメントに記載されている操作説明を参照し、DNSプロバイダーでCNAMEをバインディングしてください。

[](id:q7)
###ドメイン名は無効にできるが、削除できないのはなぜですか。
ユーザーが共同編集者であるかどうかを確認してください。共同編集者の操作権限は、CDNサービスの作成者によって設定されます。作成者が対応の権限をアサインしていない場合、共同編集者は操作できません。権限を取得したことを確定しているが関連の操作ができない場合は、[チケットをサブミット]（https://console.cloud.tencent.com/workorder/category）でメンテナンス担当者に処理を依頼できます。

[](id:q8)
###加速サービスが無効になった後、ドメイン名の設定は保持されますか。
無効にした後のドメイン名設定は保留されますがアクセラレーションサービスはその後提供されなくなります。この場合、ユーザーリクエストには404のステータスコードが返ってきます。

[](id:q9)
###加速ドメイン名を削除した後、ドメイン名の設定は保持されますか。
ドメイン名が削除されると、その設定は保持されません。

[](id:q10)
###加速サービスを無効にするにはどうすればよいですか。
CDNのコンソールで加速ドメイン名を無効にすることができます。詳細については、[加速ドメイン名を無効にする](https://intl.cloud.tencent.com/document/product/228/5736)をご参照ください。

[](id:q11)
###加速ドメイン名を削除するにはどうすればよいですか。
CDNのコンソールで加速ドメイン名を削除できます。詳細については、加速ドメイン名を無効にする](https://intl.cloud.tencent.com/document/product/228/5736)をご参照ください。

[](id:q12)
###ドメイン名がブロックされた場合、解除するにはどうすればよいですか。
[チケットをサブミット](https://console.cloud.tencent.com/workorder/category)でメンテナンス担当者にご依頼ください。

[](id:q13)
### CDNがサポートしている業務タイプはどのようなものがあるでしょうか。
業務タイプの選択によって、ドメイン名でスケジューリングするリソースプラットフォームが決まります。リソースプラットフォームのアクセラレーション設定の違いによりある程度の差異が生じます。お客様の業務にフィットする業務タイプを選択してください。
- 静的加速：Eコマース、ウェブサイト、ゲーム画像など静的リソースの加速に適しています。
- ウンロードの加速：ゲームのインストールパッケージ、音声・動画リソースファイルのダウンロード、携帯電話ファームウェアの配布などに適しています。
- ストリーミングメディアVODアクセラレーション：オーディオビデオVODアクセラレーションなどのユースケースに適用します。

[](id:q14)
### CDNドメイン名の所属プロジェクトはどのように修正しますか。

[CDNコンソール](https://console.cloud.tencent.com/cdn)にログインし、左側メニューバーの【ドメイン名管理】を選択して、ドメイン名または操作バーの【管理】をクリックします。Tabの【基本設定】ページで、所属プロジェクトを修正できます。複数のドメイン名の所属プロジェクトを修正したい場合は、【ドメイン名管理】ページで複数のドメイン名を選択し、上側の【更なる操作】で【プロジェクトの編集】を選択すれば、複数のドメイン名の所属プロジェクトを同時に修正可能です（1回につき50件のドメイン名を選択可能）。

>!CDNの権限システムを使用するユーザーは、この操作によりサブユーザーの権限に変更が生じます。慎重に操作してください。


[](id:m1)
### ドメイン名を工信部にてICP登録しているのに、CDNアクセラレーションドメイン名に追加するとドメイン名がICP未登録と表示されます。なぜですか。
ICP登録完了後、通常、工信部の情報が同期され、Tencent Cloud CDNからICP登録情報がプルされるまでには、ある程度の時間を要します。24時間経ってから再度アクセスしてください

[](id:q16)
### アクセラレーションドメイン名/オリジンサーバーではポート設定をサポートしていますか。
- アクセラレーションドメイン名ポート：現在CDNアクセラレーションのポートは、デフォルトで80、443、8080の3つをサポートしています。その他のポートは現在サポートしていません。
- オリジンサーバーポート：オリジンサーバーアドレスの後のポート設定に対応しています。ポート（1 - 65535）を設定可能です。

[](id:q17)
### CDN back-to-origin HOST設定とは何ですか。
back-to-origin HOSTとは、CDNノードのback-to-originのプロセスで、オリジンサーバーにアクセスするサイトのドメインを指します。オリジンサーバーとback-to-origin HOSTでは、オリジンサーバーで設定するIP/ドメイン名によってCDNノードのback-to-origin時に、対応するオリジンサーバーのサーバーをさがし出すことができ、サーバーにはWebサイトが幾つか存在することがありますが、back-to-origin HOSTによってリソースが所在するサイトが示されます。

[](id:q18)
### CDNが有効になったかどうかをどのように判断しますか。

nslookupコマンド（nslookupドメイン名）を使って、CDNアクセラレーションドメイン名のDNSに名前解決を問い合わせます。問い合わせたドメイン名の名前解決の中に、上図の赤枠のような末尾が dnsv1.comのCNAME解決レコードがあれば、アクセスドメイン名のCDNアクセラレーションサービスがすでに有効であることを意味します。
![](https://main.qcloudimg.com/raw/4576b46fd8a04b726e6893a08f3fe61f.png)


[](id:q19)
### CDNファイルがダウンロードできません

ファイルをダウンロードできない場合は、次のいくつかの方向から解決することを推奨します。
1. オリジンサーバーが正常にダウンロードできるかチェックします。
2. CDNドメイン名が正しく設定されているかチェックします。CDNコンソール > 基本設定 > back-to-origin hostとチェックし、設定したback-to-origin hostドメイン名がアクセスできる状態になっていることを確認してください。そうなっていない場合、back-to-originが失敗し、お客様の業務に影響します。
3. オリジンサーバーのセキュリティポリシーをチェックします。オリジンサーバーにセキュリティポリシーが設定されているためにback-to-originが失敗していないか確認し、そうであった場合は、[お問い合わせ](https://intl.cloud.tencent.com/support)ください。back-to-origin IPのネットワークセグメントを取得して、オリジンサーバーのホワイトリスト登録を処理します。

[](id:q20)
### wordpressでCDNのアクセラレーションを設定すると、バックエンドでログインできません。
wordpressは一部の動的リクエストに関係します。キャッシュ設定が適切でないためにログインエラーが生じている場合は、該当する動的ファイルタイプのキャッシュ時間を0に設定することをお勧めします。こうすればこのタイプのファイルは自動キャッシュしなくなります。一般的な動的ファイルには、asp 、.jsp、.php、.perl、.cgiなどがあります。具体的な操作については、[ノードキャッシュ設定](https://intl.cloud.tencent.com/document/product/228/35317)をご参照ください。


