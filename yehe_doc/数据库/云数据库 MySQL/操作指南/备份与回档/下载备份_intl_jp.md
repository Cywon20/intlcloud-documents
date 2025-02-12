TencentDB for MySQLコンソールでは、バックアップファイルのリストが提供され、バックアップファイルをダウンロードすることができます。ダウンロードしたバックアップファイルを使用して、データベースを自分で作成したデータベースなどの他のデータベースに復元できます。

このドキュメントでは、コンソールでバックアップをダウンロードする方法について説明します。
>!リージョンによってダウンロードルールは異なります。クロスドメインバックアップを有効にすると、選択したバックアップリージョンごとにダウンロードルールを設定する必要があります。

## 操作手順
1. [MySQL コンソール](https://console.cloud.tencent.com/cdb)にログインします。
2. 上側でリージョンを選択し、目的インスタンスを見つけ、インスタンスIDまたは**操作**列の**管理**をクリックすると、インスタンス管理画面が表示されます。
3. インスタンス管理ページで**バックアップ復元**ページを選択し、データバックアップリストまたはログバックアップリストを選択してバックアップファイルを確認できます。
![](https://qcloudimg.tencent-cloud.cn/raw/b3f84a061a2500a9d7fcc103ddc56db1.png)
4. バックアップリストの**操作**列で、**ダウンロード**をクリックして、ダウンロードページにアクセスし、必要なリージョンの下のバックアップファイルを選択します**ダウンロードリンクをコピーし**wgetコマンドを使って高速ダウンロードまたは**ローカルダウンロード**を実行します。
>?
>- ダウンロードアドレスをコピーして、[MySQLが置かれるVPCののCVM（Linuxシステム）](https://www.tencentcloud.com/document/product/213/10517)にログインし、wgetコマンドを使ってプライベートネットワークを介して高速、高効率のダウンロードを行うことをお勧めします。
>- ダウンロードアドレスの有効期間が12時間です。12時間を超えた場合は、新たに生成されたダウンロードアドレスを取得するためにダウンロードページに再アクセスする必要があります。
>- wgetでダウンロードする際に、URLに英語の引用符を追加する必要があります。
>- `wgetコマンド形式：`wget -c 「バックアップファイルのダウンロードアドレス」 -O カスタムファイル名.xb`。
