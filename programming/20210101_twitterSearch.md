# 楽にtwitterで高機能な検索をしたい

twitterって頑張ればいろいろ高機能なオプション付けて検索が可能なのですが、  
オプションを調べて、それを検索フォームに手入力して・・・とやろうとすると面倒くさいなと。

そこで、お手製の簡易ツール作れば検索が捗るのでは？　と思ったので作ってみました。  
検索したい条件を入力して、「生成」ボタンを押すことで、リンクを自動で生成します。

## 検索ツール

| param           | overview             | example         | input                                                                                                |
| --------------- | -------------------- | --------------- | ---------------------------------------------------------------------------------------------------- |
| @               | ユーザが関連するもの | @r_kuzumi       | <input type="textbox" name="twisearch_id" data-key="@" placeholder="userId"/>                        |
| from            | ユーザが発信したもの | from:r_kuzumi   | <input type="textbox" name="twisearch_from" data-key="from:" placeholder="userId"/>                  |
| to              | ユーザが受信したもの | to:r_kuzumi     | <input type="textbox" name="twisearch_to" data-key="to:" placeholder="userId"/>                      |
| since           | 指定日以降           | since:2021-1-1  | <input type="textbox" name="twisearch_since" data-key="since:" placeholder="2021-1-1"/>              |
| until           | 指定日以前           | since:2021-1-1  | <input type="textbox" name="twisearch_until" data-key="until:" placeholder="2021-1-1_23:00:00_JST"/> |
| filter:media    | 画像,動画を含むもの  | filter:media    | <input type="checkbox" name="twisearch_images" data-key="filter:media"/>                             |
| filter:images   | 画像を含むもの       | filter:images   | <input type="checkbox" name="twisearch_images" data-key="filter:images"/>                            |
| filter:videos   | 動画を含むもの       | filter:videos   | <input type="checkbox" name="twisearch_videos" data-key="filter:videos"/>                            |
| filter:links    | リンクを含むもの     | filter:links    | <input type="checkbox" name="twisearch_links" data-key="filter:links"/>                              |
| filter:verified | 認証アカウント       | filter:verified | <input type="checkbox" name="twisearch_verified" data-key="filter:verified"/>                        |
| filter:follows  | フォロー垢に限定     | filter:follows  | <input type="checkbox" name="twisearch_follows" data-key="filter:follows"/>                          |


<button onclick="generateTwitterSearchUrl();">生成</button>
<div>
<span><a id="linkUrl" target="_brank"></a></span><br>
<textarea id="outputUrl"></textarea>
</div>

---
## 補足
- 一般的なキーワード検索はサポートしていません
  - アクセスした検索結果画面で、検索条件に追加してください
- `since`, `until`は日付のみ、時間指定どちらでもOKです
  - 記法の例として、それぞれ違う書き方をplaceholderにしています
- すべての検索条件はサポートしていません
  - 「この検索条件を追加してほしい」がありましたらリクエストください

### おことわり
本ツールを使用した結果についての責任は負いかねます。  
念のため、生成されたリンクURLを確認いただき、各自の責任のもとでのご使用をお願いします。

### 参考資料

- [Twitter（ツイッター）の検索コマンド全22選　日付やアカウントを指定して探す -Appliv TOPICS](https://mag.app-liv.jp/archive/81735/)
- [【2021年版】Twitterの検索コマンド60種類全まとめ！ユーザー指定や日付指定など使い方を解説 | ヨノイブログ](https://yonoi.com/twitter-search-command/)

<script>
function generateTwitterSearchUrl(){
    var baseUrl = 'https://twitter.com/search?q=';

    var queryConditions = [];
    var conditions = document.querySelectorAll('input[name^=twisearch_]');
    conditions.forEach(
        function(currentValue) {
            if(currentValue.type=='text' && currentValue.value != '') {
                var tmp = currentValue.dataset.key + currentValue.value;
                queryConditions.push(tmp);
            } else if(currentValue.type=='checkbox' && currentValue.checked) {
                queryConditions.push(currentValue.dataset.key);
            }
        }
    );
    console.log(queryConditions);


    var searchUrl = baseUrl + queryConditions.join('%20');
    document.getElementById('linkUrl').href = searchUrl;
    document.getElementById('linkUrl').innerText = '指定した条件（下記URL）で検索する';
    document.getElementById('outputUrl').value = searchUrl;
}
</script>