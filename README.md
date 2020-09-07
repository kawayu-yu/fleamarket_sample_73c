# README

## 概要
TECH:CAMPで、チーム開発(アジャイル開発)を行い作成したフリマアプリです。
  ・トップページ
  ![トップページ](https://user-images.githubusercontent.com/63997771/92347533-574fc000-f10b-11ea-8d71-dd1d1bbaf7e0.png)

## URL
http://54.168.66.180/

■ Basic認証
ID：admin
Pass：73c

■ テスト用アカウント
◯ 購入者用  
メールアドレス：ssss@zzz.gmail.com  
パスワード：05161212s
◯ 購入用カード情報  
番号：4242424242424242  
期限：01/24  
セキュリティカード：111
◯ 出品者用  
メールアドレス：aaaa@yyy.gmail.com  
パスワード：12121985a

## 担当箇所
■ DB設計

■ 商品詳細ページ・商品詳細表示
![詳細ページ](https://user-images.githubusercontent.com/63997771/92351195-f5488800-f115-11ea-88b5-c4370d2c4968.gif)

■ 商品についての質問・コメント機能
![コメント機能](https://user-images.githubusercontent.com/63997771/92351258-21fc9f80-f116-11ea-909e-25db635777d2.gif)

■ 商品情報編集
![商品編集機能](https://user-images.githubusercontent.com/63997771/92351546-f4fcbc80-f116-11ea-95bf-470885a109e9.gif)

## 詳細
https://docs.google.com/document/d/1GI6LzkQjPPER8o1e_rCwHclvVKQMP-gwvEq9Rr6gp-A/edit#heading=h.uc9r1bu1xm1f


## DB設計
## usersテーブル
|Column|Type|Options|
|------|----|-------|
|nickname|string|null:false|
|email|string|null:false, unique:true|
|encrypted_password|string|null:false|

### Association
- has_one:plofile
- has_one: destination
- has_one:credit_card
- has_many:products
- has_many: comments
- has_many:likes
- has_many:to-do_lists
- has_many:notices

## profilesテーブル
|Column|Type|Options|
|------|----|-------|
|first_name|string|null:false|
|family_name|string|null:false|
|first_name_kana|string|null:false|
|family_name_kana|string|null:false|
|birth_year|integer|null:false|
|birth_month|integer|null:false|
|birth_day|integer|null:false|
|user|refrences|null:false,foreign_key:true|

### Association
- belongs_to:user

## destinationsテーブル
|Column|Type|Options|
|------|----|-------|
|destination_first_name|string|null:false|
|destination_family_name|string|null:false|
|destination_first_name_kana|string|null:false|
|destination_family_name_kana|string|null:false|
|postal_code|string(7)|null:false|
|prefecture|string|null:false|
|city|string|null:false|
|address|string|null:false|
|building|string|
|phone_number|string|
|user|references|null:false,foreigin_key:true|

### Association
- belongs_to:user

## productsテーブル
|Column|Type|Options|
|------|----|-------|
|title|string|null:false|
|description|text|null:false|
|postage|integer|null:false|
|condition|integer|null:false,default:0|
|price|integer|null:false|
|status|integer|null:false,default:0|
|user|references|null:false,foreign_key:true|
|comment|references| null:false,foreign_key:true|
|brand|references|foreign_key:true|
|image|references|null:false,foreign_key:true|
|like|references|foreign_key:true|
|category|references|null:false,foreign_key:true|
|prefecture_id|integer|null:false|
|sipping_day_id|integer|null:false|
|size|references|null:false,foreign_key:true|
|buyer_id|integer|

### enum
- enum condition:{brand_new: 1, look_brand_new: 2, no_noticeable_scratches: 3, some_scratches: 4, noticeable_scratches: 5, bad_condition: 6}
- enum postage: {including_shipping_fee: 1, cash_on_delivery: 2}
- enum status: {on_display: 0, sold: 1}
### Association
- belongs_to: user
- belongs_to: brand
- belongs_to: category
- belongs_to: size, optional: true
- has_many: images,dependent::destroy
- has_many: commemts,dependent::destroy
- has_many: likes,dependent::destroy
- belongs_to: active_hash: shipping_day
- belongs_to:active_hash: prefecture

## credit_cardsテーブル
|Column|Type|Options|
|------|----|-------|
|user|references|null:false,foreign_key:true|
|card_id|integer|null:false|
|customer_id|integer|null:false|

### Association
- belongs_to:user

## noticesテーブル
|Column|Type|Options|
|------|----|-------|
|list|text|null:false|
|user|references|null:false, foreign_key:true|

### Association
- belongs_to:user

## categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null:false|
|ancestry|string|

### Asociation
- has_many :products
- has_many :category_sizes
- has_many :sizes, through: :category_sizes
- has_ancestry

## sizesテーブル
|Column|Type|Options|
|------|----|-------|
|size|string|null:false|
|ancestry|string|

### Asociation
- has_many :products
- has_many :category_sizes
- has_many :categories, through: :category_sizes
- has_ancestry

## category_sizes テーブル
|Column|Type|Options|
|------|----|-------|
|category|references|foreign_key:true|
|size|references|foreign_key:true|

### Asociation
- belongs_to :category
- belongs_to :size

## brandsテーブル
|Column|Type|Options|
|------|----|-------|
|brand_name|string|null:false|

### Asociation
- has_many :products

## commentsテーブル
|Column|Type|Options|
|------|----|-------|
|user|references|null:false, foreign_key: true|
|product|refrences|null:false, foreign_key:true|
|comment|text|null:false|

### Asociation
- belongs_to :user
- belongs_to :product

## likesテーブル
|Column|Type|Options|
|------|----|-------|
|user|references|null:false,foreign_key: true|
|product|references|null:false, foreign_key: true|

### Asociation
- belongs_to :user
- belongs_to :product

## imagesテーブル
|Column|Type|Options|
|------|----|-------|
|src|string|null:false|
|product|references|null:false, foreign_key: true|

### Asociation
- belongs_to :product

## to-do_listsテーブル
|Column|Type|Options|
|------|----|-------|
|list|text|null:false|
|user|references|null:false, foreign_key: true|

### Asociation
- belongs_to :user