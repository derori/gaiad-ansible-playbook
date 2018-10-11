cosmos.networkの gaiadのシンプルなインストールplaybookです。
ubuntuを対象にして、デプロイサーバ(クライアントPC可)からリモートサーバへのデプロイを想定しています。
※ansibleあんまり触ったことありまへん。

対象のサーバにsudoできるユーザーでログインしておいてね。

# 回し順

## install golang

golang, depのインストールとgaiadユーザーの追加をします。

e.g `ansible-playbook -i ansible/inventory/hosts --private-key=PKEY -u USERNAME -become ansible/playbook/golang-install.yml`

## install gaiad

gaiadのソースのDLとビルドを行います。systemdへの登録もココで。

`ansible-playbook -i ansible/inventory/hosts --private-key=~PKEY -u USERNAME -become ansible/playbook/gaiad-install.yml`


# 初期セットアップ

対象先のサーバでノードを立ち上げます。鍵などがターミナルに表示されるので手動でやる感じ。

公式ドキュメントはこちら https://cosmos.network/docs/getting-started/full-node.html

## gaiad init

初期化ファイルを生成してくれます。

```
$ gaiad init --name gerorin
Password for account 'gerorin' (default 12345678):
{          
  "chain_id": "test-chain-m7fFCA",                                 
  "node_id": "d23177eb246db4490a90f34e7ceea5d386c3dce1",
  "app_message": {
    "secret": "fruit rather cloud blade harbor pistol hybrid same main cup yellow alone lake win skate toilet about kingdom ready canyon truth advance awkward clap"
  }
}
```

もしアップデートなどによって genesis.json 初期以外のものを利用したい場合には https://github.com/cosmos/testnets あたりから持ってきて上書きするとよいでしょう。

## config.toml

~/.gaiad/config/config.toml 内のseedsに初回につなぐノードを入れておく必要があります。
https://cosmos.network/docs/getting-started/full-node.html#add-seed-nodes

をそのままどうぞ。


# 起動

systemdで素直に起動すればよいです。

`$ sudo systemctl start gaiad`

OS起動時に自動起動にしたければenabledにしてくことをお忘れなく。
`$ sudo systemctl enable gaiad`

