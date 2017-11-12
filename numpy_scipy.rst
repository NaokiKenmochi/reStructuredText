========
タイトル
========

セクション 1
============

xxx

セクション 2
============

xxx

サブセクション C
----------------

xxx

.. code-block:: python

  import numpy as np

  def get_xy(r, theta):
      """
      Returns x, y values from polar coordinate variable r (radial coordinate)
      and theta (angular coordinate).
      """
      x = r * np.cos(theta)
      y = r * np.sin(theta)
      return x, y
