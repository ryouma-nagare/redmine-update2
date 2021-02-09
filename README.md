# redmine-update2


## 概要

LinuxにインストールしたRedmine/RedMicaの本体／プラグイン／テーマをアップデートするためのplaybook

## 前提条件

* systemdを採用しているLinux
* Redmineをsvn、またはRedMicaをgitでインストールしている
* プラグイン／テーマをgitでインストールしている
* svn/git/bundle コマンドをインストールしている

## 環境依存の設定

### ホスト設定
- `inventory/hosts`
  - `ホスト名`：任意の名前（ラベル）
  - `ansible_host`：FQDN または IP
  - `ansible_ssh_user`：SSHでログインするユーザ名
  - `ansible_ssh_pass`：パスワード
  - `ansible_become_pass`：パスワード
  - `ansible_private_key_file`：鍵ファイル名

※ansible_ssh_pass/ansible_private_key_file はいずれかを指定


### ホスト変数設定

`host_vars/ホスト名` を作成する

#### プラグインのブランチ指定
- `plugins`
  - `"プラグイン名"` : `ブランチ名`

1. 複数指定する場合は行を繰り返し指定
1. 常に最新に更新するプラグインは指定不要
1. ブランチ指定がない場合は、 `plugins:` のみ残す
1. `plugins:` を消した場合は、pluginのアップデートを行いません

#### ディレクトリなどの変数
- `shell`:シェルのフルパス
- `redmine_dir`:Redmineのインストールディレクトリ
- `redmine_owner`:`redmine_dir`のオーナユーザ
- `bundle_bin`/`svn_bin`/`git_bin`:各コマンド名
  - `redmine_owner`で各コマンドにパスが通っていない場合はフルパスを記述
- `http_proxy`: プロキシURL
  - 不要な場合は空のまま

#### 再起動するサービス名
- `services`:アップデート後に`systemctl restart`するサービス名のリスト

## 実行方法

```
ansible-playbook -i ./inventory/hosts redmine-update2.yml
```
