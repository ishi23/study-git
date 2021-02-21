# 日常的に利用するgitコマンドやgithubの使い方

### .gitignoreファイル
- git管理すべきでないファイルが多く存在する
- 特にパスワードを含むファイルやログファイル等
- .gitignoreファイルにgit管理対象外のファイルを書いておく
- ワイルドカードやフォルダ指定も可能(*.tif, dirname/)
- https://github.com/github/gitignore に通常どういったファイルをignoreすべきか（.gitignoreのサンプルファイル）がたくさんある。
- 例えばpython用の例だと.ipynb_checkpointsなんかも含まれる
- ここに指定されたファイルやフォルダはローカルリポにしか存在しない事になる。編集してもchanged statusにもならない。
- 一度git管理を開始したファイルは後で.gitignoreに書いてもキャッシュにより管理が続いてしまうようである。下記コマンドでキャッシュを削除して有効化できる。
    - `git rm -r --cached .` //ファイル全体キャッシュ削除
    - `git rm -r --cached [ファイル名]`  //ファイル指定してキャッシュ削除


### `git init`  
- ローカルリポジトリの作成（.gitが作成される)
- `git init <repo-name>` でrepo-nameのリポジトリを新規に作成
- `git init` を既存のフォルダ内で実行することで既存フォルダをローカルリポジトリ化

### `git clone <url>`
- リモートリポジトリをコピーしてローカルリポジトリを作成
- urlはhttpでもいいがssh登録していればssh
- GitHub上のリポジトリの”Code”からコピーしてこれる


### `git status`
- 管理下のファイルの状態を確認
- 前回のコミットからどのファイルに変更があったか、どんな変更か、どのファイルの変更がステージングされているか　等。
- oh-my-zsh + powerlevel10k の環境ではstatus概要はヘッダーから確認できるため減らせる

### `git log`
- commitのログを表示
- commitIDやタグ、コメントも表示。
- --oneline オプションで簡潔に
- --all オプションで全ブランチのログ
- --graphオプションで各コミットを線で繋ぐ

### `git remote -v`
- ローカルリポジトリに紐づいているリモートリポジトリを確認

### `git add <filename>`
- working director　-> staging area
- `git add .` でリポジトリ内の全てのファイルをadd


### `git commit -m "comment"`
- staging area -> repository
- コメントは必ずつける

### `git pull <remote_ref> <branch_name>` 
- ローカルリポジトリを最新状態にする
- 複数人開発の場合はpushする前には必ず行うべき
- remote_refは通常"origin" (git remote -v で確認)

### `git push <remote_ref> <branch_name>`
- ローカルリポジトリのコミットをリモートリポジトリに反映
- ブランチ名を間違えないように注意
- ローカルで作ったブランチでリモートに存在しない場合、リモートに新しいブランチが作成される
- 開発ブランチのプッシュを間違ってメインブランチにしないように注意
- タグ情報はpushされないので、下記の要領で別途pushする。
```
$ git push <remote_ref> <tagname>  # 特定のタグ情報をpush
$ git push <remote_ref> --tags  # 全てのタグ城オフをpush
$ git push <remote_ref> :<tagname>  # 特定のtag情報をリモートリポから削除
```

### `git branch <branch-name>`
- 基本的にはメインブランチはまとまった更新（バージョンアップ、リリース）をする場所。
- 開発者はブランチで日々の更新をしていく。また、開発する機能に応じてブランチを切っていく。
- その機能の開発が終わったらメインブランチにmergeして、ブランチは削除する。
```
$ git branch <branch_name>  # ブランチの作成
$ git branch -d <branch_name>  # -d オプションでmerge済みのブランチの削除
$ git branch -d <branch_name>  # -D オプションでmergeしてないブランチの強制削除
$ git branch -m <old-branch-name> <new-branch-name> # ブランチ名変更
```

### `git merge`
- 開発内容をメインブランチに反映（統合）する操作。リモートリポでプルリクエストを出してmergeするパターンと、ローカルリポでmergeするパターンがある。
- 前者はチーム開発の場合、特にメインブランチの更新の場合である。後者は個人作業の場合等。`git merge`は後者の場合で使うコマンド。
- mergeする前に`git pull`をしておくことと、`git diff`で差分確認しておくべき。
- `git merge <branchname>` 現在のブランチに<branchname>をmerge

**mergeの種類**
1. Fast forwardマージ   
ブランチを切る際のcommitを+-0として、ブランチのコミットを+1とする。この時、merge先が+-0のコミットのままであればそのままマージ（単純な更新）ができる。これをFast forwardマージといい、なんの問題もなくmergeできるパターン。

2. Automatic Merge   
実際にはmerge先で別のコミットがある場合がある。この場合でも更新が競合しなければmerge可能である。これを自動で判別して問題ないことを判定してmergeすることをAutomatic Mergeという。Automatic mergeの場合、統合先に存在していた+1と今回統合する+1どちらのコミットにも当たらない新たなmergeコミットが作成されることになる。

3. Conflict   
更新が競合した場合をConflictと呼ぶ。コンフリクトが生じた場合、git側でconflictを発見し箇所が明示される。また、その箇所にはアノテーションが入る。その箇所を手動で修正（あるコミットによる編集を残し、他のコミットの編集を削除する作業）をして再度add&コミットすることで解消される。この操作はmergeではなくて普通のcommitコマンドであるが、実際にはのmerge commitが作成される。



### GihHubでのマージ（プルリク）
- ブランチをメインブランチに統合する（メインを更新する）
- Pull requestsタブ -> どのブランチからどのブランチに、の選択  
-> 差分が出てくるので確認 -> "create pull request"  
-> titleや内容を記載してもう一度"create pull request"  
-> Pull requestsタブからPull requestsを確認。差分を見て問題なければ"Merge pull request"でマージする。  
-> リポトップページの"branchs"にいくとmerge済みのブランチはmergedマークがついているので、右側のゴミ箱ボタンを押して削除 (ローカルのブランチはgit branch -d repo-name)


### `git diff`
- マージ前や、適時の差分確認
- 基本的には左側に編集前、右側に編集後を置く
- tagが使える。また、diftoolを設定していればdiftoolコマンドで立ち上げることができる。
```
git diff <base-branch> <compare-branch>
git diff <tagname1> <tagname2>
git difftool <tagname1> <tagname2>
```

### `git checkout`
- ブランチの切り替え。実際は任意のコミットIDに切り替え可能
- master等のブランチ名は特定のコミットを指すものなので、そこがブランチ名でもコミットIDでもHEADでも良い。
```
$ git checkout <branch-name>  # ブランチ切り替え
$ git checkout <commitID>  # コミットへ移動
$ git checkout tags/<tagname>  # タグで指定
$ git switch <branch_name/commitID>  # でも可（v2.23以降）
$ git checkout -b <branch-name>  # ブランチ作成(git branch)と切り替え(checkout/switch)を同時に実行。
$ git switch -c <branch_name>  # 同上
```

### `git restore --staged <file>`
- stagedのstatusのファイルを一旦unstageする
- `git restore <file>`
- working directoryでの作業をキャンセルする（編集前に戻す）。


### `git tag`
- tag機能は特定のコミットに任意のタグをつけられる便利な機能
- 通常はバージョン名がつけられる。(v1.0 とか）
- commitIDと同じように扱えることが多い。diffもタグで指定できる。
- `git log`でもcommitIDだけでなくtagが見れる。
- tagのpushは通常のpushとは別途必要(push参照)
```
$ git tag <tagname>  # 最新のコミットにtagをつける
$ git tag -a <tagname>  # アノテーションをつける。エディタが開くのでそこに情報を書き足す。基本こっちを使うべし
$ git tag -a <tagname> <commitID>  # 最新のコミットではなく、指定のコミットにタグをつける。これを使うことが最も多そう。
$ git tag --list  # タグ一覧を表示
$ git tag --delete <tagname>  # 指定したtagの削除
```

### `git mv <filename1> <filename2>`
- git管理下のファイル名の変更
- mvコマンドだと削除＆新規作成の扱いになる
- git mv を使うとシンプルにファイル名変更の扱いにできる

### `git rm <filename>`
- git管理下のファイルの削除
- 削除の操作もgitではrestoreで戻ることができる。
- rmコマンドだと「ファイルの削除」操作をstageするために`git add -A`が必要になる（ちょっと面倒）




### README.md について
- 基本的には5W1Hを記述
- ただし簡潔に。長い詳細はGitHub上のwikiに書く。Wikiに書く場合はREADMEにその旨を書いておく。
- markdownの記法はググりながら良いチートシートを見つけておくと良い
- VScodeの場合、右上の [見開き＋虫眼鏡] ボタンをクリックでプレビュー表示しながら書ける
- 動かし方や挙動のGIFをつけておくとGOOD (GIF作成：https://convertio.co/)。`![alt txt](url or path)`でgifや画像を添付できる。pathは相対ぱすで良いので、画像用のフォルダを作って指定（images/hoge.gif)
