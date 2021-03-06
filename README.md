# vibro-acoustic-solver for learning  

### 使い方
linux/octave用solverです．文字コードおよびコメントアウトに注意してください．（下記参照）  
実行方法はmain.m スクリプトファイルを実行するだけです．  
solverは３種類から選ぶことができます．
main.mの中盤に，次の３つの入力ファイルが書かれている部分があるので，
実行したい入力ファイルを選んで，それ以外の関数をコメントアウトしてください．
```matlab
%% ３種類の入力ファイルを実行できます．
%% 実行したい入力ファイル以外をコメントアウトしてください．
sample_truss2D(id);               % 静定トラス問題
sample_bar1D(id);                 % １次元梁問題
inputPureAcousFemTest(itr,id);    % 音響問題（周波数領域計算）
```
###### sample_truss2D  
二次元２自由度トラス問題です．トラス構造の２次元図が描画されたのち，
各部材の応力などの結果が標準出力されます．
周波数ループに対応していません，itr.start とitr.stop を同じ値にしてください．
###### sample_bar1D  
１次元１自由度(変位d)梁問題です．変位と応力の分布を標準出力＆描画します．
周波数ループに対応していません，itr.start とitr.stop を同じ値にしてください．
###### inputPureAcousFemTest  
３次元１自由度(圧力p)音響問題です．圧力分布などをvtk形式で出力します．
アセンブリの実装が一般的です．
周波数ループに対応しているので，itr.start，itr.stop，itr.df を設定してください．

### コメントアウトについて
デフォルトではoctave用になっています．  
本文中の#を%へ置換することでmatlab対応になります．  
linuxの場合，次のコマンドを実行することで置換可能です.  
```bash
# "#" を "%" に置換 
find . -name "*.m" -exec sed -i -e "s/%/#/g" {} \;
```

ただし，%を#に戻す場合は，置換後にformat文の手動修正が必要です．
```matlab
% 例：修正前
fprintf(fid, 'POINT_DATA #d \n nnp')
% 例：修正後
fprintf(fid, 'POINT_DATA %d \n nnp')
```

### 文字コード/改行コードについて
デフォルトではlinux用になっています．
utf8-LF -> sjis-CRLFへ変換することで，windows用になります．
```bash
# utf8-LF -> sjis-CRLF変換例 (nkf要インストール)
find . -name "*.m" -exec nkf --overwrite -Lw -s {} \;
```

### paraview pythonについて
音響問題ソルバーは後処理でVTKファイルを出力します．
VTKファイルは周波数連番となっており，paraview_python/view.py 
バッチファイルで圧力分布の全周波数可視化が自動実行可能です．
paraview を起動したのち，[tools]-[python shell]を選択し，
Python Shell を起動してください．
[Run Script]からview.py バッチファイルを選択すると自動実行が始まります．

バッチファイルをカスタマイズしたい場合は，[tools]-[Start/Stop Trace]を活用してください．
GUI操作をPythonスクリプトとして出力します．  
[参考：Paraviewによる可視化中級編(pdf)](http://www.opencae.jp/attachment/wiki/%E3%82%AA%E3%83%BC%E3%83%97%E3%83%B3CAE%E3%82%B7%E3%83%B3%E3%83%9D%E3%82%B8%E3%82%A6%E3%83%A02011/course20111201handout.pdf)

### log
* マルチドメイン連成計算からシングルドメイン計算のみに変更
* meshod ID(4) (吸音材biotモデル)を削除
* paraview_pythonディレクトリとpythonスクリプトの追加

### 参考文献
有限要素法の勉強に用いた資料の一覧です．  
* [MATLABによる有限要素プログラム](http://pub.maruzen.co.jp/book_magazine/support/FTM_12chapter/chap12_Japanese.pdf)  
このソルバーの骨子を作るにあたり，参考にしました．  
とりわけ，2Dトラス問題と1D梁問題を参考にさせていただいています．  
このソルバーにはそれらに加えて，3D音響問題を実装しています．  

* [よくわかる有限要素法](http://www.fem.gr.jp/yourtextbook.html)  
有限要素法の勉強には，このホームページ＆書籍が参考になります．  
この内容はオーム社から書籍になっており，そちらの方が見やすいので，
図書館で借りることをお勧めします．  
