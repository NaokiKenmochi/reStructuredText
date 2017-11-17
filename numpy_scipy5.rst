NumPyによる計算高速化
========================

Pythonは動的型付けを行う（実行時に型を決める言語）インタプリタであるため，柔軟で短いコードが書けたり，短時間で開発ができると言うメリットで十分かもしれませんが，やはり科学技術計算をしようとするならば高速な計算が望ましいです．
そこで，この節では高速な科学技術計算のためのテクニックを幾つか紹介します．
　数値計算をする上でfor文による多重ループを使いたくなる事があるでしょう．
しかし，コンパイル言語ではないPythonでは，for文を用いると処理が非常に遅くなります．
そこで，NumPyの組み込み関数(universal function)を活用します．
ユニバーサル関数とは，ndarrayの全要素に対して，ブロードキャスティングにより要素ごとに演算処理を行い，結果をndarrayで返す関数です．
これらの関数はCやFortranで実装されており，かつ線形演算ではBLAS/LAPACKのおかげで，C/C++と遜色のないほど高速で動作します．

.. ipython:: python

    import time
    import sys
    
for文を使って１から１億までの和を計算する（Python的な書き方ではない）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. ipython:: python
    
    def test_for_loop():
        tick = time.time()
        s = 0
        for i in range(1, 100001):
            s += i
        print('Calculation result: %d' % s)
        tock = time.time()
        print('Time of %s: %.06f[s]' % (sys._getframe().f_code.co_name, tock-tick))


1から1億を返すイテレータを用意し，その和を計算する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. ipython:: python
    
    def test_sum():
        tick = time.time()
        s = sum(range(1, 100001))
        print('Calculation result: %d' % s)
        tock = time.time()
        print('Time of %s: %.06f[s]' % (sys._getframe().f_code.co_name, tock-tick))

NumPyを使い，1から1億が入った配列を用意し，その和を計算する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. ipython:: python

    def test_numpy_sum():
        tick = time.time()
        a = np.arange(1, 100001, dtype=np.int64)
        print('Calculation result: %d' % a.sum())
        tock = time.time()
        print('Time of %s: %.06f[s]' % (sys._getframe().f_code.co_name, tock-tick))
    
用意した関数を実行して，計算速度を比較してみましょう．

.. ipython:: python
    
    test_for_loop()
    test_sum()
    test_numpy_sum()

このように，numpy.sumを用いると，for文を用いた場合に比べて計算時間を１０分の１以下に抑えることができる場合があります．

numpy.whereを用いた条件制御
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

次に，ブロードキャストを利用した高速化の例として，ユニバーサル関数であるnp.whereを用いた例を紹介します．
科学技術計算をする上で，for文とともに頻出なのが三項演算子（条件文）である”x if condition else y”の処理でしょう．
numpy.whereはこの三項演算子のベクトル演算版です．
x, yを配列または数値として，np.where(条件, x, y)のように書きます．
まずは簡単な例として，真偽値の配列condと２つの配列xarr, yarrを用いて挙動を見てみましょう．

.. code-block:: python

    cond = np.array([True, True, False, True, False])
    xarr = np.array([1.0, 1.1, 1.2, 1.3, 1.4]) 
    yarr = np.array([2.0, 2.1, 2.2, 2.3, 2.4])

cond, xarr, yarrを上記のように定義します．
このとき，condの要素がTrueであればxarrの同位置の要素を，Falseであればyarrの同位置の要素を取る処理を考えます．
これをPythonのリスト内包を用いて書くと次のようになります．
    
.. code-block:: python

    result = np.where(cond, xarr, yarr)

np.whereの2番目と3番目の引数（先ほどの例ではxarr, yarr）は，配列でなくスカラー値を取ることもできます．
np.whereを使う主な場面は，ある配列を基にして別の配列を作るようなときでしょう．

np.where関数に配列を渡すとき，同じサイズの1つの配列や1つのスカラー値を渡す以外にも別の方法があります．
個々ではその一例を紹介します．
少し工夫をすると，np.whereで更に複雑なことができます．
2つの真偽値の配列cond1とcond2があるとします．
このとき，とりうる真偽の組は4種類あります．
この種類に応じて，それぞれ別の値を割り当てたいとします．
この処理をPython標準機能で書くと次のようになります．

.. code-block:: python

    result = []
    for i in range(n):
        result.append(0)

これをnp.whereを使って書くと次のようになります.
   
.. code-block:: python

    np.where(cond1 & cond2, 0,
        np.where(cond1, 1, 
            np.where(cond2, 2, 3)))

pythonの処理を高速化するには，ndarrayのユニバーサル関数や演算を用いて可能な限りforループを使わずに基礎的な数値計算を実装することが鍵になります．




