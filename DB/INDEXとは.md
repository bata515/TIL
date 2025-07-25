# INDEX(索引)とは

## 概要
INDEXは、テーブルの特定のカラムに対して作成され、データの検索を効率化します。
例えば、図書館で「〇〇さんが書いた本」を探すとき、本棚を1冊ずつ確認していたら時間がかかります。
でも、作者名順の索引（インデックス）があれば、すぐに〇〇さんの本の棚にたどり着けます。
そんなイメージです。

## 具体例
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT,
  created_at TIMESTAMP
);
```
```sql
CREATE INDEX idx_users_email ON users(email);
```
- 上記の例では、`users`テーブルの`email`カラムに対してインデックスを作成しています。
- これにより、`email`カラムを使った検索が高速化されます。
- インデックスは、テーブルのデータをソートして保存するため、検索時に全ての行をスキャンする必要がなくなります。
- 例えば、`SELECT * FROM users WHERE email = '<email>'`というクエリを実行する際、インデックスがあれば、データベースはインデックスを使って迅速に該当する行を見つけることができます。

## INDEXのデメリット
- **書き込み（INSERT / UPDATE / DELETE）速度の低下**: インデックスを作成すると、データの挿入や更新時にインデックスも更新する必要があるため、書き込み速度が低下します。

### INDEXを貼るべき方針
- **検索頻度が高いカラム**: よく検索されるカラムにはインデックスを貼るべきです。例えば、ユーザーIDやメールアドレスなど、検索条件としてよく使われるカラムです。
- **結合に使用されるカラム**: 他のテーブルとの結合に使用されるカラムにもインデックスを貼ることで、結合処理を高速化できます。
- **ソートやフィルタリングに使用されるカラム**: ORDER BYやWHERE句で頻繁に使用されるカラムにもインデックスを貼ると、クエリのパフォーマンスが向上します。

### INDEXを貼るべきでない方針
- **更新頻度が高いカラム**: 頻繁に更新されるカラムにインデックスを貼ると、更新時にインデックスも更新する必要があるため、パフォーマンスが低下します。例えば、ログテーブルのように、頻繁に更新されるデータにはインデックスを貼らない方が良いです。
- **データ量が少ないカラム**: データ量が少ないカラムにインデックスを貼ると、逆にパフォーマンスが低下することがあります。例えば、性別や国コードなど、値の種類が少ないカラムにはインデックスを貼らない方が良いです。
- **複数のカラムに対するインデックス**: 複数のカラムに対してインデックスを貼る場合、特にそのカラムの組み合わせが頻繁に変わる場合は注意が必要です。複数カラムのインデックスは、特定のクエリに対しては効果的ですが、他のクエリに対しては効果が薄くなることがあります。特に、インデックスの更新コストが高くなるため、慎重に選定する必要があります。

## INDEXの種類
- **B-treeインデックス**: データベースで最も一般的なインデックスの形式で、データを木構造で管理します。検索、挿入、削除が効率的に行えます。
- **ハッシュインデックス**: キーと値のペアをハッシュ関数で管理し、高速な検索を実現します。ただし、範囲検索には向いていません。
- **全文検索インデックス**: テキストデータの検索を効率化するためのインデックスで、特に大規模なテキストデータベースで有効です。キーワード検索やフレーズ検索が可能です。
- **GiSTインデックス**: PostgreSQLなどで使用される汎用的なインデックスで、空間データや複雑なデータ型の検索に対応しています。GiSTは「Generalized Search Tree」の略で、様々なデータ型に対応可能です。
- **SP-GiSTインデックス**: GiSTの派生で、特定のデータ型に特化したインデックスです。空間データや非線形データの検索に適しています。
- **BRINインデックス**: 大規模なテーブルに対して、データの物理的な配置に基づいて効率的な検索を提供します。特に、データが連続している場合に効果的です。
- **RTREEインデックス**: 空間データの検索に特化したインデックスで、地理情報システムやGISアプリケーションでよく使用されます。2次元や3次元の空間データを効率的に管理します。

