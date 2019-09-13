# ansible-sample

## リポジトリの概要
本リポジトリは、Ansibleで各種サーバを構築する場合のサンプルファイルを保管しています。

## 前提条件
現在想定しているサーバ種別(roles)は以下の通りです。
- フロントエンドサーバ(HAProxy)
- APサーバ(Apache/PHP)
- DBサーバ(MariaDB)
- コンテナ基盤サーバ(Docker)

OSはCentOS 7を想定しています。

## Ansible環境準備
### パッケージインストール
構築対象のサーバとは別に、CentOS 7がインストールされた管理サーバを用意し、必要パッケージをインストールします。

```
# yum -y install git epel-release
# yum -y install ansible
```

### 設定ファイル編集
SSH認証鍵のチェックをスキップするため、Ansibleの設定ファイルを編集します。

```
# vi /etc/ansible/ansible.cfg

#ssh_args = -C -o ControlMaster=auto -o ControlPersist=60s
↓変更
ssh_args = -C -o ControlMaster=auto -o ControlPersist=60s -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
```

## 実行方法
### Playbook取得
本リポジトリをクローンし、カレントディレクトリを変更します。

```
$ git clone https://github.com/thoshino2501/ansible-sample.git
$ cd ansible-sample
```
### 作業対象サーバの設定
hostsファイルを編集し、作業対象サーバの情報を指定します。

```
$ vi hosts
```

### パラメータの設定
group_varsフォルダ内の各ファイルを編集し、各サーバのパラメータを指定します。

```
$ vi group_vars/all.yml
```

### 作業対象Playbookの設定
site.ymlファイルを編集し、実行するPlaybookファイルを指定します。

```
$ vi site.yml
```

### Ansibleの実行
Ansibleを起動し、Playbookを実行します。
なお、-vオプションを付与すると、より詳細な処理内容が処理中に表示されます。

```
$ ansible-playbook -i hosts site.yml
```
