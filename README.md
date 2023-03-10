# ユーザー認証機能

このチュートリアルではユーザー認証機能に関する機能を作っていきます。具体的には次の機能を作成します。

* ユーザー登録機能
* ログイン
* ログアウト
* マイページ（認証が必要なページ）

## 準備

このチュートリアルではデータベースを利用します。
そのため、データベースの基本的な操作について知っている必要があります。
MacではMAMP、WindowsではXAMPPをインストールすることで環境を用意できます。

### データベースを用意する

まずは、データベースを用意しましょう。
`workshop` という名前でデータベースを作成します。sql文で実行する場合は次のsqlを実行します。

```sql
CREATE DATABASE workshop;
```

### テーブルを用意する

データベースが作成できたら、次はテーブルを作成します。
`init.sql` にテーブルを作成するsqlを用意しています。
そのままコピーして実行してもいいですし、次のコマンドでsql文を実行することができます。

```bash
$ mysql -h 127.0.0.1 -u root root workshop < init.sql
```

このチュートリアルでは、次の環境下で実行しています。

* ユーザー名: root
* パスワード: root
* ポート番号: 3306

ユーザー名、パスワード、ポート番号はお使いの環境に合わせて変更してください。


テーブルが作成されていることを確認してみましょう。次のようなテーブル情報が表示されていれば成功です！

```bash
$ mysql -h 127.0.0.1 -u root -proot workshop -e "desc users;"
mysql: [Warning] Using a password on the command line interface can be insecure.
+------------+--------------+------+-----+-------------------+----------------+
| Field      | Type         | Null | Key | Default           | Extra          |
+------------+--------------+------+-----+-------------------+----------------+
| id         | int(11)      | NO   | PRI | NULL              | auto_increment |
| user_name  | varchar(255) | NO   |     | NULL              |                |
| email      | varchar(400) | NO   |     | NULL              |                |
| password   | varchar(255) | NO   |     | NULL              |                |
| created_at | datetime     | NO   |     | CURRENT_TIMESTAMP |                |
| updated_at | datetime     | NO   |     | CURRENT_TIMESTAMP |                |
+------------+--------------+------+-----+-------------------+----------------+
```

```text
補足

次の警告文はパスワードがコマンド上に入力されているため表示されています。

mysql: [Warning] Using a password on the command line interface can be insecure.

このチュートリアルではわかりやすさを優先してパスワードをコマンドに含めています。この警告文を出さないようにするためには -p の後のパスワードを消してください。
```

### データベースの接続先の設定

データベースの接続先を設定します。
`database.php` の次の部分をお使いのデータベースの情報に変更してください。

```php
define('DB_HOST', '127.0.0.1'); //データベースのホスト名
define('DB_NAME', 'workshop');  //データベース名
define('DB_USER', 'root');      //データベースのユーザー名
define('DB_PASSWORD', 'root');  //データベースのパスワード
define('DB_PORT', '3306');      //データベースのポート番号
```

## アプリケーションの起動

MAMPの `htdocs/` の配下に `user-registration`を配置してください。

http://localhost/user-registration にブラウザでアクセスしてみましょう。
ユーザー登録画面が表示されます。

もし、エラーが表示された場合はデータベースの接続情報を確認してみてください。
