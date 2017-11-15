NumPy/SciPyの利用
===============================

　それでは早速，NumPy/SciPyを使っていきましょう．
１章を参考にanacondaを使ってPythonをインストールした人は既にNumPyは入っていると思います．
そこで，まずはNumPyの有無を確認しましょう．
Pythonコンソールで
.. ipython:: python
  import numpy
  import scipy

と打ってみてエラーがなければ無事にインストールされています．
”No Module Named numpy”のようなエラーが出る場合はターミナルで以下のコマンドを入れてインストールして下さい．

.. code-block:: bash
  pip install numpy
  pip install scipy

　NumPyのインストールが完了したら，プログラム中で使用するためにimportします．
外部パッケージの使用に関する詳細は２章を参照してください．
NumPyをimportするには，プログラム冒頭で以下のように宣言します．

.. ipython:: python
  import numpy
  from numpy import *

.. ipython:: python
  from モジュール名 import *
というコードは，既にスコープに存在する変数を知らない間に上書きしてしまう恐れがあります．
本章ではNumPyの呼び出しは

.. ipython:: python
  import numpy as np

に統一してあります．
読者の皆さんにもnp.関数名での呼び出し記法を強く推奨します．

.. ipython:: python
  import numpy as np
  from numpy import array
  import scipy as sp

