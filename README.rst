設定
======================================

Conda環境で実行してください．
プロジェクトのディレクトリに利用するデータの置き場やパラメータの保存場所を示す `config/paths.json` を置いてください．
たとえば内容は以下のような感じです．

::

    {
        "meta": "/data/unagi0/ono/financial_data/meta/",
        "data_dir": "/data/unagi0/ono/",
        "daily_historical": "/data/unagi0/ono/financial_data/stock_daily_historical/stock_price_db/",
        "yahoo_finance": "/data/unagi0/ono/financial_data/stock_daily_historical/yahoo_finance/",
        "ili": "/data/unagi0/ono/ili_data/"
    }


実験
======================================

学習
######################################

人工データでの実験
--------------------------------------

::

    python panel_transformer/train_particle.py
        -m {btmvn, btmse, vtmvn, vtmse}
        -g <gpu_number>
        -e <max_epochs>
        -n {drop, fill, flag}
        -r <repetition>
        -mr <missing_rate>

コマンドライン引数は他にもありますが，他はいじらなくて大丈夫です．
修論では `missing_rate` を `{0.00, 0.05, 0.10, 0.15, 0.20}` と変化させて，欠損率への頑健さを確認しました．
オプション `-r` は実験を繰り返す回数で5回程度やって結果の平均と標準偏差を計算すればよいと思います．

インフルエンザ様感染症データでの実験
--------------------------------------

::

    python panel_transformer/train_ili.py
        -m {btmvn, btmse, vtmvn, vtmse}
        -g <gpu_number>
        -e <max_epochs>
        -n {drop, fill, flag}
        -r <repetition>


東証株式データでの実験
--------------------------------------

::

    python panel_transformer/train_tosho.py
        -m {btmvn, btmse, vtmvn, vtmse}
        -g <gpu_number>
        -e <max_epochs>
        -n {drop, fill, flag}
        -r <repetition>
        -c

評価
######################################

学習データ・検証データ・テストデータに対する評価が `./runs/` 以下に保存され， tensorboard で閲覧することができます．
`eval.ipynb` は，検証データでの性能がいいエポック数を選択して，テストデータで評価するプロセスを自動化したものです．
