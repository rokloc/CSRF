# CSRFとは
ユーザーがログイン済みの状態で、攻撃者のサイトから意図しないリクエストが送られてしまうという脆弱性。

# 攻撃者目線

# 対策の心得


# CSRF対策具体例（Spring Boot + Spring Security）
## (1) CSRFトークンをセッションに紐づけ（SpringSecurityではデフォルトでON）
POSTリクエストフォームにCSRFトークンを紐づけ
<img width="795" height="197" alt="スクリーンショット (16)" src="https://github.com/user-attachments/assets/c40e6b96-dfd6-4130-b4b4-09428dc024ea" />
SpringSecurityではCSRF対策がデフォルトでオンになっている
そのためPOSTリクエストには上記のCSRFトークンを紐づけする必要がある。
結果としてトークンを知らない他のサイトからのPOSTリクエストを防げる。

※CSRFトークンはPOSTリクエストのみに紐づけされる
- 対策しないとGETに対してCSRFが通ってしまう

※CSRFトークンだけでは不十分なケース
- GETリクエストにはCSRFトークンが使われない（副作用があるGETは避けるべき）
- CORS設定が甘いと、外部サイトからのリクエストが通ってしまうこともある
- セッション固定攻撃（Session Fixation）など、他の脆弱性と組み合わされると突破される可能性もある

## (2) SameSite Cookie属性の設定
外部サイトからのリクエストに、Cookieを付与させないように設定
<img width="401" height="60" alt="スクリーンショット (17)" src="https://github.com/user-attachments/assets/c3be6cc7-c379-48ae-b729-4df28f7286ab" />

※XSSでクッキーが盗まれると、SameSite=Strictはそもそも無意味になるためクッキーが盗まれないための脆弱性対策と連携が必須
