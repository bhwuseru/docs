# 外部のWindowsパソコンからSSHでWSLに接続

外部のWindowsパソコンからSSHを使用してWSL環境に接続する手順を説明します。まず、WSLをインストールして設定する必要があります。

## WSL環境の設定

WSL環境側の設定を行います。操作はWSLのターミナルから行います。

### 1. WSLのIPアドレスの確認

WSLのIPアドレスを確認します。Ubuntuディストリビューションを使用している場合、次のコマンドを実行します：

`ip address show eth0 | grep 'inet ' | awk '{split($2,ipa,"/");print ipa[1]}'`


### 2. SSHホスト鍵の作成

SSHホスト鍵を作成します：

`sudo ssh-keygen -A`


### 3. SSHサーバの再起動

SSHサーバを再起動します：

`sudo service ssh restart`


**エラー：Failed to restart ssh.service: Unit ssh.service not found.**
以下の手順を試してみてください
1. SSHサービスの存在を確認する

`sudo systemctl list-units | grep ssh`

- 上記のコマンドでSSH関連のユニットが表示されるかどうかを確認してください。SSHサービスがインストールされていない場合、インストールする必要があります。UbuntuベースのWSLでは、以下のコマンドでインストールできます：

`sudo apt-get install openssh-server`

2. SSHサービスを起動する
SSHサービスがインストールされている場合、以下のコマンドでサービスを起動できます：

`sudo service ssh start`

3. サービスの状態を確認する
サービスが正常に起動したかどうかを確認するために、以下のコマンドを使用します

`sudo service ssh status`
4. サービスの再起動
サービスが正常に起動している場合でも、再起動する必要がある場合は次のコマンドを使用できます：

`sudo service ssh restart`

## Windows側の設定

次に、WSLが稼働しているWindows側の設定を行います。操作は管理者権限で実行したPowerShellのターミナルから行います。

### 4. WindowsのIPアドレスの確認

WindowsのIPアドレスを確認します：

`([System.Net.Dns]::GetHostAddresses((hostname)) | Where-Object {$_.AddressFamily -eq "InterNetwork"}).IPAddressToString

`


### 5. ポートフォワーディングの設定

WSLのIPアドレスとWindowsのIPアドレスを紐付けるポートフォワーディングを設定します。SSHのポート（通常は22）を指定してください：

`netsh interface portproxy add v4tov4 listenaddress=<WindowsのIPアドレス> listenport=22 connectaddress=<WSLのIPアドレス> connectport=22`

**削除方法**

netshコマンドを使用して追加したポートプロキシ設定を削除するには、netshコマンドを再度使用して該当の設定を削除します。ポートプロキシ設定は、netsh interface portproxyコマンドを使用して管理します。以下は、ポートプロキシ設定を削除する一般的な手順です。

`netsh interface portproxy delete v4tov4 listenaddress=ローカルアドレス listenport=ローカルポート`



### 6. ファイアウォールの設定

WindowsファイアウォールでSSHのポート（通常は22）を許可します：

`New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort 22 -Action Allow -Protocol TCP`
`New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort 22 -Action Allow -Protocol TCP`


## 外部のWindowsパソコンからSSHでWSLに接続

最後に、外部のWindowsパソコンからSSHを使用してWSL環境に接続します。以下のコマンドを使用します：

`ssh <ユーザ名>@<WindowsのIPアドレス>`


これで、外部のWindowsパソコンからWSL環境にSSHで接続する手順が完了しました。上記の手順に従って設定を行い、SSH接続を試してみてください。



