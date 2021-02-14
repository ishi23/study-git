# 日常的に利用するgitコマンドやgithubの使い方

- .gitignoreファイル
    - git管理すべきでないファイルが多く存在する
    - 特にパスワードを含むファイルやログファイル等
    - ワイルドカードやフォルダ指定も可能(*.tif, dirname/)
    - https://github.com/github/gitignore に通常どういったファイルをignoreすべきか（.gitignoreのサンプルファイル）がたくさんある。
    - 例えばpython用の例だと.ipynb_checkpointsなんかも含まれる
    - ここに指定されたファイルやフォルダはローカルリポにしか存在しない事になる。編集してもchanged statusにもならない。

- `git init`  
    - ローカルリポジトリの作成（.gitが作成される)
    - `git init <repo-name>` でrepo-nameのリポジトリを新規に作成
    - `git init` を既存のフォルダ内で実行することで既存フォルダをローカルリポジトリ化

- `git clone <url>`
    - リモートリポジトリをコピーしてローカルリポジトリを作成
    - urlはhttpでもいいがssh登録していればssh


- `git status`
    - 管理下のファイルの状態を確認
    - oh-my-zsh + powerlevel10k の環境ではstatus概要はヘッダーから確認できるため減らせる

- `git log`
    - commitのログを表示
    - --oneline オプションで簡潔に
    - --graphオプションで各コミットを線で繋ぐ

- `git remote -v`
    - ローカルリポジトリに紐づいているリモートリポジトリを確認

- `git add <filename>`
    - working director　-> staging area
    - `git add .` でリポジトリ内の全てのファイルをadd


- `git commit -m "comment"`
    - staging area -> repository
    - コメントは必ずつける

- `git pull <remote_ref> <branch_name>` 
    - ローカルリポジトリを最新状態にする
    - 複数人開発の場合はpushする前に必ず行うべき
    - remote_refは通常"origin" (git remote -v で確認)

- `git push <remote_ref> <branch_name>`
    - ローカルリポジトリのコミットをリモートリポジトリに反映
    - ブランチ名を間違えないように注意
    - ローカルで作ったブランチでリモートに存在しない場合、リモートに新しいブランチが作成される


- `git branch <branch-name>`
    - ブランチの作成
    - -d オプションでmerge済みのブランチの削除
    - -D オプションでmergeしてないブランチの強制削除

- `git checkout <branch-name>`
    - ブランチの切り替え
    - `git checkout -b <branch-name>` でブランチ作成(git branch)と切り替えを同時に実行


- `git restore --staged <file>`
    - stagedのstatusのファイルを一旦unstageする
- `git restore <file>`
    - working directoryでの作業をキャンセルする（編集前に戻す）。


- `git mv <filename1> <filename2>`
    - git管理下のファイル名の変更
    - mvコマンドだと削除＆新規作成の扱いになる
    - git mv を使うとシンプルにファイル名変更の扱いにできる

- `git rm <filename>`
    - git管理下のファイルの削除
    - 削除の操作もgitではrestoreで戻ることができる。
    - rmコマンドだと「ファイルの削除」操作をstageするために`git add -A`が必要になる（ちょっと面倒）


- GihHub: ブランチのマージ
    - ブランチをメインブランチに統合する（メインを更新する）
    - Pull requestsタブ -> どのブランチからどのブランチに、の選択  
    -> 差分が出てくるので確認 -> "create pull request"  
    -> titleや内容を記載してもう一度"create pull request"  
    -> Pull requestsタブからPull requestsを確認。差分を見て問題なければ"Merge pull request"でマージする。  
    -> リポトップページの"branchs"にいくとmerge済みのブランチはmergedマークがついているので、右側のゴミ箱ボタンを押して削除 (ローカルのブランチはgit branch -d repo-name)