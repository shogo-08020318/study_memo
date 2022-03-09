# セットアップ
**postgresqlにログイン**
```
$ psql -d postgres -U shogotakemura
```
すると、以下のようにSQLを入力する状態になる
```sql
$ psql -d postgres -U shogotakemura
psql (13.4)
Type "help" for help.

postgres=# 
```

# １章
### データベース作成
データベースを作成するコマンド
```sql
# CREATE DATABASE <データベース名>;
```
`shop`という名前のデータベースを作成
```sql
postgres=# CREATE DATABASE shop;
CREATE DATABASE
```
これで作成された。
データベースが作成されたか確認
```sql
$ psql -l
                                            List of databases
           Name            |     Owner     | Encoding | Collate | Ctype |        Access privileges        
---------------------------+---------------+----------+---------+-------+---------------------------------
 
 postgres                  | shogotakemura | UTF8     | C       | C     | 
 shop                      | shogotakemura | UTF8     | C       | C     | 
 template0                 | shogotakemura | UTF8     | C       | C     |

(12 rows)
```

### テーブル作成
テーブルを作成するコマンド
```sql
CREATE TABLE Shohin
(<列名1> <データ型> <この列の制約>,
 <列名2> <データ型> <この列の制約>,
 <列名3> <データ型> <この列の制約>,
 <列名4> <データ型> <この列の制約>,
               ・
               ・
               ・
  <このテーブルの制約1>, <このテーブルの制約2>, ・・・);
```
`Shohin`という名前のテーブルを作成
```sql
postgres=# CREATE TABLE Shohin
(shohin_id     CHAR(4) NOT NULL,
 shohin_mei    VARCHAR(100) NOT NULL,
 shohin_bunrui VARCHAR(32) NOT NULL,
 hanbai_tanka  INTEGER ,
 shiire_tanka  INTEGER ,
 torokubi      DATE ,
     PRIMARY KEY (shohin_id));
CREATE TABLE
```
テーブルが作成されたか確認
```sql
postgres=# \dt;
            List of relations
 Schema |  Name  | Type  |     Owner     
--------+--------+-------+---------------
 public | shohin | table | shogotakemura
(1 row)
```

###  テーブル削除
テーブルを削除するコマンド
```sql
DROP TABLE <テーブル名>;
```
`Shohin`を削除
```sql
DROP TABLE Shohin;
```

### テーブル定義の変更
- テーブルにカラムを追加する場合
```sql
ALTER TABLE <テーブル名> ADD COLUMN <列の定義>;
```
`Shohin`テーブルに`shohin_mei_kana`カラムを追加する
```sql
ALTER TABLE Shohin ADD COLUMN shohin_mei_kana VARCHAR(100);
```
<br>
- テーブルから列を削除する場合
```sql
ALTER TABLE <テーブル名> DROP <列名>;
```
`shohin`テーブルから`shohin_mei_kana`カラムを削除
```sql
ALTER TABLE Shohin DROP (shohin_mei_kana);
```

### テーブルにデータ登録
`Shohin`テーブルにデータを追加する
```sql
BEGIN TRANSACTION;
INSERT INTO Shohin VALUES ('0001', 'Tシャツ', '衣服', 1000, 500, '2009-09-20');
INSERT INTO Shohin VALUES ('0002', '穴あけパンチ', '事務用品', 500, 320, '2009-09-11');
INSERT INTO Shohin VALUES ('0003', 'カッターシャツ', '衣服', 4000, 2800, NULL);
INSERT INTO Shohin VALUES ('0004', '包丁', 'キッチン用品', 3000, 2800, '2009-09-20');
INSERT INTO Shohin VALUES ('0005', '圧力鍋', 'キッチン用品', 6800, 5000, '2009-01-15');
INSERT INTO Shohin VALUES ('0006', 'フォーク', 'キッチン用品', 500, NULL, '2009-09-20');
INSERT INTO Shohin VALUES ('0007', 'おろしがね', 'キッチン用品', 880, 790, '2008-04-28');
INSERT INTO Shohin VALUES ('0008', 'ボールペン', '事務用品', 100, NULL, '2009-11-11');
COMMIT;
```
データが作成されたか確認
```sql
postgres=# select * from Shohin;
 shohin_id |   shohin_mei   | shohin_bunrui | hanbai_tanka | shiire_tanka |  torokubi  
-----------+----------------+---------------+--------------+--------------+------------
 0001      | Tシャツ        | 衣服          |         1000 |          500 | 2009-09-20
 0002      | 穴あけパンチ   | 事務用品      |          500 |          320 | 2009-09-11
 0003      | カッターシャツ | 衣服          |         4000 |         2800 | 
 0004      | 包丁           | キッチン用品  |         3000 |         2800 | 2009-09-20
 0005      | 圧力鍋         | キッチン用品  |         6800 |         5000 | 2009-01-15
 0006      | フォーク       | キッチン用品  |          500 |              | 2009-09-20
 0007      | おろしがね     | キッチン用品  |          880 |          790 | 2008-04-28
 0008      | ボールペン     | 事務用品      |          100 |              | 2009-11-11
(8 rows)
```

**VALUES**
テーブルに登録する値を記述するために使用
```
VALUES(値1, 値2, ...)
```
のようにカンマ区切りで記述する。

**INSERT**
行を追加する命令文

**BEGIN TRANSACTION**
行を追加を開始する命令文

**COMMIT**
行の追加を確定する命令文

### テーブルの修正
既にデータがある状態でテーブル名を修正したいとき
```sql
ALTER TABLE <変更前の名前> RENAME TO <変更後の名前>;
```
`Sohin`を`Shohin`に変更する場合
```sql
ALTER TABLE Sohin RENAME TO Shohin;
```

### カラムの変更
カラム名の変更するとき
```sql
ALTER TABLE <テーブル名> RENAME COLUMN <変更前のカラム名> to <変更後のカラム名>;
```
