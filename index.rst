=================================================
リファクタリングツールあれこれ
=================================================

.. revealjs:: リファクタリングツールあれこれ
 :title-heading: h2
 :subtitle: 〜 May the force be with you 〜
 :subtitle-heading: h5

 .. rv_small::

  PyCon JP 2014.

.. revealjs:: 資料

 .. image:: images/zvl_99mqw~gl.png

 http://tell-k.github.io/pyconjp2014/

.. revealjs:: 誰だお前は

 .. image:: images/tell-k.png
  :width: 200px

 .. raw:: html

  <ul>
  <li class="fragment">名前: tell-k</li>
  <li class="fragment">所属: <a href="http://www.beproud.jp/">beproud.inc</a></li>
  <li class="fragment">Twitter: <a href="https://twitter.com/tell_k">@tell_k</a></li>
  <li class="fragment">Github: <a href="https://github.com/tell-k">https://github.com/tell-k</a></li>
  <li class="fragment">特茶が大好き</li>
  </ul>

.. revealjs:: 動機

 .. raw:: html

  <ol>
  <li class="fragment">普段からpythonを書いてる</li>
  <li class="fragment">コードを書く時に気をつけてる事がある</li>
  <li class="fragment">その時に使ってるツールが結構ある</li>
  <li class="fragment">一度まとめてみたかった</li>
  </ol>

.. revealjs:: 
 :noheading:
 :data-background: images/_0~vb9q2y~4l.png

 .. raw:: html
 
  <div style="font-size:35px; padding:30px 10px 10px 10px; color: #fff; background-color: #000; filter:alpha(opacity=60); -moz-opacity:0.60; -khtml-opacity: 0.60; opacity:0.60;">
  <h3 style="color:white;line-height:60px;">
  リファクタリングの話はしない。<br />これからもするつもりはない。<br />いいね？ 
  </h3>
  </div>

.. revealjs:: 
 :noheading:

 .. image:: images/maln8-0saby2.png

.. revealjs::  対象者

 .. raw:: html

  <ol>
  <li class="fragment">プログラム初心者</li>
  <li class="fragment">Python入門したて</li>
  <li class="fragment">楽にコード書きたい</li>
  <li class="fragment">先輩の細かいコードレビューに嫌気がさしてきた</li>
  </ol>

.. revealjs:: 当該発表者

 .. raw:: html

  <ul>
  <li class="fragment">中途半端なVimmer</li>
  <li class="fragment">他のエディタ事情に疎い</li>
  </ul>

.. revealjs::  
 :data-background: http://blog-imgs-70.fc2.com/2/c/h/2chrising/entry_img_275.jpg

 .. raw:: html
 
  <div style="font-size:35px; padding:10px; color: #fff; background-color: #000; filter:alpha(opacity=60); -moz-opacity:0.60; -khtml-opacity: 0.60; opacity:0.60;">
  <h2 style="color:white;line-height:70px;">我が世には<br />Vim以外存在しない</h2>
  </div>

.. revealjs::  目次

 .. raw:: html

  <ol>
  <li class="fragment">コーディングスタイルを知る</li>
  <li class="fragment">自動整形に任せる</li>
  <li class="fragment">自動補完で楽する</li>
  <li class="fragment">リファクタリングツールを使う</li>
  <li class="fragment">コードメトリクスを見る</li>
  <li class="fragment">デッドコードを探す</li>
  <li class="fragment">まとめ</li>
  </ol>

.. revealjs:: 1. コーディングスタイルを知る


.. revealjs:: 
 :noheading:

 * PEP8 -- Style Guide for Python Code
 * PEP257 -- Docstring Conventions
 * Pythonコミュニティが推奨するスタイルが存在する

.. revealjs:: PEP 8 -- Style Guide for Python Code

 .. image:: images/52ya4pswctfy.png

 via http://legacy.python.org/dev/peps/pep-0008/

.. revealjs::
 :noheading:

 * インデントはスペース4
 * 行の長さ最大79文字
 * import分の順序
 * 命名スタイル
 * etc ..

.. revealjs:: PEP 257 -- Docstring Conventions

 .. image:: images/dfh_-vk-nh3c.png
 
 via http://legacy.python.org/dev/peps/pep-0257/

.. revealjs:: ツールでチェック

 * pep8 ... pepe8のチェック
 * pep257 ... pep257のチェック
 * pyflakes ... 文法エラーなどをチェック
 * flake8 ... pep8 + pyflakes

.. revealjs:: pep8

 * PEPの名前そのままのライブラリ
 * PEP8として準拠してくれるかチェックしてくれる
 * https://pypi.python.org/pypi/pep8

.. revealjs:: 
 :noheading:

 * 例えばこんなコード

 .. rv_code::

   import math, sys;

   def example1():
       ####This is a long comment. This should be wrapped to fit within 72 characters.
       some_tuple=(   1,2, 3,'a'  );
       some_variable={'long':'Long code lines should be wrapped within 79 characters.',
       'other':[math.pi, 100,200,300,9876543210,'This is a long string that goes on'],
       'more':{'inner':'This whole logical line should be wrapped.',some_tuple:[1,
       20,300,40000,500000000,60000000000000000]}}
       return (some_tuple, some_variable)
   def example2(): return {'has_key() is deprecated':True}.has_key({'f':2}.has_key(''));
   class Example3(   object ):
       def __init__    ( self, bar ):
        #Comments should have a space after the hash.
        if bar : bar+=1;  bar=bar* bar   ; return bar
        else:
                       some_string = """
                          Indentation in multiline strings should not be touched.
   Only actual code should be reindented.
   """
                    return (sys.path, some_string)

.. revealjs:: 
 :noheading:

 * 警告メッセージが表示

 .. rv_code::

   (pyconjp2014)$ pep8 example1.py
   example1.py:1:12: E401 multiple imports on one line
   example1.py:1:17: E703 statement ends with a semicolon
   example1.py:3:1: E302 expected 2 blank lines, found 1
   example1.py:4:5: E265 block comment should start with '# '
   example1.py:4:80: E501 line too long (83 > 79 characters)
   example1.py:5:15: E225 missing whitespace around operator
   example1.py:5:17: E201 whitespace after '('
   example1.py:5:21: E231 missing whitespace after ','
   example1.py:5:26: E231 missing whitespace after ','
   example1.py:5:31: E202 whitespace before ')'
   example1.py:5:33: E703 statement ends with a semicolon
   example1.py:6:18: E225 missing whitespace around operator
   example1.py:6:26: E231 missing whitespace after ':'
   example1.py:6:80: E501 line too long (84 > 79 characters)
   example1.py:7:5: E128 continuation line under-indented for visual indent

.. revealjs:: pep257

 * これもPEP名そのまま
 * PEP257として準拠してくれるかチェックしてくれる
 * https://pypi.python.org/pypi/pep257

.. revealjs:: 
 :noheading:

 .. rv_code::

  """   Here are some examples.

      This module docstring should be dedented."""

  def launch_rocket():
      """Launch
  the
  rocket. Go colonize space."""


  def factorial(x):
      '''

      Return x factorial.

      This uses math.factorial.

      '''
      import math
      return math.factorial(x)


  def print_factorial(x):
      """Print x factorial"""
      print(factorial(x))


  def main():
      """Main
      function"""
      print_factorial(5)
      if factorial(10):
          launch_rocket()


.. revealjs:: 
 :noheading:

 .. rv_code::

   (pyconjp2014) $ pep257 example2.py 
   example2.py:1 at module level:
           D209: Put multi-line docstring closing quotes on separate line
   example2.py:1 at module level:
           D208: Docstring is over-indented
   example2.py:5 in public function `launch_rocket`:
           D209: Put multi-line docstring closing quotes on separate line
   example2.py:5 in public function `launch_rocket`:
           D400: First line should end with '.', not 'h'
   example2.py:5 in public function `launch_rocket`:
           D205: Blank line missing between one-line summary and description
   example2.py:5 in public function `launch_rocket`:
           D207: Docstring is under-indented
   example2.py:11 in public function `factorial`:
           D300: Expected """-quotes, got '''-quotes
   example2.py:23 in public function `print_factorial`:
           D400: First line should end with '.', not 'l'
   example2.py:28 in public function `main`:
           D209: Put multi-line docstring closing quotes on separate line
   example2.py:28 in public function `main`:
           D400: First line should end with '.', not 'n'
   example2.py:28 in public function `main`:
               D205: Blank line missing between one-line summary and description

.. revealjs:: pyflakes

 * 文法エラー/未定義の変数・関数の使用等をチェック
 * コーディングスタイルのチェックではない
 * https://pypi.python.org/pypi/pyflakes

.. revealjs:: 
 :noheading:

 .. rv_code::

   import math
   import re
   import os
   import random
   import multiprocessing
   import grp, pwd, platform
   import subprocess, sys


   def foo():
       from abc import ABCMeta, WeakSet
       try:
           import multiprocessing
           print(multiprocessing.cpu_count())
       except ImportError as exception:
           print(sys.version)
       return math.pi

.. revealjs:: 
 :noheading:

 .. rv_code::

   example3.py:2: 're' imported but unused
   example3.py:3: 'os' imported but unused
   example3.py:4: 'random' imported but unused
   example3.py:5: 'multiprocessing' imported but unused
   example3.py:6: 'grp' imported but unused
   example3.py:6: 'platform' imported but unused
   example3.py:6: 'pwd' imported but unused
   example3.py:7: 'subprocess' imported but unused
   example3.py:11: 'ABCMeta' imported but unused
   example3.py:11: 'WeakSet' imported but unused
   example3.py:13: redefinition of unused 'multiprocessing' from line 5
   example3.py:15: local variable 'exception' is assigned to but never used

.. revealjs:: 全ての警告に対処すれば...

  * スタイルに準拠しつつ無駄な記述を排除できる。
  * ３つのツールをバラバラに使うのは。。

.. revealjs:: 
 :data-background: images/mrlb6sebeopl.png

 .. raw:: html

  <div style="font-size:35px; padding:30px 10px 10px 10px; color: #fff; background-color: #000; filter:alpha(opacity=60); -moz-opacity:0.60; -khtml-opacity: 0.60; opacity:0.60;">
    <h3 style="color:white;line-height:60px;">
    ぶっちゃけ面倒<br />いままで紹介したツールは一旦全て忘れてください
    </h3>
  </div>

.. revealjs:: flake8

  * pep8 + pyflakes
  * これ一つで大概のチェックが可能
  * 拡張機能あり
  * 循環的複雑度とか指定可能
  * https://pypi.python.org/pypi/flake8

.. revealjs:: 某社レビューガイドラインのファーストステップ

 .. image:: images/brciapvh0zzx.png
   :width: 150%

 訳) 最低flake8に通してからレビューに出そうや。な?

.. revealjs:: はい

.. revealjs:: flake8のメッセージ体系

 * E***/W***  pep8 のメッセージ
 * F***: pyflakes のメッセージ
 * C9**: McCabe complexity pluginのメッセージ
 * N8**: pep8-naming plugin のメッセージ

 * http://flake8.readthedocs.org/en/latest/warnings.html 

.. revealjs::

 .. image:: images/6-ebmt-qmsil.png
   :scale: 100%

.. revealjs:: flake8のTips

 * setup.cfgでカスタマイズ
 * どうしても無視したいヤツがいる
 * VCS Hookを使う
 * 拡張を使う

.. revealjs:: setup.cfgでカスタマイズ

 * チェック内容をカスタマイズ
 * 1行の最大長を120にしたい
 * 特定の個所だけチェック除外したい
 * プロジェクトのルートにsetup.cfgを用意

.. revealjs:: 
 :noheading:

 .. rv_code::

   [flake8]
   max-line-length = 120
   exclude = /apps/*/migrations/*

 * http://flake8.readthedocs.org/en/latest/config.html

.. revealjs:: どうしても無視したい

 * やむ得ない事情で無視したい
 * Djangoのsettingsのアスタリスクインポート

.. revealjs:: 
 :noheading:

 .. rv_code::

  from settings.base import *  # NOQA

 * # NOQAとコメントを書けばいい
 * やむ得ない場合ですよ。やむ得ない。

.. revealjs:: VCS Hookを使う

 .hgがあるディレクトリで以下のコマンドを叩く

 .. rv_code::

  $ flake8 --install-hook

 .hg/hgrcに勝手に追記してくれる

 .. rv_code::

  [hooks]
  commit = python:flake8.hooks.hg_hook
  qrefresh = python:flake8.hooks.hg_hook

  [flake8]
  complexity = 10
  strict = False
  ignore = None
  lazy = False

.. revealjs:: 拡張を使う

 * pep257のチェックにも対応させたい
 * flake8-docstringという拡張を使う
 * https://pypi.python.org/pypi/flake8-docstrings/0.1.0

.. revealjs:: 
 :noheading:

 .. rv_code::

  $ pip install flake8_docstrings

 やる事はこれだけ

 .. rv_code::

  $ flake8 example3.py 
  example3.py:1:1: D100  Docstring missing
  example3.py:2:1: F401 're' imported but unused
  example3.py:3:1: F401 'os' imported but unused
  example3.py:4:1: F401 'random' imported but unused
  example3.py:5:1: F401 'multiprocessing' imported but unused
  example3.py:6:1: F401 'grp' imported but unused
  example3.py:6:1: F401 'platform' imported but unused
  example3.py:6:1: F401 'pwd' imported but unused
  example3.py:6:11: E401 multiple imports on one line
  example3.py:7:1: F401 'subprocess' imported but unused
  example3.py:7:18: E401 multiple imports on one line
  example3.py:10:1: D103  Docstring missing

 Dで始まるメッセージが表示されるようになる。

.. revealjs:: 他には?

 * pep8-naming -> クラス・変数などの命名規則のチェック
 * flake8-todo -> # TODO を拾ってくれる

.. revealjs:: コーディングスタイルについて

 * コミュニティ推奨のスタイルが存在する
 * とりあえず始めるにはflake8がおすすめ 

.. revealjs:: 2. 自動整形に任せる

 * チェックツールは沢山ある
 * ただし直すのは
 
.. revealjs:: THE 人の手

.. revealjs:: 辛い

 * golangにはgofmtていう便利なツールがあってだな。

.. revealjs:: Pythonでもあるよ

 * autopep8 
 * autoflake
 * その他

.. revealjs:: autopep8

 * pep8に従って整形してくれる
 * gofmtのような存在
 * これだけでも大分楽になる
 * https://pypi.python.org/pypi/autopep8

.. revealjs:: 

 Before

 .. rv_code::

  import math, sys;

  def example1():
      ####This is a long comment. This should be wrapped to fit within 72 characters.
      some_tuple=(   1,2, 3,'a'  );
      some_variable={'long':'Long code lines should be wrapped within 79 characters.',
      'other':[math.pi, 100,200,300,9876543210,'This is a long string that goes on'],
      'more':{'inner':'This whole logical line should be wrapped.',some_tuple:[1,
      20,300,40000,500000000,60000000000000000]}}
      return (some_tuple, some_variable)
  def example2(): return {'has_key() is deprecated':True}.has_key({'f':2}.has_key(''));
  class Example3(   object ):
      def __init__    ( self, bar ):
       #Comments should have a space after the hash.
       if bar : bar+=1;  bar=bar* bar   ; return bar
       else:
                      some_string = """
                         Indentation in multiline strings should not be touched.
  Only actual code should be reindented.
  """
                           return (sys.path, some_string)

 コマンド

 .. rv_code::

  $ autopep8 --in-place --aggressive --aggressive exmaple1.py
  
.. revealjs:: 

 After

 .. rv_code::

   import math
   import sys

   def example1():
       # This is a long comment. This should be wrapped to fit within 72
       # characters.
       some_tuple = (1, 2, 3, 'a')
       some_variable = {
           'long': 'Long code lines should be wrapped within 79 characters.',
           'other': [
               math.pi,
               100,
               200,
               300,
               9876543210,
               'This is a long string that goes on'],
           'more': {
               'inner': 'This whole logical line should be wrapped.',
               some_tuple: [
                   1,
                   20,
                   300,
                   40000,
                   500000000,
                   60000000000000000]}}
       return (some_tuple, some_variable)


   def example2():
       return ('' in {'f': 2}) in {'has_key() is deprecated': True}


   class Example3(object):

       def __init__(self, bar):
           # Comments should have a space after the hash.
           if bar:
               bar += 1
               bar = bar * bar
               return bar
           else:
               some_string = """
                          Indentation in multiline strings should not be touched.
   Only actual code should be reindented.
   """
               return (sys.path, some_string)

.. revealjs:: tips

  * 特定のディレクトリ以下を一発置換
  * --aggresiveを重ねるとアグレッシブになる

.. revealjs:: autoflake

  * pyflakesが出すようなメッセージに対応
  * 利用されてないimport文の削除
  * 利用されてない変数の削除
  * https://pypi.python.org/pypi/autoflake

.. revealjs:: 

  Before

  .. rv_code::

    import math
    import re
    import os
    import random
    import multiprocessing
    import grp, pwd, platform
    import subprocess, sys

    hoge = "hoge"

    def foo():
        from abc import ABCMeta, WeakSet
        fuga = "fuga"
        try:
            import multiprocessing
            print(multiprocessing.cpu_count())
        except ImportError as exception:
            print(sys.version)
        return math.pi

  コマンド

  .. rv_code::

    $ autoflake --in-place --remove-unused-variables example3-autoflake.py

.. revealjs:: 

  After

  .. rv_code::

    import math
    import sys

    hoge = "hoge" # <= モジュールスコープの変数は消さないでくれる

    def foo():
        try:
            import multiprocessing
            print(multiprocessing.cpu_count())
        except ImportError:
            print(sys.version)
        return math.pi

.. revealjs:: その他

 * docformatter ... pep257に併せてdocstringを整形

   * https://pypi.python.org/pypi/docformatter 

 * eradicate ... 不要なコメントアウトを消す

   * https://pypi.python.org/pypi/eradicate

 * unify ... シングルクォーテーションに統一

   * https://pypi.python.org/pypi/unify

 * pyformat ... autopep8, autoflake, docformatter, unify 統合

   * https://pypi.python.org/pypi/pyformat

 * 細かな設定ができなさそう


.. revealjs:: 自動整形について

 * スタイルに合わせる時間を軽減できる
 * 一括整形とかやると割とスキッとする
 * 他人のコードに寛容になれる
 * ただしツールを過信しすぎない
 * PEP8とかPEP257とか凄い忘れる

.. revealjs:: 3. 自動補完で楽する

 * コード書いてる時にやっぱり欲しい
 * 昔は python-complete 今は jedi  
 * virtualenvにも対応しているの地味に嬉しい
 * https://pypi.python.org/pypi/jedi
 * https://github.com/davidhalter/jedi-vim

.. revealjs:: ドット区切りで補完

 .. image:: images/5gg-vf795qdg.png

 * ドット区切りの時に候補の表示
 * 精度がいい
 * OFFにすることも可能

.. revealjs:: 関数ヒントの表示

 .. image:: images/3vwi6tbmg5l8.png

 関数の括弧にきたら勝手にでる

.. revealjs:: Docstringの表示

 .. image:: images/i9h-531qz5-9.png

 * 対象にカーソルをおいて、「Shift + K」

.. revealjs:: 定義場所のjump

 .. image:: images/xjen4s60vv7u.png

 * 対象にカーソルをおいて、「<leader> + d」
 * vimのTabで勝手に開く

.. revealjs:: その他には？

 * <leader> + d  対象のリネーム
 * <leader> + n  利用場所の一覧を別ウィンドウに表示
 * <leader> + g  変数等の初期化場所にjump?

 * 若干じゃじゃ馬になる時があって旨く使いこなせてない

 .. rv_note:: 
  
   http://jedidjah.ch/code/2013/1/19/why_jedi_not_rope/

.. revealjs:: 自動補完について

 * コードを書く手間が省ける
 * コードリーディングが捗る

.. revealjs:: 4. リファクタリングツールを使う

 * Ropeというツールを利用する
 * 出来る事が若干jediと被る
  
   * 定義位置へのjump
   * リネーム

 * リファクタリング作業に特化してる
 * https://pypi.python.org/pypi/rope
 * https://github.com/python-rope
 * http://methane.hatenablog.jp/entry/2013/01/11/ropevimをインストールしてみる

.. revealjs:: 最初にやる事

 * ropeprojectを作る
 * Vimを開いて「:RopeProjectConfig」と打つ
 * Ropeプロジェクトを作成するか聞かれる
 
 .. image:: images/cjodp38za3yt.png
  :width: 90%


.. revealjs:: 設定ファイル

 .ropeproject/config.py

 .. image:: images/8atg~5cg7zvd.png

.. revealjs:: 設定ファイル

 * .ropeproject/config.py が設定ファイルにパスを通す

 .. rv_code::

  # プロジェクトのソースのパスを指定する
  # 'src/my_source_folder' for instance.
  prefs.add('source_folders', 'django_apps')

  # Virtualenvにパスを通す
  # You can extend python path for looking up modules
  prefs.add('python_path', '/Users/hoge/.virtualenvs/pyconjp2014/lib/python2.7/site-packages')

 .. rv_note::


.. revealjs:: プロジェクトのロード

 * Vimを開いて「:RopeAnalyzeModules」とタイプ
 * 対処ソース情報とかを.ropeproject/objectdbに蓄えてくれる
 * 動作が変とか思ったらとりあえず一回やっとく

 .. image:: images/uod0w05a0vn0.png

.. revealjs:: 使ってみる

 * RopeRename .... 変数/関数/クラスをリネーム
 * RopeMove .... 変数/関数/クラス等の移動
 * RopeAutoImport .... import文の自動追記
 * RopeFindImplementations .... 実装の検索
 * RopeChangeSignature .... 関数の引数をその場で変更
 * RopeExtractMethod .... メソッドの抽出
 * RopeUndo/Redo ... 取り消し/やり直し

.. revealjs:: RopeRename

 * プロジェクト全体の変数/関数/クラスなどをリネーム
 
 spam.py

 .. rv_code::
 
  def get_spam(num=1):
      return "spam x {}".format(num)

 main.py

 .. rv_code::
 
  from spam import get_spam

  print get_spam() # => "spam x 1"

 main.py の「get_spam()」にカーソルを置いて「:RopeRename」

.. revealjs::
 :noheading:

 新しい名前を聞かれる

 .. image:: images/j-9unv3oul_w.png
    :width: 90%

.. revealjs::
 :noheading:

 1を入力してプレビューを確認する。

 .. image:: images/0o5ex4s5a6tj.png
    :width: 90%

.. revealjs::
 :noheading:

 .. image:: images/c~j3fz7p~r5p.png
    :width: 60%

 * diffを表示して事前に確認する事ができる

.. revealjs:: RopeMove

 * プロジェクト全体の変数/関数/クラスなど移動
 
 spam.py

 .. rv_code::
 
  def get_spam(num=1):
      return "spam x {}".format(num)

 main.py

 .. rv_code::
 
  from spam import get_spam

  print get_spam() # => "spam x 1"

 * spam.pyのget_spamをmain.py移動

.. revealjs:: 
 :noheading:

 .. image:: images/l5a~pgwdpmqr.png
    :width: 90%

.. revealjs:: 
 :noheading:

 丸っと移動される。不要なimportも削除

 .. image:: images/gs6mkrvbgasp.png
    :width: 60%

.. revealjs:: RopeAutoImport

 * インポート文を自動で追記してくれる
 
 spam.py

 .. rv_code::

   def get_spam(num=1):
      return "spam x {}".format(num)

 main.py

 .. rv_code::
 
   print get_spam() # => "spam x 1"

 * import文が書いてない
 * get_spam()にカーソルを置いて「:RopeAutoImport」

.. revealjs:: 
 :noheading:
 
 .. image:: images/0~j00-gnd6pl.png
    :width: 90%

.. revealjs:: RopeFindImplementations

 * 継承先の実装個所を探してくれる
 
 spam.py

 .. rv_code::
   # BaseSpamという抽象クラスを用意
   from abc import ABCMeta

   class BaseSpam:
       __metaclass__ = ABCMeta

       @abstractmethod
       def get_spam(self):
          """ must implement. """

.. revealjs:: 
 :noheading:

 main.py

 .. rv_code::
   from spam import BaseSpam

   class MySpam(BaseSpam):

       def get_spam(self, num=1):
           return "my spam x {}".format(num)

   class YourSpam(BaseSpam):

       def get_spam(self, num=1):
           return "your spam x {}".format(num)

 * BaseSpamのget_spamが実装してある個所を探したい
 * BaseSpamのget_spamにカーソルを置いて「:RopeFindImplementations」

.. revealjs:: 
 :noheading:
 
 .. image:: images/k73wf7060k3a.png
    :width: 90%

 * 実装してある個所を見つけてくれる

.. revealjs:: RopeChangeSignature

 * 例えば関数の引数を急に変えたくなった時に使う
 * get_spamにpriceを追加したい

 spam.py

 .. rv_code::
 
  def get_spam(num=1):
      return "spam x {}".format(num)

 main.py

 .. rv_code::
 
  from spam import get_spam

  print get_spam() # => "spam x 1"

 * get_spamにカーソルを置いて「:RopeChangeSignature」

.. revealjs:: 
 :noheading:

 .. image:: images/heyfcc0i6den.png
    :width: 90%

 引数にpriceを追加

.. revealjs:: 
 :noheading:

 .. image:: images/te91r-iclefr.png
    :width: 90%

 * 他の呼び出しに影響が無いように配慮してくれる

.. revealjs:: RopeExtractMethod

 * 選択範囲をよしなにメソッド化
 * 以下のif文を丸っとメソッドにする

 .. rv_code::
    
   # 〜 省略 〜

   class Customer(object):

       def statement(self):
           total_amount = 0
           frequent_renter_points = 0
           rentals = self._rentals
           result = "Rental Record for {name}\n".format(name=self.name)

           for each in rentals:
               this_amount = 0

               # ここのif文全体を丸っとメソッドにする
               if each.movie.price_code == Movie.REGULAR:
                   this_amount += 2
                   if each.days_rented > 2:
                       this_amount += (each.days_rented - 2) * 1.5

               elif each.movie.price_code == Movie.NEW_RELEASE:
                   this_amount += each.days_rented * 3

               elif each.movie.price_code == Movie.CHILDRENS:
                   this_amount += 1.5
                   if each.days_rented > 3:
                       this_amount += (each.days_rented - 3) * 1.5

   # 〜 省略 〜

.. revealjs:: 
 :noheading:

 .. image:: images/5l_y7j2ubcbi.png
    :width: 90%

 * 抽出したい範囲を選択 -> RopeExtractMethodを実行
 * メソッド名は「_amount_for」

.. revealjs:: 
 :noheading:

 .. image:: images/e7j3bh_-dhx4.png
    :width: 90%

 * if文を削除し一つのメソッド呼び出しになった

.. revealjs:: 
 :noheading:

 .. image:: images/euo4x~bl9bzu.png
    :width: 90%

 * 文脈を呼んで、引数をよしなにしてくれるのが嬉しい

.. revealjs:: RopeUndo/RopeRedo

 * Ropeでの変更内容は履歴として保存
 * .ropeproject/history 
 * 直前の操作を取り消し or やり直し可能
 * 大量ファイルのRenameに失敗しても安心

.. revealjs:: 
 :noheading:
 
 RopeUndo

 .. image:: images/yv1j37mcbcte.png
    :width: 70%

 RopeRedo

 .. image:: images/yvtorvcga9_4.png
    :width: 70%

.. revealjs:: ショートカット

 .. image:: images/lry-vq9fwgyq.png

.. revealjs:: Ropeについて

  * IDEに見劣りしない編集作業が可能になる。
  * 事前に変更内容をプレビューは良い
  * 慣れるまで多少時間が掛かった
  * 使い慣れてるエディタで作業できるのは嬉しい
  * こいつもじゃじゃ馬になることがある。

.. revealjs:: 5. コードメトリクスを見る

  * 定量的な見方でリファクタリング対象を探す
  * 複雑になりすぎてないかチェック
  * 潜在的にバグになりそうな所を特定しやすくなる

.. revealjs:: ところで

  * Code Climateて知ってますか？
  * コードメトリクス等で自動でレビューしてくれる

  .. image:: https://s3.amazonaws.com/dandemeyere_production/2014/feed.jpg

.. revealjs:: （・ω・）いいなぁ 

.. revealjs:: 

   .. image:: images/i-~it~ub7iu9.png

.. revealjs:: 
  :data-background: images/i-~it~ub7iu9.png

  .. raw:: html
 
    <div style="font-size:35px; padding:10px; color: #fff; background-color: #000; filter:alpha(opacity=60); -moz-opacity:0.60; -khtml-opacity: 0.60; opacity:0.60;">
    <h2 style="color:white;">
    まだダメです！
    </h2>
    </div>

.. revealjs:: Radon
  
 .. image:: http://ecx.images-amazon.com/images/I/518VNBK1WCL.jpg

.. revealjs:: Radon

 * メトリクスをカジュアルに見せてくれる
 * Code ClimateのようにABC評価を付けてくれる

   * Cyclomatic Complexity
   * Maintainability Index

 * https://pypi.python.org/pypi/radon

.. revealjs:: Cyclomatic Complexity

 * 循環的複雑度
 * Thomas J. McCabe という人が考案
 * 幾つか決定する方法がある
 * もっとも簡単な方法は「閉じたループ + 1」
 * http://ja.wikipedia.org/wiki/循環的複雑度

.. revealjs:: 簡単な例

  .. rv_code::

   def fizzbuzz(max_number):
       ret = []
       for i in range(1, max_number + 1): # <= +1
           if i % 15 == 0:                # <= +1
               ret.append('FizzBuzz')
           elif i % 5 == 0:               # <= +1
               ret.append('Buzz')
           elif i % 3 == 0:               # <= +1
               ret.append('Fizz')
           else:
               ret.append(i)
       return ret

   print fizzbuzz(max_number=15)
   # => [1, 2, 'Fizz', 4, 'Buzz', 'Fizz', 7, 8, 'Fizz',
   #    'Buzz', 11, 'Fizz', 13, 14, 'FizzBuzz']
  
  * この場合 制御構文は全部4つ
  * 4 + 1 = 5 <= 循環的複雑度

.. revealjs:: Radonの評価

  * Radonは6段階に分けて複雑度を評価

  .. image:: images/phyq0if-1d48.png
     :width: 90%

.. revealjs:: Radon CC

  .. image:: images/~7h9ti469jr7.png
     :width: 90%

  * fizzbuzzは循環的複雑度「5」なのでA評価
  * 後一個分岐が増えたら？

.. revealjs:: 
 :noheading:

 わざと複雑度をあげてみる

 .. rv_code::
   
   def fizzbuzz(max_number):
       ret = []
       for i in range(1, max_number + 1): 
           if i % 15 == 0:                
               ret.append('FizzBuzz')
           elif i % 5 == 0:
               ret.append('Buzz')
           elif i % 3 == 0:
               ret.append('Fizz')
           elif i % 2 == 0:         # <= 追加 +1
               pass  # do nothing
           else:
               ret.append(i)
       return ret
  
 複雑度が6になるので「B」になる

 .. image:: images/_vxdmxoq2vcj.png
    :width: 70%

.. revealjs::

 rand ccコマンドのオプションは色々ある

 .. image:: images/mltprc411x-b.png
 
 via http://qiita.com/mima_ita/items/5010aaa6808b7290d68d

.. revealjs:: Maintainability Index

 * 保守容易性指数
 * コードの相対的な保守容易性を表す 
 * 0 〜 100の数値 最高評価が100
 * 複雑度とコードの行数などを元に計算

.. revealjs:: 
 :noheading:

 .. image:: images/d57bi3e12fnw.png
 
 via http://qiita.com/mima_ita/items/5010aaa6808b7290d68d

.. revealjs:: 歪んだ楽しみ方

 * 良さげなOSSを見つける
 * Sentryという著名なログトラッキングツール
 * 実行しみてる

.. revealjs:: 
 :noheading:
 
 .. image:: images/mu7ayogeisn0.png

.. revealjs:: 
 :data-background: images/7rmin~k-pkx5.png

 .. raw:: html

   <div style="font-size:35px; padding:10px; color: #fff; background-color: #000; filter:alpha(opacity=60); -moz-opacity:0.60; -khtml-opacity: 0.60; opacity:0.60;">
   <h2 style="color:white;">
   オールA 評価
   </h2>
   <h3 style="color:white;">
   普通に凄かったw
   </h3>
   </div>

.. revealjs:: その他

 * Raw Metrics ... 単純な行数、コメント、空行なども表示
 * radon raw コマンドで取得可能

.. revealjs:: コードメトリクスについて

 * 評価が高ければ必ずしも良いというものではない。
 * コードを書く、リファクタリングをする時間は有限。
 * 指標を参考にしつつ、適切な改善対象を見つける。
 * Radonは素早く結果が見れるのでオススメ

.. revealjs:: 6. デッドコードを探す

 * 大規模なプロジェクトとかにアサイン
 * 完全に死んでるコードを排除したい
 * 一つ一つ調べるのではなくて、一括で調べられると良い
 * http://stackoverflow.com/questions/9524873/finding-dead-code-in-large-python-project

.. revealjs:: 銀の弾丸は無かた

.. revealjs:: vulture

 .. image:: images/knzi4kv2s57v.png

.. revealjs:: vulture
 :noheading:

 * Find dead code
 * 静的解析して死んでるコードを一括で見つけてくれる
 * https://pypi.python.org/pypi/vulture

.. revealjs:: 
 :noheading:

 .. image:: images/lrs2ndn-uh03.png
   :width: 90%

.. revealjs::

 .. rv_code::

  class ProjectAdmin(admin.ModelAdmin):                                                               
      list_display = ('full_slug', 'owner', 'platform', 'status', 'date_added')                       
      list_filter = ('status', 'platform', 'public')                                                  
      search_fields = ('name', 'owner__username', 'owner__email', 'team__slug',                       
                       'team__name', 'slug')                                                          
      raw_id_fields = ('owner', 'team')                                                               
                                                                                                      
      def full_slug(self, instance):                                                                  
          if not instance.team:                                                                       
              slug = instance.slug                                                                    
          else:                                                                                       
              slug = '%s/%s' % (instance.team.slug, instance.slug) 
  
 * 確かにコード中では使ってないけど使ってる
 * virtualenvの中とかそういう所までは考慮してくれない
 * ただ実行結果が出てくるのは早い

.. revealjs:: Coverageを取る

 * 全てのコードを動かせるなら
 * Coverageで計測できる

.. revealjs:: 例えばDjango

 例えばDjango

 .. rv_code::

  coverage run manage.py runserver --noreload

 アプリ動作テストみたいなのを一通りする

 .. rv_code::

  coverage report -m

 カバレッジを見る

 .. rv_code::
  
  [run]
  omit = *migrations*,*.virtualenvs*

 除外対象を .coveragercに書くと良い

.. revealjs:: 
 :noheading:

 .. rv_code::

   Name                                     Stmts   Miss  Cover   Missing
   ----------------------------------------------------------------------
   apps/__init__                                0      0   100%
   apps/account/__init__                        0      0   100%
   apps/account/api                            68     56    18%   23, 31-84, 91-111, 120-129, 133-141
   apps/account/forms                          11      0   100%
   apps/account/models                         63     20    68%   94, 100-102, 108-113, 118-127
   apps/account/urls                            5      0   100%
   apps/account/validators                      5      0   100%
   apps/account/views                         128     67    48%   38-65, 70-96, 107-114, 123-124, 129, 142-143, 146-149, 152, 155, 17
   apps/comment/__init__                        0      0   100%
   apps/comment/admin                           3      0   100%
   apps/comment/api                            18     11    39%   15, 26-28, 34-40
   apps/comment/cache                           5      2    60%   8, 16
   apps/comment/forms                           5      0   100%
   apps/comment/models                         28      7    75%   15-16, 35, 38-41, 53
   apps/comment/sitemaps                       26     12    54%   35-38, 41, 47-62

 * 起動時で自動でロードされるもの
 * 操作した分動作したもの
 * これらはカバレッジに反映される

.. revealjs:: デッドコードを探す

 * 簡単には行かない
 * リファクタリングしつつ、適宜掃除するのが大事 
 * 紹介したツールは参考程度に見るには良い

.. revealjs:: まとめ

 .. raw:: html

  <ol>
  <li class="fragment">コーディングスタイルを知る</li>
  <li class="fragment">自動整形に任せる</li>
  <li class="fragment">自動補完で楽する</li>
  <li class="fragment">リファクタリングツールを使う</li>
  <li class="fragment">コードメトリクスを見る</li>
  <li class="fragment">デッドコードを探す</li>
  </ol>

.. revealjs:: まとめ

 .. raw:: html

  <ol>
  <li class="fragment">コードを書く時間は有限</li>
  <li class="fragment">ツールに任せる</li>
  <li class="fragment">紹介しなかったツールも沢山</li>
  <li class="fragment">手に馴染むにツールを手に入れる</li>
  <li class="fragment">注力すべき作業に集中しよう</li>
  </ol>

.. revealjs:: 謝辞

 * ツールの開発者さん
 * Webにいつも書いてくれる皆さん
 * いつもありがとうざいます m(_ _)m

.. revealjs:: 
 :noheading:

 .. raw:: html

  <div style="text-align:left; border: 5px solid #0043f5; padding:50px 30px; border-radius: 10px; -webkit-border-radius: 10px; -moz-border-radius: 10px; font-size:80%; font-family: MS Sans Serif, Arial, sans-serif;">
    <p style="margin-bottom:20px;">
    ビープラウド広告
    </p>

    <p style="margin-bottom:25px;">
    この記事はビープラウド勤務中に書かれたかもしれない。
    </p>

    <p style="margin-bottom:25px;">
    ここから採用された場合は、社長およびCTOから特茶のバックマージンが発生するじゃないかなって妄想してる。
    </p>

    <p style="margin-bottom:25px;">
    ビープラウドは本物のPythonプログラマーを募集しています。
    </p>

    <p style="margin-bottom:25px;">
    <a href="http://jobs.beproud.jp/">採用情報&#65372;株式会社ビープラウド</a>
    </p>

    <p>
    CC BY-ND 4.0: <a href="http://creativecommons.org/licenses/by-nd/4.0/deed.en_US">Creative Commons &#8212; Attribution-NoDerivatives 4.0 International &#8212; CC BY-ND 4.0</a>
    </p>

  </div>

.. revealjs:: ご清聴ありがとうございました

