# 初期設定の記録と推奨
### SSH設定: 高セキュリティな接続で一度設定すれば簡単なので、基本この方式を取る
- `$ ssh-keygen -t ed25519 -C "<email address>"` -> SSHキーが`~/.ssh/id_ed25519.pub`に作成される。
- `$ eval "$(ssh-agent -s)"` -> ssh-agentを起動させる
- ~/.ssh/config に以下を書き込む(Macの場合。WinやLinuxの場合はUseKeycahinでエラーが出たので外したら動いた)
```
Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_ed25519
```
- `$ ssh-add -K ~/.ssh/id_ed25519` -> SSHキーをssh-agentに登録
- `$ pbcopy <~/.ssh/id_ed25519.pub` -> 公開鍵をクリップボードにコピー
- GitHub上でSettings > SSH and GPG keys > New SSH key -> 公開鍵をペーストして登録

### Git快適なプロンプト設定をする（Mac, Linuxの場合。オプションだけどおすすめ）
- bashの場合：https://datawokagaku.com/terminal_prompt_git/ 参照
- zshの場合：以下を実行する
- `$ cp ~/.zshrc ~/.zshrc_bk` .zshrcが更新されてしまうのでバックアップをとる
- oh-my-zshをインストール: https://ohmyz.sh/ に行くとインストール用のコマンドが出るので、それをコピペしてターミナルで実行する
- Powerlevel10kをcloneする: Powerlevel10kのGitHubリポジトリに行き、cloneする `$ git clone https://github.com/romkatv/powerlevel10k.git ~/.oh-my-zsh/custom/themes/powerlevel10k`
- .zshrc の編集 `ZSH_THEME="powerlevel10k/powerlevel10k"`（デフォルトで違うテーマが入っている)
- command + t でターミナルのタブを開くと初期設定モードに入る
- 質問に答えていく。好みの見た目を選んでいく。



### Gitの初期設定
- 設定ファイルは二種類。globalな設定ファイルは ~/.gitconfig で、リポ毎の設定は .git/config
- 上記のファイルを直接更新することでの設定も可能だが、`git config`コマンドから書き換えるのが安全。`--global`オプションをつけると.gitconfigの方を書き換える
- GitHubのアカウントを設定
```
$ git config --global user.name "<username>"
$ git config --global user.email "<email>"
```

- 初期状態だと`git branch`や`git diff`の出力がpager出力で鬱陶しいので変える
```
$ git config --global --replace-all core.pager "less -F -X"
```


### diff可視化ツール p4mergeのインストールと設定（オプション）
- diffを見やすくしないとよくわからん状態になるので、そのためのツール。
- p4mergeで検索
- HPで"ダウンロード" → OSとか選択するとダウンロードできる
- Macの場合、ダウンロードしたものをダブルクリックしてp4mergeをApplicationsフォルダにD＆D
- `$ ls /Applications/p4merge.app/Contents/MacOS/` でp4mergeが入っていることを確認
- `$ ./p4merge`で起動できることを確認
- 以下のコマンドでgitからp4mergeを起動できるように設定する
```
$ git config --global diff.tool p4merge
$ git config --global difftool.p4merge.path /Applications/p4merge.app/Contents/MacOS/p4merge
$ git config --global difftool.prompt false
$ git config --global merge.tool p4merge
$ git config --global mergetool.p4merge.path /Applications/p4merge.app/Contents/MacOS/p4merge
$ git config --global mergetool.prompt false
```
- `$ cat ~/.gitconfig` で正しく設定されていることを確認。下記のようになっているはず。
```
[user]
	name = ishi23
	email = ********@gmail.com
[core]
	pager = less -F -X
[diff]
	tool = p4merge
[difftool "p4merge"]
	path = /Applications/p4merge.app/Contents/MacOS/p4merge
[difftool]
	prompt = false
[merge]
	tool = p4merge
[mergetool "p4merge"]
	path = /Applications/p4merge.app/Contents/MacOS/p4merge
[mergetool]
	prompt = false
```
