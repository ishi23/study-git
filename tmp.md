branch   
- 実際のプロジェクトにおいてはmainブランチで作業すべきではない
- ブランチを切って作業する
- まとまったupdateを終了したらmainに統合する
- ブランチ名は「何の作業をするか」がわかるよう、-区切り
- git branch -m <old-branch-name> <new-branch-name> ブランチ名変更


merge
- リモートリポでマージ：チーム開発でmainリポを更新するケース。GitHubでPull Request
- ローカルリポでマージ：自分のブランチから更にブランチを出していて、マージ先がmainではないケース。git mergeコマンド


git merge <branchname>
現在のブランチに<branchname>をmerge

マージ前に差分確認
git diff <base-branch> <compare-branch>
baseが編集前でcompareが編集後

Fast forwardマージ：
ブランチを切る際のcommitを+-0として、ブランチのコミットを+1とする。この時、merge先が+-0のコミットのままであればそのままマージ（単純な更新）ができる。


Automatic Merge：しかし実際にはmerge先で別のコミットがある場合がある。この場合でも更新が競合しなければmerge可能である。これを自動で判別して問題ないことを判定してmergeすることをAutomatic Mergeという。できない場合をConflictと呼ぶ。Automatic mergeの場合、統合先に存在していた+1と今回統合する+1どちらのコミットにも当たらない新たなコミットが作成されることになる。

Conflict: コンフリクトが生じた場合、git側でconflict commitを発生させる。この時conflictの場所は明示され、その箇所にはアノテーションが入る。その箇所を手動で修正（実際にはあるコミットによる編集を残し、他のコミットの編集を削除する作業）をして再度add&コミットすることで解消される。この操作はmergeではなくて普通のcommit操作であるが、実際にはのmerge commitが作成される。

aaaabbbb