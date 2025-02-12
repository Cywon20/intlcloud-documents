## ユースケース

このドキュメントは、カスタマイズイメージの削除について説明します。

## 注意事項
削除操作を実行する前に、以下の事項に注意してください：
 - カスタマイズイメージを削除した後、このイメージを利用してインスタンスを作成することはできませんが、起動したインスタンスには影響しません。このイメージを利用して起動したすべてのインスタンスを削除する必要がある場合、[インスタンスの回収](https://intl.cloud.tencent.com/document/product/213/4931)  或は[インスタンスの廃棄/リターン](https://intl.cloud.tencent.com/document/product/213/4930)をご参考ください。
 - 共有したイメージを削除できません、削除する前にすべての共有を取り消しする必要があります。 共有イメージの取り消しについて、[カスタマイズイメージの共有の取り消し](https://intl.cloud.tencent.com/document/product/213/7148)をご参照ください。
 - カスタマイズイメージのみ削除できます。パブリックイメージと共有イメージを自主的に削除できません、取得したイメージも削除できません。

## 操作手順
<dx-tabs>
::: コンソールを利用して削除する
1. CVMコンソールにログインします 、左側ナビゲーションバーで、[**イメージ**](https://console.cloud.tencent.com/cvm/)をクリックし
2. **カスタマイズイメージ**を選択して、カスタマイズイメージの管理画面に入ります。
3. 実際のニーズに応じて、カスタマイズイメージを削除するための操作方式を選択します。
 - **単一のイメージの削除**：リストで削除対象のカスタマイズイメージを検索して、**その他**>**削除**をクリックします。
  ![](https://qcloudimg.tencent-cloud.cn/raw/1211de56434b4b6034dccb1ff9ce4aa1.png)
 - **複数のイメージの削除**：リストですべての削除対象のカスタマイズイメージを選択し、トップの**削除**をクリックします。
 ![](https://qcloudimg.tencent-cloud.cn/raw/9f002d5cce1cfa2c94931ea3bca25ac2.png)
4. 削除を確認した後、選択したすべてのイメージを削除できます。削除できない場合、理由が表示されます。
  
:::
::: APIを利用して删除する
ユーザーはDeleteImagesインターフェースを使用してイメージを削除します。詳細については、[イメージの削除](https://intl.cloud.tencent.com/document/product/213/33275)をご参照ください。
:::
</dx-tabs>
