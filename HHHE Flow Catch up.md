# ➀ HE_不可視項目のデータクリア

`責任区分` field doesn't include `KDDI`
- Set `経験年数` field to `Empty String`
- Set `当事者職務` field to `Empty String`

`HE判定（速報）`doesn't match `HH`
- Set `HH内容` field to `Empty String`
  
`アクション会議開催要否` doesn't match `要`
- Set `最終アクション会議開催日` to `Empty`
- Set `1回目アクション会議の日程` to `Empty`

`追加コスト発生` doesn't match `有り`
- Set `追加コスト内容` to `Empty`

サービス影響 doesn't match `有り`
- Set `サービス影響内容` to `Empty String`

`他社影響` doesn't match `有り`
- Set `他社影響内容` to `Empty String`

`リスケ発生` doesn't match `有り`
- Set `リスケ発生内容` to `Empty String`

# ➁ HEシステムメール通知
 - Get Email Address from `HE_notification_addressee__c` and send mail to that address

# ➂ HE入力者所属情報登録
 - Get `共通_社員マスタ` data with log in user Id and set department level information from `共通_社員マスタ` to HE管理

# ➃ HE報告メール作成・送信
 - Send Mail to Addresses that return from `HEシステムメール通知` flow

# ⑤ HE報告書メール作成
 - Prepare mail body

# ⑥ HE報告メール送信
 - send mail to Address from `HE_notification_addressee_c` object

# ➆ HE担当者所属情報登録
	(When record is create or update)
 - add department levels when record is create or update and `責任部署担当者` is modified

# ➇ HE管理 サービス影響チェック
	(When record is update)
 - if `サービス影響` is `有り` and there is no related `HE関連サービス` record, it will show error message

# ➈ HE管理_項目連動フロー 
	(When record is create or update)
 - When `主な原因` or `主な原因(検討結果)` is changed, update `主な原因` and `主な原因(検討結果)`

# ⑩ HE管理番号追加
	 (When record is create)
 -  Update `管理番号` field

# ⑪ HE関連サービス 削除イベント
	(When record is delete)
 -  Get `HE関連サービス` record with record id
 -  If `HE関連サービス` is more than 1 record, No Service Impact and `進捗ステータス` match `発生中` or `調査中`, it can be deleted . Otherwise it will show error.

# ⑫ HH_コメント登録時メール送信フロー
	(When record is create or update)
 -  When create comment in HH管理 send email to `コメント` create user 
	 （※ QA - When a `コメント` is created, should an email be sent to the user who created the parent HH管理 record, or to the commenter?）
-  Also send to `担当者` field of `HH管理` user when field isn't `null` 
![Pasted image 20240320142316.png](https://github.com/mtm-phyothihakyaw/Salesforce-Notes/blob/main/Pasted%20image%2020240320142316.png)
![Pasted image 20240320142404.png](https://github.com/mtm-phyothihakyaw/Salesforce-Notes/blob/main/Pasted%21image%20240320142404.png)

# ⑬ HH_部署情報連携
	(When record is create or update)
 -  When `担当者` field is updated, Get `共通_社員マスタ` record with `担当者` and update the following fields 
	 - `所属レベル03_格納用`
	 - `所属レべル04_格納用`
	 - `所属レベル05_格納用
	 - `所属レベル06_格納用`
	 - `所属レベル07_格納用`
	 - `会社名_格納用`

# ⑭ HH「いいね!」通知メール送信
