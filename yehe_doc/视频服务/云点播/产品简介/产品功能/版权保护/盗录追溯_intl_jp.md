## 概要

著作権侵害における主な形式が、ビデオの不正録画です。不正録画者はスクリーンキャプチャソフトを利用して録画を行ったり、カメラで直接撮影したりして、不正録画したビデオを拡散します。
不正録画に対する最も有効な方法の一つが、不正録画者を追跡し、他の手段を併用して権益を保護し、不正録画を行う者を震え上がらせ、失われた利益を回収することです。
VODは一般的な画像・テキストウォーターマーク、再生側フローティングウォーターマーク、トレーサビリティウォーターマークをサポートし、不正録画者の追跡を実現します。

<table ><thead ><tr>
<th style="width:150px">機能</th><th >説明</th><th >特徴</th></tr>
</thead><tbody ><tr>
<td>一般的な画像・テキストウォーターマーク</td>
<td>ビデオトランスコードの際にビデオ画面にエンコードする画像またはテキストウォーターマーク</td>
<td>安全性が高く、高コストです。<li>安全性：ウォーターマークは画面にエンコードされているため、セキュリティレベルが高いです</li>
<li>コスト：視聴者ごとに独自のウォーターマークを付ける必要がある場合は多くのトランスコードレプリカが必要となるため、コストが非常に高くなります</li>
</td>
</tr>
<tr>
<td>再生側フローティングウォーターマーク</td>
<td>プレーヤーでの再生の際にビデオレイヤー上に表示されるウォーターマーク</td>
<td>安全性が低く、低コストです。<li>安全性：ウォーターマークは上層のレイヤーにあるため、元のビデオストリームが攻撃者に盗用されるおそれがあり、セキュリティレベルが低いです</li>
<li>コスト：再生側で実装するため、低コストです</li>
</td>
</tr>
<tr>
<td>トレーサビリティウォーターマーク</td>
<td>ウォーターマークがビデオ画面の中にエンコードされ、不正録画ビデオの中からユーザーのIDを取得し、不正録画ユーザーに対する追跡を実現することができます</td>
<td>安全性が高く、低コストです。<li>安全性：ウォーターマークは画面にエンコードされているため、セキュリティレベルが高いです</li>
<li>コスト：2回分のトランスコードとストレージコストのみで済むため、比較的低コストです</li>
</td>
</tr>
</tbody>
</table>



総合的に見て、トレーサビリティウォーターマークか、またはトレーサビリティウォーターマークと再生側フローティングウォーターマークを併用し、不正録画のトレーサビリティと高い安全性および低コストを両立することをお勧めします。

## 適用ケース
<table ><thead ><tr>
<th style="width:150px">ケース</th><th >説明</th></tr>
</thead><tbody ><tr>
<td>eラーニング</td>
<td>教育や研修系のカリキュラムには高額な制作コストが必要なため、不正録画防止のためにトレーサビリティウォーターマークを使用することをお勧めします。また、再生側フローティングウォーターマークの使用も検討可能です。</td>
</tr>
<tr>
<td>機密性の高いビデオ</td>
<td>機密性の高い内部ビデオで、外部への配信が厳禁である場合は、不正録画防止のためにトレーサビリティウォーターマークを使用し、再生側フローティングウォーターマークを併用することをお勧めします。</td>
</tr>
<tr>
<td>著作権のある映画・ドラマ</td>
<td>映画・ドラマがプロフェッショナル版の著作権者から提供されたものの場合は、一般的な画像・テキストウォーターマークの上に著作権者またはプラットフォームのロゴを加え、さらに不正録画防止のためにトレーサビリティウォーターマークを使用することをお勧めします。</td>
</tr>
</tbody>
</table>


## 使用方法

* 一般的な画像・テキストウォーターマークの使用方法に関しては、[リンク](https://intl.cloud.tencent.com/document/product/266/33939)をご参照ください。
* 再生側フローティングウォーターマークの使用方法に関しては、[リンク](https://intl.cloud.tencent.com/document/product/266/33976)をご参照ください。
* トレーサビリティウォーターマークの使用方法に関しては、[リンク](https://intl.cloud.tencent.com/document/product/266/47920)をご参照ください。
