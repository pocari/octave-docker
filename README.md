# octave + gnuplot 用Docker

## 必要ライブラリ

グラフの描画内容を表示するためにdocker側からmacのXQuartzに飛ばすためmac側でも準備が要る

```
brew install socat
brew install caskroom/cask/brew-cask
brew cask install xquartz
```

## build

```
docker build -t octave-docker .
```

## run

別ウィンドウでdockerから表示するためのmac側の待受ポートを設定する。
これでDocker側からこのPCのipアドレスに向けてグラフの画像を送信できる。
```
socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"
```

DISPLAY環境変数を(このPCのipアドレス:0)に設定してコンテナ起動
```
docker run --rm -it -e DISPLAY=$(ifconfig en0 | grep -v inet6 | grep inet | awk '{print $2 ":0"}') -v $(pwd):/var/ruby octave-docker bash
```

## sample

コンテナ内で下記を実行
```
# octaveのサンプル
octave
> source samples/octave_sample.m

# gnuplotのサンプル
gnuplot
> load "samples/gnuplot_sample.plt"
```
