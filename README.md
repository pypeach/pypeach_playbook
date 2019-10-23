# pypeach_playbook
## はじめに
Ansibleを使用して環境構築するためのplaybookです。  
サンプルアプリケーションは[Gitリポジトリ](https://github.com/pypeach/pypeach_django.git)に関する環境構築を定義しています。  

### 前提事項
linux(Ubuntu 18.04 LTS)で動作を行います。  

|  項目 | バージョン |
|:------------|:------------|
| python | 2.7.15+ |
| ansible | 2.8.6 |

## プロジェクト構成
プロジェクト構成は以下となります。  

```
/ansible_playbook
├─group_vars
├─roles
│   ├─account
│   ├─mysql
│   └─python
├─ app_server.yml
├─ inventory
└─ site.yml
  
```

|  項目 | 説明 |
|:------------|:------------|
| group_vars | グループ共通の共通変数を定義したファイルを格納するディレクトリ |
| roles| playbookを格納するディレクトリ |
| app_server.yml| サーバで実行するplaybookを定義する |
| inventory| サーバ接続先を定義する |
| site.yml| マスタのplaybook。サーバ単位のplaybookのインポートを定義する |

## セットアップ
ansible及びplaybookのセットアップを行います。 

### ansibleのインストール
ansibleのインストールを行います。  

```
# python-pipをインストールする
sudo apt-get update
sudo apt install python-pip

# pipコマンドでカレントユーザにansibleをインストールする
pip install ansible --user

# bash_profileにパスを追加する
echo "export PATH=\$PATH:~/.local/bin/" >> ~/.bash_profile
source .bash_profile

# ansibleのバージョンを確認する
ansible --version

```

### SSH接続設定
Ansibleを使用して環境構築を行うサーバにssh接続します。  

```
# SSHの公開鍵と秘密鍵を作成する
ssh-keygen -t rsa

# SSHの公開鍵を接続先サーバに転送する
ssh-copy-id -i ~/.ssh/id_rsa.pub {ユーザ名}@{ホスト名・IPアドレス}

# 必要に応じて一時的に秘密鍵のパスフレーズをssh-agentで保持します
eval `ssh-agent`
ssh-add ~/.ssh/id_rsa

```
### Gitリポジトリのクローン
playbookは[Gitリポジトリ](https://github.com/pypeach/pypeach_playbook.git)からクローンしてください。  

## playbook実行
Ansibleを使用してplaybookの定義を実行します。  

```
# コマンドを実行します
ansible-playbook -i inventory site.yml

# playbookの構文チェックを行う場合は起動オプションを指定します
ansible-playbook -i inventory site.yml --syntax-check
```

