# pypeach_playbook
## はじめに
ansibleを使用して構成管理を実現するためのplaybookです。  
サンプルアプリケーションは[Gitリポジトリ](https://github.com/pypeach/pypeach_django.git)に関する環境構築を定義しています。  

### 前提事項
linux(Ubuntu 18.04 LTS)で動作確認しています。  

|  項目 | バージョン |
|:------------|:------------|
| python | 2.7.15+ |
| ansible | 2.8.6 |


## プロジェクト構成
プロジェクト構成は以下となります。  
TODO 以下はまだ
djangoプロジェクトのデフォルトに一部リソースのフォルダを追加しました。 

```
/$  
  ├─app_pypeach_django
  │  ├─management
  │  │  └─commands
  │  └─migrations
  ├─locale
  ├─log
  ├─pypeach_django
  ├─resources
  ├─shell  
  ├─template  
  └─.gitignore等  
  
```

|  項目 | 説明 |
|:------------|:------------|
| app_pypeach_django | アプリケーションのパッケージ |
| log| ログ出力先のフォルダ |
| locale| getTextで使用するメッセージを格納するフォルダ |
| pypeach_django| django関連の設定を定義するフォルダ |
| resources| 設定ファイルを格納するフォルダ |
| shell| 起動シェルスクリプトを格納するフォルダ |
| template| テンプレートファイル（メールテンプレート等）を格納するフォルダ |

## セットアップ
ansible及びplaybookのセットアップを行います。 

### Gitリポジトリのクローン
playbookは[Gitリポジトリ](https://github.com/pypeach/pypeach_playbook.git)からクローンしてください。  

### ansibleのインストール
ansibleのインストールを行います。  

```
# python-pipをインストールする
sudo apt-get update
sudo apt install python-pip

# pipコマンドでユーザ環境にansibleをインストールする
pip install ansible --user

# bash_profileにパスを追加する
echo "export PATH=\$PATH:~/.local/bin/" >> ~/.bash_profile
source .bash_profile

# ansibleのバージョンを確認する
ansible --version

```

### ssh接続
ansibleを使用して環境構築を行うサーバにssh接続します。  

```
# SSHの公開鍵と秘密鍵を作成する
ssh-keygen -t rsa

# SSHの公開鍵を接続先サーバに転送する
ssh-copy-id -i ~/.ssh/id_rsa.pub {ユーザ名}@{ホスト名・IPアドレス}

# 必要に応じて一時的に秘密鍵のパスフレーズをssh-agentで保持します
eval `ssh-agent`
ssh-add ~/.ssh/id_rsa

```

### ansibleのコマンド実行
ansibleを使用してplaybookの定義を実行します。  

```
# コマンドを実行します
ansible-playbook -i inventory site.yml

# playbookの構文チェックを行う場合は起動オプションを指定します
ansible-playbook -i inventory site.yml --syntax-check
```

