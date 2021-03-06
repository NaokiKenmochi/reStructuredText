配列の生成
==============================
NumPy/SciPyを使う準備ができたところで，配列(ndarray)を作ってみましょう．
NumPyには配列を生成する方法が幾つかありますが，最も簡単なのはnp.arrayを用いる方法でしょう．
np.arrayは引数に列挙形式の変数を取り，そのデータを格納した新しいndarray変数を返します．

.. ipython:: python

    a = np.array([1, 2, 3, 4, 5])
    a

引数の列挙変数をネストさせることで，多次元配列を生成することもできます．
この場合はネスト内は同じ要素数にする必要があります．

.. ipython:: python

    b = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
    b

np.array以外にもndarrayの作成方法はいくつかあります．
ここでは，代表的なものとしてnp.zeros, np.ones, np.arange, np.linespace, np.eyes及びnp.identityを紹介します．

numpy.zeros
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

np.zerosを用いることで，0で埋められた指定した形状(shape)をもつ配列を生成できます．

.. ipython:: python
    
    np.zeros(5)
    np.zeros((2, 3))

numpy.ones
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

np.onesを用いることで，1で埋められた指定した形状(shape)をもつ配列を生成できます．
それ以外はnp.zerosと同様です．

.. .. ipython:: python
..     
..     np.ones(5)
..     np.ones((2, 3))

numpy.arange
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

np.arangeは，連番や等差数列を生成します．
使い方はPythonの組み込み関数rangeと似ており，以下のように引数を取ります．
なお，[]で囲んだ引数は省略できるということを意味します．

``arange([start,] stop, [step,][, dtype])``

startで指定した数からstopで指定した数まで，step間隔の数字列を生成します．
第２引数stop以外は省略ができますが，第３引数stepを指定するときは同時に第１引数startも設定する必要があります．
なお，第２引数stopだけを指定した場合は，初項0で交差1の等差数列を要素とするndarrayを生成します．


.. ipython:: python
    
    np.arange(10)
    np.arange(2, 10, dtype=np.float)
    np.arange(2, 3, 0.1)

numpy.linspace
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
np.linspaceは等差数列を生成する関数です．
同様の関数として先程紹介したnp.arangeがありますが，np.linspaceを使用すると指定した区間をN等分した配列を生成しているということが明確になります．

``linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)``

の形で使用し，生成する等差数列の始点と終点をstartとstopで指定します．
第３引数numで配列の長さを，第４引数endpointで終点を配列の要素として含むかどうかを指定します．

.. 等差数列の生成には上述したnp.arangeもありますが，

.. ipython:: python
    
    np.linspace(1., 4., 6)

numpy.eye, numpy.identity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
N×Nの単位行列を生成するには，np.eye, np.identityを用います．

.. ipython:: python

    np.eye(3)   #3×3の単位行列を生成
..    np.identity(5)  #5×5の単位行列を生成

配列の廃棄（ガベージコレクション）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

C言語やFortran言語のユーザーの方々には，配列の廃棄方法が気になるかと思います．
ここでは，Pythonにおける配列の廃棄に関して簡単に紹介します．

プログラムが動作する際には，以下の２つの順に処理が行われます．

1. プログラム処理に必要な情報をメモリ上に読み込む
2. 読み込んだ情報をもとに処理を実行する

読み込まれた情報は，処理が終わってしまうと不要になるため，C言語などではプログラマ自身がメモリの確保と解法を行うためのコードをわざわざ書いてメモリ管理を行っています．
一方で，Pythonには「ガベージコレクション」と呼ばれる，プログラムが確保したメモリ領域のうち，不要になった箇所だけを開放してくれる機能があります．
Pythonではガベージコレクションを標準機能として備えており，プログラマがメモリ管理のためにコードを書かなくても，自動的にメモリ領域の要不要をチェックし自動的にメモリ解放を行なってくれます．
ただし，本講座では取り扱いませんが，手動でガベージコレクションを行う方法もありますので，その詳細は公式ドキュメント [gc_en]_ [gc_jp]_ を参照して下さい．

.. [gc_en] https://docs.python.org/3.6/library/gc.html#module-gc
.. [gc_jp] https://docs.python.jp/3/library/gc.html
