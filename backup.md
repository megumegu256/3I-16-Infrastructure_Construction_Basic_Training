# Linux(Ubuntu)サーバの構築および運用管理の手順書

## 3年Iコース16番 富永恵

## 目次

## 環境情報

## 1 Ubuntuで起動する
### Ubuntuのインストールメディア作成

-  配布のUSBメモリからダウンロードしたisoファイル(https://www.ubuntulinux.jp/download/ja-remix)をrufus(rufus-4.6.exe	標準	Windows x64	1.5 MB	2024.10.21)で選択、上書き予定のusbを差し込んでそれを選択。

- "スタート"、"OK"、以降"はい"をクリックし続ける。

- 準備完了後、"再度スタート"をクリックするとインストールが再度初めから始まってしまうためクリックしないように注意する。

### Ubuntuのインストール
- OSをインストールするPC(ASUS laptop)にusbを差し込んで起動、F2、F10キークリック等でBIOS画面を開く。

- 開かれた画面の右側のBoot Priorityの"UEFI: Generic Flash Disk 8.07"を一番上に移動させ、F10キーを押す等してOKで保存する。

- "Try or Install Ubuntu"を選択して、ENTERキーをクリック、または放置でインストール画面に切り替わる。

- "Ubuntuをインストール"をクリック。

- "Japanese"、"Japanese"、を選択し、"続ける"をクリック。

- "I don't want to connect to a Wi-Fi network right now"を選択し"続ける"をクリック。

- "通常のインストール"を選択し"続ける"をクリック。

- "続ける"をクリック。(ディスクを削除してUbuntuをインストール)

- "続ける"をクリック。

- 住んでるところ入力して"続ける"をクリック。

- 続けて情報を入力し、"続ける"をクリック。

- インストール完了後、"今すぐ再起動をする"をクリックする。

- "Please remove the installation medium, then press ENTER"の表示が出たら、USBメモリを抜いてからENTERキーを押す。

- Ubuntuを選択するか、放置すると画面が切り替わる。

- Livepatchの画面が出たら、"次へ"、"次へ"、"次へ"、"完了"をクリック。

- これで、Ubuntuをインストールし、OSを起動させることができた。


## 2
- Vimの説明


## 3 ネットワークの基本と設定
### rootユーザーのパスワード設定
- $ sudo passwd root
- Ubuntu をインストール直後は root ユーザにはパスワードが設定されていないため、自分で設定する必要がある。

###  ユーザーの新規追加(adduser)
- 以下の名を例に実行する。
- ユーザー名 : "megu64"
- パスワード : "megu64"

- デスクトップ左下画面の9つの点のアイコンをクリックし、ターミナルを開く。
- ターミナルの操作一覧を以下に示す。
<>


- 新規に追加するユーザー名をmegu64として、"$ sudo adduser megu64" を実行。
- パスワードとして"megu64"を入力してEnterキーを押す。
- パスワードとして"megu64"を再入力してEnterキーを押す。
- Enterキーを押し続け、場合に応じて"Y"と入力してEnter。
- ユーザーの新規追加が完了する。

### ユーザーをグループへ追加
- 以下の名を例に実行する。
- グループ名 : "megu64"
- ユーザー名 : "megu64"

- "$ sudo usermod -aG megu64 megu64" ("$ sudo usermod -aG グループ名 ユーザ名") を実行する

### Webサーバの構築
- Wi-Fiに接続する。
- "$ sudo apt update" を実行し、パッケージを更新する。
- "$ sudo apt -y install apache2" を実行し、Apache HTTP Serverのインストールをする。

- "$ sudo systemctl start apache2" を実行し、Apacheを起動。
- "$ sudo systemctl enable apache2" を実行し、Apacheの自動起動の有効化。

- "$ sudo systemctl status apache2" を実行すると、Apacheのステータスを表示することができる。

- Firefoxウェブ・ブラウザを開き、url="http://localhost"を開くと、以下のような、Apacheのデフォルトページが閲覧できる。
<画像>


## 4
### Apacheの設定
- "$ sudo touch /etc/apache2/sites-available/test.conf" を実行して、デフォルトで設定されている"000-default.conf"とは異なる、"test.conf"(以降、設定ファイル)という仮想ホストを作る。

- "$ sudo vi /etc/apache2/sites-available/test.conf" を実行し、Vimで設定ファイルを編集する。Vimの使い方は2に記した通り。

- ディレクトリファイルを"/var/www/html"、ファイル名を"test"とする設定ファイルを以下のテキストのように編集する。
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    DirectoryIndex test
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

- 自身がコンテンツをデプロイするディレクトリのパスを"/var/www/html"として、 "$ sudo mkdir -p /var/www/html" ("$ sudo mkdir -p ディレクトリパス")を実行してディレクトリを作成する。

- トップページとして表示したいファイル名を"test.html"として、"$ sudo touch /var/www/html/test.html" ("$ sudo touch /var/www/ディレクトリ名/ファイル名")を実行して、ファイルを作成する。



- "$ sudo vi /var/www/html/test.html" ("$ sudo vi /var/www/ディレクトリ名/ファイル名")を実行して、作成したファイル"/var/www/html/test.html"にHTML形式で記述し保存する。以下に、開いたWebページに"Welcome to my page"と表示される例の記述を示す。
<html>
    <body>
        <h1>Welcome to my page</h1>
    </body>
</html>

$ sudo a2ensite ファイル名.conf # 仮想ホストの有効化
$ sudo systemctl reload apache2 # 有効化した仮想ホストをサーバに反映

$ sudo a2dissite 000-default.conf # 仮想ホストの無効化
$ sudo systemctl reload apache2 # 有効化した仮想ホストをサーバに反映

$ sudo ufw enable # ファイアウォールの有効化
$ sudo ufw allow ‘Apache’ # 80 番ポートの開放
$ sudo ufw status

- $ sudo systemctl reload sshd (sshリロード)


### SSHの利用


## 5
13.3~
### 2段階認証の利用
- SSH を利用するクライアント PC で鍵生成を行う。Windows の場合は、コマンドプロンプトもしくは PowerShell から、Mac や Linux の場合は、ターミナルもしくは端末から鍵生成を行う。(Windows)

- $ ssh-keygen -t ed25519 -f ./.ssh/id_ed25519 
- ($ ssh-keygen -t 暗号化アルゴリズム(主流はed25519) -f ファイル名)

- $ cat $HOME\.ssh\id_ed25519.pub | ssh megu64@10.133.3.68 -p 2022 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
- ($ cat $HOME\.ssh\公開鍵のファイル名(拡張子は.pub) | ssh ユーザ名@ホスト名(もしくは IP アドレス) -p ポート番号 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys")

- サーバーPCで $ sudo vi etc/ssh/sshd_config  に AuthenticationMethods publickey,password を入力して保存する
- $ sudo systemctl reload ssh を実行してSSHをリロードする

### Dockerを使った環境構築や運用

- 以下のdownload_install.shをロードする
#### Add Docker's official GPG key:
- $ sudo apt-get update
- $ sudo apt-get install ca-certificates curl
- $ sudo install -m 0755 -d /etc/apt/keyrings
- $ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
- $ sudo chmod a+r /etc/apt/keyrings/docker.asc

#### Add the repository to Apt sources:
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


## URL
### 完了
- 

### Ubuntu Download
- https://www.ubuntulinux.jp/download/ja-remix
### Rufus Download
- https://rufus.ie/ja/

### 参考
- https://diagnose-fix.com/topic2-003/