# SSHキー認証を使用してVS CodeからWSL上のLinuxに接続する

このガイドでは、Visual Studio Code（VS Code）を使用してWSL上のLinuxにSSHキー認証を使用して接続する手順を説明します。これにより、パスワードを求められることなく接続できます。

## 前提条件

- Mac上でSSHキーペアを生成していること。
- WSLがインストールされ、Linuxディストリビューションが実行されていること。

## 手順

1. **SSHキーペアの生成**:

   Mac上でSSHキーペアを生成します。

   `ssh-keygen -t rsa`


### 公開鍵をWSLにコピー

生成した公開鍵（通常は ~/.ssh/id_rsa.pub）をWSL上のLinuxにコピーします。SSHを使用して、MacからWSLに接続し、公開鍵を ~/.ssh/authorized_keys ファイルに追加します。

`ssh username@<WSL_IP_ADDRESS> 'echo "$(cat ~/.ssh/id_rsa.pub)" >> ~/.ssh/authorized_keys'`

### VS Codeのリモート開発拡張機能を設定

VS Codeを開き、リモート開発拡張機能をインストールします。
左下の「Remote Explorer」アイコンをクリックし、リモートサーバーのリストを表示します。
左上の歯車アイコンをクリックし、「Remote - SSH: Connect to Host...」を選択します。
SSH構成ファイルを編集:

`~/.ssh/config` ファイルを作成または編集し、WSLへの接続設定を追加します。以下は例です。

```
Host mywsl
  HostName <WSL_IP_ADDRESS>
  User username
  IdentityFile ~/.ssh/id_rsa
```
**<WSL_IP_ADDRESS> はWSLのIPアドレスです。username はWSLのユーザー名です。**

### VS Codeで接続

VS Codeのリモート開発拡張機能を使用して、SSHキー認証を介してWSL上のLinuxに接続します。
左下の「Remote Explorer」から、作成したSSH構成を選択し、接続します。
これで、VS CodeからWSL上のLinuxにSSHキー認証を使用して接続できます。パスワードは求められず、安全なSSHキーを介してアクセスできます。
