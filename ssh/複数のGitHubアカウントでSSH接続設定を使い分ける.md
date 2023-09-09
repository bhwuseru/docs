# 複数のGitHubアカウントでSSH接続設定を使い分ける

GitHubに複数のアカウントを持っている場合、異なるアカウントを使い分けるためにSSH接続設定を適切に構成する必要があります。以下は、複数のGitHubアカウントを管理するためのステップバイステップのガイドです。

## 1. SSHキーの生成

### ステップ 1: 新しいSSHキーの生成

異なるGitHubアカウントごとに異なるSSHキーペアを生成する必要があります。次のコマンドを使用して新しいSSHキーを生成します。

```
ssh-keygen -t ed25519 -C "your-email@example.com"
```
または
```ssh-keygen -t rsa -C {Githubメールアドレス} -f {作成する鍵の名前}```

-t ed25519: Ed25519アルゴリズムを使用してSSHキーを生成します。より安全です。
-C "your-email@example.com": 任意のメールアドレスを指定します。

2. ステップ 2: SSHキーの名前付け
SSHキーを生成すると、デフォルトで ~/.ssh/id_ed25519（秘密鍵）と ~/.ssh/id_ed25519.pub（公開鍵）という名前で保存されます。しかし、異なるアカウントごとにキーを管理するために、キーに名前をつけましょう。以下のコマンドで名前を変更します。

```
mv ~/.ssh/id_ed25519 ~/.ssh/id_ed25519_account1
mv ~/.ssh/id_ed25519.pub ~/.ssh/id_ed25519_account1.pub
```
これでSSHキーペアが ~/.ssh/id_ed25519_account1 と ~/.ssh/id_ed25519_account1.pub という名前で保存されます。同様に、別のアカウントのキーも生成し、名前を変更します。

- SSH設定ファイルの編集
ステップ 1: SSH設定ファイルの編集
SSH設定ファイル (~/.ssh/config) を編集して、異なるアカウントごとに異なるSSHキーを使用するように設定します。以下は設定ファイルの例です。
```
# アカウント1のGitHub
Host github.com-account1
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_account1

# アカウント2のGitHub
Host github.com-account2
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_account2
Host: 任意のホスト名を指定します。この名前を後でSSH接続に使用します。
HostName: GitHubのホスト名を指定します。
User: git ユーザーを指定します。
IdentityFile: 対応するSSHキーペアのパスを指定します。
```

### ステップ 2: 複数のアカウントを設定
必要に応じて、これらの設定を追加し、アカウントごとに設定を変更します。

3. SSHキーのGitHubに登録
GitHubの各アカウントに対応するSSH公開鍵を追加します。

GitHubアカウントにログインし、設定（Settings）にアクセスします。
左側のサイドバーで「SSH and GPG keys」を選択します。
「New SSH key」または「Add SSH key」ボタンをクリックします。
キーの名前を入力し、公開鍵ファイル (~/.ssh/id_ed25519_account1.pub) の内容をコピーして貼り付けます。
キーを追加または保存します。
同様に、異なるアカウントに対応する公開鍵を追加します。

4. 接続のテスト
接続をテストするために、次のコマンドを使用します。

`ssh -T github.com-account1`

または

`ssh -T github.com-account2`
適切なアカウントとキーが使用されるかどうかを確認します。正常に接続できれば、GitHubアカウントごとに異なるSSHキーを使用することができます。

以上が、複数のGitHubアカウントでSSH接続設定を使い分けるための手順です。

