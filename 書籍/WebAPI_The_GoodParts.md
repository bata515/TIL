# Web API The Good Parts を読んで学んだこと

## 1章　Web APIとは何か

### Web APIの重要性
自分たちで作成した機能やでーたをAPIとして公開するということは、近視眼的に見れば損しているようにも見えるが、そのAPIから新しいシステム、サービスを公開する力を持った開発者に公開することでサービスに付加価値を与えてくれて自分たちのサービスが発展する力をもらうこともできる。

拡張機能やアフィリエイト広告などもAPIを使ったサービスの横展開で、例えばAmazonnの商品リンクをアプリやブログに組み込むことで、自分でECサイトの構築や在庫の用意はしなくても、商品を販売し一部の利益がもらえるようになる。
またAmazonは自社サービスの発展につながる。

### 2章 エンドポイントの設計とリクエストの形式

### エンドポイントの基本的な設計
1. 短く入力しやすいURI
    同じ単語や表現はなくし、理解しやすく、短く、入力しやすいURIを心がける。
2. 人間が呼んで理解できるURI
    略語などは使用せずに、誰が見ても理解できるURIを心がける。和製英語やローマ字表記も避け全世界共通の正しい英語を使用してURIを表現するのが好ましい。
3. 大文字、小文字が混在していないURI
4. 改造しやすい(Hackable)URI
5. サーバー側のアーキテクチャが反映されていないURI
6. ルールが統一されたURI

### X-HTTP-Method-Override ヘッダ
HTTPメソッドを上書きするためのヘッダで、POSTリクエストに対してPUTやDELETEなどのメソッドを指定することができる。
HTMLフォームではGETとPOSTしか使用できないため、PUTやDELETEを使用したい場合にこのヘッダを使用する。
#### 例
```http
POST /api/resource HTTP/1.1
Host: example.com
Content-Type: application/json
{
  "name": "example"
}
X-HTTP-Method-Override: PUT
```

クライアント側ソースコードでの指定方法例
JavaScript（Fetch API）の場合：
```javascript
fetch('/api/resource', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-HTTP-Method-Override': 'PUT'
  },
  body: JSON.stringify({
    name: 'example'
  })
  });
```
まとめ
- クライアント開発時に使用している一部のライブラリ、HTMLフォームはGET、POSTのみしか対応していない場合があり、 PUT や DELETE を直接送れないことがある

- サーバー側で X-HTTP-Method-Override を検知して、元のリクエストメソッドを「上書き」する
