# redmine-update2


## 概要

LinuxにインストールしたRedmineの本体／プラグイン／テーマをアップデートするためのplaybook

## 前提条件

* systemdを採用しているLinux
* Redmineをsvn、またはRedMicaをgitでインストールしている
* プラグイン／テーマをgitでインストールしている
* svn/git/bundle/awk コマンドをインストールしている
* ログインユーザが`パスワード無しでsudo`できる

## 環境依存の設定

### ホスト設定
- `inventory/hosts`
  - `ホスト名`：任意の名前（ラベル）
  - `ansible_host`：FQDN または IP
  - `ansible_user`：SSHでログインするユーザ名
  - `ansible_password`：パスワード
  - `ansible_private_key_file`：鍵ファイル名

※ansible_password/ansible_private_key_file はいずれかを指定


### ホスト変数設定

`host_vars/ホスト名` を作成する

#### プラグインのブランチ指定
- `plugins`
 - `"プラグイン名"` : `ブランチ名`

1. 複数指定する場合は行を繰り返し指定
1. 常に最新に更新するプラグインは指定不要
1. ブランチ指定がない場合は、 `plugins:` のみ残す

#### 実行するコマンド
- `shell`:シェルのフルパス
- `redmine_dir`:Redmineのインストールディレクトリ
- `redmine_owner`:`redmine_dir`のオーナユーザ
- `bundle_bin`/`svn_bin`/`git_bin`:各コマンド名
 - `redmine_owner`で各コマンドにパスが通っていない場合はフルパスを記述

#### 再起動するサービス名
- `services`:アップデート後に`systemctl restart`するサービス名のリスト

## 実行方法

```
ansible-playbook -i ./inventory/hosts redmine-update2.yml
```
