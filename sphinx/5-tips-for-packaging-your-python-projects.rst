====================
`Fetchez le Python`_
====================

.. image:: http://1.gravatar.com/blavatar/107485281a5c5ceea5df5c78be3fd0d
    5?s=48&ts=1313810564

August 19, 2011 4:05 pm

..
    `5 tips for packaging your Python projects`_
    =============================================

`Python プロジェクトをパッケージングする5つの Tips`_
=====================================================

..
    Next week I am keynoting at `Pycon Japan`_, and one thing I will talk about
    is packaging of course. And in particular: what advice can I give my audience
    on how to package Python projects ***today*** ?

来週、私は `Pycon Japan`_ で基調講演を行います。そこで講演する話題の1つは、当然 packaging に関するものです。具体的に言うと、 **昨今** の Python プロジェクトをパッケージングするにあたって、参加者の方々へ私からどんなアドバイスをできるだろうか？

..
    This is a hard task, because we are in some kind of transitional state.

これは難しい話題です。というのは、いまはその過渡期だからです。

..
    Anyways, I wrote down a list of advices and removed everything that was
    dependent on the tools we did not release yet -- that's another part in my
    keynote.

とにかく、私はアドバイスのリストを書き出しました。またリリースしなかったツールに関する内容は削除しました。それは私の基調講演の別の話題になります。

..
    Here's a list. Most of them are not controversial. If you see something
    missing or want to rant about one, please comment.

これがそのリストです。ここに書き出したほとんどの内容について異論の余地はありません。もし不足している内容や何かしらツッコミがあればコメントをください。

..
    Tip # 1 -- Use a `PEP 386`_ compatible scheme for your versions
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tip #1 -- バージョンにあった `PEP 386`_ 互換にする
--------------------------------------------------

..
    Having several version scheme in our eco-system is pure madness. It breaks
    interoperability, and makes it impossible to write tools that handle versions
    properly. By using a `PEP 386`_-friendly scheme now, you are making your
    project future-proof !

パッケージ管理のエコシステムにおいて、複数のバージョン管理の仕組みを利用するのは、全く馬鹿げたことです。それは相互運用性を破綻させ、バージョンを適切に管理するツールを書くのが不可能になります。いまは `PEP 386`_ 互換の仕組みを利用することで、あなたのプロジェクトの将来が保証されます！

..
    PyPI already rejects any `Metadata 1.2`_ project that does not comply to this
    policy. You probably don't know this because no tools produces Metadata 1.2
    packages yet. But that's going to be the default in Python 3.3 and
    distutils2.

PyPI は、このポリシーに準拠しない全ての `Metadata 1.2`_ プロジェクトを排除しました。おそらく、あなたはこの事実を知らないでしょう。それは Metadata 1.2 のパッケージを作成するツールが全くなかったからです。しかし、このプロジェクトは Python 3.3 と distutils2 において標準になろうとしています。

..
    So long "devdevdev123" and "3765-2011-test" versions !

ようやく "devdevdev123" や "3765-2011-test" のバージョンとお別れだね！

..
    Tip #2 -- try to make setup.py as dumb and simple as possible
    -------------------------------------------------------------

Tip #2 -- できるだけシンプル且つひとまとめに setup.py を作る
------------------------------------------------------------

..
    **setup.py** is not your personal build system. I have seen crazy things in
    some projects. Remember that setup.py is used by installers for a lot of
    different tasks. Like getting the metadata fields of the project.

**setup.py** は、個々人のビルドシステムではありません。私はいくつかのプロジェクトでおかしなものを目の当たりにしました。インストーラーは、setup.py を様々な作業に利用することに注意してください。例えば、そのプロジェクトのメタデータの項目を取り出すといったことです。

..
    Here's a simple test: make sure ***"python setup.py -name"*** returns the
    name field without any external dependency, and without calling any function
    or method.

ここで単純なテストをしてみましょう。 **"python setup.py -name"** は、関数やメソッドを全く呼び出さず、さらに外部依存もなく、名前の項目を返すことを確認してください。

..
    Remember that *setup.py* is going away in Python 3.3 and distutils2, replaced
    by simple options in ***setup.cfg***. Don't be scared, you will still able to
    do complex tasks.

また *setup.py* は Python 3.3 と distutils2 ではなくなってしまうことも覚えておいてください。そして、setup.py は簡単なオプションで **setup.cfg** に置き換えられます。心配しなくても、依然として複雑な作業もできますからね。

..
    My advice: don't do anything else that feeding ***setup()*** with options in
    there. Put all your build things in another place, and if they need to be
    called by setup.py, make sure they are called only when needed.

私からのアドバイスとして、そこでオプションと共に **setup()** へ渡すものは何もありません。別の場所でビルドする全てのものを置いて、もしそれらのファイルが setup.py から呼び出されるなら、必要なときのみ呼び出されることを確認してください。

..
    Tip #3 -- Do not make any assumption about which installer will be used
    -----------------------------------------------------------------------

Tip #3 -- インストーラーが利用することを前提としない
----------------------------------------------------

..
    Make sure your ***setup.py*** can be run by a vanilla Python (==distutils).
    Even if you use setuptools or distribute, in most case you can manage to have
    it working in both tools. You can always tell the user to do extra steps
    manually if he needs to.

**setup.py** が普通の Python (== distutils) で実行できることを確認してください。もし setuptools や distribute を使っていたとしても、ほとんどの場合、両方のツールで動作するように管理できます。必要なら、ユーザーが手動で追加処理を行う方法もあります。

..
    Forcing the installation of an installer, by using the ***ez_setup*** script
    for instance, without asking, is a bit rude to the end-user. It's basically
    forcing the end user to use a new installer. If you do this in your setup.py,
    ask first !

ユーザーの確認なく、インスタンスのために **ez_setup** を使用することで、インストーラーによるインストールを強制することは、ユーザーに対してやや不親切です。それは基本的にエンドユーザーに新しいインストーラーの利用を強制しています。もし setup.py の内部でそうしたいなら、最初にユーザーへ問い合わせるようにしてください！

..
    Or simply tell the user "This project only works with the XXX installer --
    install it if you want. Aborting."

もしくは、単純にユーザーへ次のように伝えてください。「このプロジェクトは XXX インストーラーでのみ動作します。必要に応じてインストールするか中断してください。」

..
    Tip #4 -- Do not release unstable releases at pypi
    --------------------------------------------------

Tip #4 -- pypi へ不安定版 (unstable) をリリースしない
-----------------------------------------------------

..
    Our installers are not -*yet*- smart enough to prefer stable releases when
    they are asked to get a project at PyPI. That's how PyPI is built: every
    project has a directory with all releases and it's up to the installer to
    decide which one is the "latest". The only tool out there that's smart about
    it is zc.buildout.

私たちのインストーラーは、PyPI 上でプロジェクトを取得するように要求したときに、 *まだ* 安定板リリースを適切に選択できません。それは PyPI のビルド方法でもあります。全てのプロジェクトは、リリースした全てのディレクトリをもっています。そして、インストーラーに依存する形で、どれが "最新" であるかを決定します。PyPI の外側でそういったバージョンを管理する唯一の優れたツールが zc.buildout です。

..
    So when you push an alpha release or a rc release at PyPI, it's going to land
    in people environments unless they have mature processes to update their
    stuff -- or simply because they make the assumption that PyPI is where stable
    release go. So do not make assumptions about how your users are updating your
    project.

従って、PyPI 上でアルファリリースや RC リリースを行うとき、そういったリリースパッケージは、そのパッケージの機能を更新する仕組みがある場合を除いて、PyPI からユーザー環境へインストールされます。また、単純に PyPI は安定版リリースが置かれる場所だという前提があります。そのため、ユーザーがパッケージを更新する方法について前提条件を設けないようにしてください。

..
    Prefer another explicit channel for your beta testers. All installers know
    how to install from any url or directory.

ベータテスター向けには、明確に分けた別チャンネルを使ってください。全てのインストーラーは、任意の URL やディレクトリからインストールする方法を知っています。

..
    Tip #5 -- Be cautious about your data files
    -------------------------------------------

Tip #5 -- データファイルに気を付ける
------------------------------------

..
    Distutils or Distribute or Python itself have no way to explicitly make a
    difference between a doc file or a media file or a configuration file. They
    are all ***data files***. Worse, since they are no universal place for data
    files on the various OSes, people tend to treat their data files like Python
    modules so they are able to find them back on the target system without
    trouble.

Distutils や Distribute または Python そのものは、ドキュメントファイル、メディアファイル、設定ファイルといった、それぞれのファイルの違いを明示的に判別する方法をもっていません。こういったものは、全て **データファイル** です。さらに悪いことに、OS の違いによってそういったデータファイルの置かれる場所が違うために、パッケージングする人たちは、対象となる各システムで問題なくデータファイルが見つかるように、Python モジュールとしてデータファイルを扱おうとします。

..
    Yeah that's broken, and we've fixed it in 3.3. But until then, that's
    unfortunately the most protable way to do this. So what you can do is
    document clearly how you handle your data files and create a single function
    or module that reads them. That'll help the downstream maintainers to handle
    your project.

そう、それはおかしいです。私たちは、この問題を Python 3.3 で修正しました。しかし、この問題を解決できたとしても、最も移植性の高い方法として、データファイルを Python モジュールとして扱う人が多いでしょう。そこで、できることは、データファイルとそういったファイルを読み込む関数またはモジュールの操作方法を分かりやすくドキュメントにまとめることです。それがあなたのプロジェクトをメンテナンスしてくれる、ダウンストリームのメンテナたちを助けることになります。

.. _Fetchez le Python: http://tarekziade.wordpress.com/
.. _Python プロジェクトをパッケージングする5つの Tips: http://tarekziade.wordpress.com/2011/08/19/5-tips-for-packaging-your-python-projects/
.. _Pycon Japan: http://2011.pycon.jp/english-information
.. _PEP 386: http://www.python.org/dev/peps/pep-0386/
.. _Metadata 1.2: http://www.python.org/dev/peps/pep-0345/
