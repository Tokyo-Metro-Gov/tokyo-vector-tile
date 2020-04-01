# 東京都 新型コロナウィルス感染症対策サイト用 Vector Tile

東京都を含む範囲のタイルを配信します。

## 地図のアップデート

`tokyo.mbtiles.a*`を差し替えてmasterブランチにpushするとGithub Actionsで自動的に公開されます。

また、tile.jsonの絶対パスはGithub Actionsで置換されます。

## mbtiles の作り方

### 事前準備

必要なツールをインストール(Ubuntuの例)

```
sudo apt install bc make wget git vim docker.io
```

Ubuntuでは`docker-compose`は別途インストールを行う。またDockerの実行権限を付与する。

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo usermod -aG docker $USER
```

[openmaptiles](https://github.com/openmaptiles/openmaptiles/blob/master/README.md) のインストールと設定

```
git clone https://github.com/openmaptiles/openmaptiles.git
cd openmaptiles
# edit .env
vim .env # set QUICKSTART_MAX_ZOOM=14
```

### mbtilesの作製

`quickstart.sh`を`kanto`オプションをつけて実行する

```
./quickstart.sh kanto
```

出力された `data/tiles.mbtiles` を `tokyo.mbtiles` として本レポジトリにコピーしたあと、`split`コマンドで分解したものをコミットする。

```
cp data/tiles.mbtiles $REPO_DIR
cd $REPO_DIR
split -b 50m tokyo.mbtiles tokyo.mbtiles.
rm tokyo.mbtiles
```

#### 注意点

- `./quickstart.sh` は引数にある範囲の`{region}.osm.pbf`があるかをチェックします。
- `./quickstart.sh` の引数なしで実行するとアルバニアの範囲がダウンロードされてしまうので注意すること。

## 範囲

http://download.geofabrik.de/asia/japan/kanto.html を参照。

## Copyright

- [© OpenMapTiles](https://www.openmaptiles.org/)
- [© OpenStreetMap contributors](https://www.openstreetmap.org/copyright)
