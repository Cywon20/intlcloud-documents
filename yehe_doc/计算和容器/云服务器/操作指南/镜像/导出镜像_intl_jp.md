## 操作シナリオ
Tencent Cloudは作成済みのカスタムイメージの[COS](https://intl.cloud.tencent.com/document/product/436/6222)バケット内へのエクスポートをサポートしています。この機能によって必要なイメージをエクスポートすることができます。

## 前提条件
- 現在、この機能を使用するには申請が必要です。[チケットを提出](https://console.intl.cloud.tencent.com/workorder/category)して、機能のアクティブ化を申請してください。
- [COSコンソール](https://console.cloud.tencent.com/cos)ですでにCOSサービスをアクティブ化していること。
- カスタムイメージの存在するリージョンでバケットを作成済みであること。詳細については、[バケットの作成](https://intl.cloud.tencent.com/document/product/436/13309)をご参照ください。


## 注意事項
- 現時点ではWindowsカスタムイメージのエクスポートはサポートしていません。
- カスタムイメージのシステムディスクおよびデータディスク1個あたりの容量は500GB以下でなければなりません。
- フルマシンのイメージをエクスポートする場合、データディスクは5個以下でなければなりません。


## 料金説明[](id:feeDescription)
- CVMをご使用中に、例えばCOSなどの他の製品を同時に使用している場合は、実際に使用している製品の課金ルールに従って料金を計算します。
- 料金説明は下表に示すとおりです。
<table>
<tr>
<th width="19%">シナリオ</th>
<th>発生料金</th>
<th width="12%">説明ドキュメント</th>
</tr>
<tr>
<td rowspan=3>イメージをCOSバケットにエクスポートします</td>
<td><b>ストレージ容量料金</b>。イメージをCOSバケットに保存すると、ストレージ容量料金が発生します。COSはオブジェクトサイズを計算し、目標オブジェクトのストレージタイプおよび所属リージョンに応じて課金します。
</td>
<td><a href="https://intl.cloud.tencent.com/document/product/436/40099">ストレージ容量料金</a></td>
</tr>
<tr>
<td><b>リクエスト料金</b>。イメージをCOSバケットにエクスポートすると、書き込みリクエストが発生します。COSは書き込みリクエストの回数を計算し，リクエスト料金が課金されます。</td>
<td><a href="https://intl.cloud.tencent.com/document/product/436/40100">リクエスト料金</a></td>
</tr>
<tr>
<td><b>トラフィック料金</b>。イメージをCOSバケットにエクスポートすると、アップストリームトラフィックが発生します。COSはトラフィックサイズを計算します。プライベートネットワークアップストリームトラフィック、パブリックネットワークアップストリームトラフィックはどちらも無料です。</td>
<td><a href="https://intl.cloud.tencent.com/document/product/436/33776">トラフィック料金</a></td>
</tr>
<tr>
<td rowspan=2>COSバケットからイメージをダウンロードします </td>
<td><b>リクエスト料金</b>。COSバケットからイメージをダウンロードすると、書き込みリクエストが発生します。COSは書き込みリクエストの回数を計算し，リクエスト料金が課金されます。</td>
<td><a href="https://intl.cloud.tencent.com/document/product/436/40100">リクエスト料金</a></td>
</tr>
<tr>
<td><b>トラフィック料金</b>。COSバケットからイメージをダウンロードすると、ダウンストリームトラフィックが発生します。COSはトラフィックサイズを計算します。プライベートネットワークダウンストリームトラフィックは無料ですが、パブリックネットワークダウンストリームトラフィックは有料です。</td>
<td><a href="https://intl.cloud.tencent.com/document/product/436/33776">トラフィック料金</a></td>
</tr>
</table>

## 操作手順
1. CVMコンソールにログインし、左側のナビゲーションバーで**[イメージ](https://console.cloud.tencent.com/cvm/image)**を選択します。
2. 「イメージ」ページの上方で、エクスポートしたいカスタムイメージの所在リージョンを選択し、**カスタムイメージ**タブをクリックします。
3.下図に示すように、 イメージが存在する行の右側の、**その他** > **イメージのエクスポート**を選択します。
![](https://qcloudimg.tencent-cloud.cn/raw/67a493d7ae96d92b514f5c124619821c.png)
4. 下図のように、 ポップアップした「イメージのエクスポート」ウィンドウで次の設定を行います。
![](https://qcloudimg.tencent-cloud.cn/raw/4cf1d41610772605ac5867be12353b5a.png)
 - **COS Bucket**：エクスポートするイメージを配置するバケットを選択します。イメージと同一のリージョンにある必要があります。
 - **エクスポートファイルのプレフィックス名**：エクスポートファイルのプレフィックス名をカスタマイズします。
 「CVMに私のCOS Bucketへのアクセス権限を付与することに同意します」にチェックを入れます。
5. **OK**をクリックすると、イメージのエクスポートタスクを開始します。
6. ポップアップした確認ウィンドウで、**OK**をクリックします。
エクスポートにかかる時間はイメージのサイズおよびタスクキューのビジー状態の程度によって異なりますので、しばらくお待ちください。エクスポートタスクが完了すると、イメージファイルはターゲットバケット内に保存されます。[バケットリスト](https://console.cloud.tencent.com/cos/bucket)ページに進み、バケットIDをクリックして詳細ページに進むと、`カスタムプレフィックス名_xvda.raw`という名前のファイルがあり、それがエクスポートされたイメージファイルとなります。


## 関連する質問
#### 1. OSのパブリックネットワークダウンストリームトラフィックはどのように発生し、どのように課金されますか。

パブリックネットワークダウンストリームトラフィックとは、インターネットを通じてデータをCOSからクライアントに転送する際に発生するトラフィックを指します。 ユーザーが**オブジェクトリンク**からオブジェクトを直接ダウンロードした場合、または**静的ウェブサイトのオリジンサーバー**を介してオブジェクトを閲覧した場合に発生するトラフィックがパブリックネットワークダウンストリームトラフィックに該当し、それに対応する料金がパブリックネットワークダウンストリームトラフィック料金です。パブリックネットワークダウンストリームトラフィック課金の詳細な情報については、[課金項目](https://www.tencentcloud.com/document/product/436/40096)および[製品価格](https://buy.intl.cloud.tencent.com/price/cos)をご参照ください。

#### 2. COSコンソール、ツール、API、SDKなどの方式でファイルをダウンロードした場合、パブリックネットワークダウンストリームトラフィック料金は発生しますか。

COSへのアクセスによって発生するトラフィック（プライベートネットワークトラフィックまたはパブリックネットワークトラフィック）はご利用の方式に関係なく、同一リージョンのクラウド製品からのCOSアクセスの場合のみ、デフォルトでプライベートネットワークを使用するため、パブリックネットワークダウンストリームトラフィック料金が発生しません。プライベートネットワークアクセスかどうかの判断については、[プライベートネットワークとパブリックネットワークアクセス](https://intl.cloud.tencent.com/document/product/436/30613)のドキュメントをご参照ください。

#### 3. COSはパブリックネットワークトラフィックをどのように区別していますか。

パブリックネットワークダウンストリームトラフィックとは、インターネットを通じてデータをCOSからクライアントに転送する際に発生するトラフィックを指します。例えば、COSに保存されているファイルをCOSコンソール経由でダウンロードする場合、ツールによってオブジェクトへのアクセスやダウンロードを行う場合、またはブラウザを使用してオブジェクトのプレビューを行う場合、オブジェクトアドレスまたはカスタムドメイン名を使用してオブジェクトにアクセスおよびダウンロードを行う場合などは、いずれもパブリックネットワークダウンストリームトラフィックが発生します。詳細については、[プライベートネットワークとパブリックネットワークアクセスの判断](https://intl.cloud.tencent.com/document/product/436/30613)のドキュメントをご参照ください。

#### 4. プライベートネットワークを使用してCOSにアクセスする場合、料金は発生しますか。

プライベートネットワーク経由でCOSにアクセスする場合、**トラフィック料金は無料です**。ただし、**ストレージ容量**と**リクエスト回数**については関連の料金が発生します。詳細な説明については、[課金項目](https://www.tencentcloud.com/document/product/436/40096)をご参照ください。
