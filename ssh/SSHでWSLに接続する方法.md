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


## Windows側の設定

次に、WSLが稼働しているWindows側の設定を行います。操作は管理者権限で実行したPowerShellのターミナルから行います。

### 4. WindowsのIPアドレスの確認

WindowsのIPアドレスを確認します：

`([System.Net.Dns]::GetHostAddresses((hostname)) | Where-Object {$_.AddressFamily -eq "InterNetwork"}).IPAddressToString

`


### 5. ポートフォワーディングの設定

WSLのIPアドレスとWindowsのIPアドレスを紐付けるポートフォワーディングを設定します。SSHのポート（通常は22）を指定してください：

`netsh interface portproxy add v4tov4 listenaddress=<WindowsのIPアドレス> listenport=22 connectaddress=<WSLのIPアドレス> connectport=22`


### 6. ファイアウォールの設定

WindowsファイアウォールでSSHのポート（通常は22）を許可します：

`New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort 22 -Action Allow -Protocol TCP`
`New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort 22 -Action Allow -Protocol TCP`


## 外部のWindowsパソコンからSSHでWSLに接続

最後に、外部のWindowsパソコンからSSHを使用してWSL環境に接続します。以下のコマンドを使用します：

`ssh <ユーザ名>@<WindowsのIPアドレス>`


これで、外部のWindowsパソコンからWSL環境にSSHで接続する手順が完了しました。上記の手順に従って設定を行い、SSH接続を試してみてください。



