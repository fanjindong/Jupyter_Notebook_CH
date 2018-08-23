.. _htmlnotebook:

The Jupyter Notebook
====================

Introduction
------------

这个notebook扩展了基于控制台交互计算的新方向，提供了一个基于web的应用程序来捕捉整个计算过程：
开发、记录和执行代码，以及输出结果。这个notebook包含了两个组件：


**A web application**: a browser-based tool for interactive authoring of
documents which combine explanatory text, mathematics, computations and their
rich media output.

**Notebook documents**: a representation of all content visible in the web
application, including inputs and outputs of the computations, explanatory
text, mathematics, images, and rich media representations of objects.

.. seealso::

    参阅 :ref:`installation guide <jupyter:install>` 解释如何安装notebook及管理依赖。


web application的主要特性
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* In-browser editing for code, with automatic syntax highlighting,
  indentation, and tab completion/introspection.

* The ability to execute code from the browser, with the results of
  computations attached to the code which generated them.

* Displaying the result of computation using rich media representations, such
  as HTML, LaTeX, PNG, SVG, etc. For example, publication-quality figures
  rendered by the matplotlib_ library, can be included inline.

* In-browser editing for rich text using the Markdown_ markup language, which
  can provide commentary for the code, is not limited to plain text.

* The ability to easily include mathematical notation within markdown cells
  using LaTeX, and rendered natively by MathJax_.



.. _MathJax: https://www.mathjax.org/


Notebook 文档
~~~~~~~~~~~~~~~~~~
Notebook documents contains the inputs and outputs of a interactive session as
well as additional text that accompanies the code but is not meant for
execution.  In this way, notebook files can serve as a complete computational
record of a session, interleaving executable code with explanatory text,
mathematics, and rich representations of resulting objects. These documents
are internally JSON_ files and are saved with the ``.ipynb`` extension. Since
JSON is a plain text format, they can be version-controlled and shared with
colleagues.

.. _JSON: https://en.wikipedia.org/wiki/JSON

Notebooks may be exported to a range of static formats, including HTML (for
example, for blog posts), reStructuredText, LaTeX, PDF, and slide shows, via
the nbconvert_ command.

Furthermore, any  ``.ipynb`` notebook document available from a public
URL can be shared via the `Jupyter Notebook Viewer <nbviewer>`_ (nbviewer_).
This service loads the notebook document from the URL and renders it as a
static web page.  The results may thus be shared with a colleague, or as a
public blog post, without other users needing to install the Jupyter notebook
themselves.  In effect, nbviewer_ is simply nbconvert_ as
a web service, so you can do your own static conversions with nbconvert,
without relying on nbviewer.



.. seealso::

    :ref:`Details on the notebook JSON file format <nbformat:notebook_file_format>`


Notebooks的数据安全
~~~~~~~~~~~~~~~~~~~~~

因为你在web浏览器上使用Jupyter，一些人会关系数据敏感问题。
然而，如果你遵循安装标准`install instructions <https://jupyter.readthedocs.io/en/latest/install.html>`_,
Jupyter事实上是运行在你自己的电脑上。
如果URL地址栏开始于 ``http://localhost:`` 或者``http://127.0.0.1:``, 
这是你的电脑作为这个服务器。Jupyter 不会发送你的数据到任何地方，而且它是开源的，
也欢迎任何人监督我们在这方面确实是诚实的。

你也可以使用远程的Jupyter:
你的公司或者大学应该帮你配置这个服务器给你，例如，
如果你想要保证工作的的某些敏感数据的安全，你应该要求你的IT或数据安全部门来帮助你。

我们为了确保你浏览器的其他页面或者和你共用一个电脑的其他用户不能够访问你的notebook服务。可以看
:ref:`server_security` 来获取更多信息。


开始notebook服务
----------------------------

你可以运行一个notebook服务，通过使用如下命令行::

    jupyter notebook

这将会在你的控制台打印关于这个notebook服务的一些信息，
然后你就可以根据这个web应用的URL来打开一个web浏览器(默认为
``http://127.0.0.1:8888``).

这个web应用Jupyter notebook的登陆页面,
the **dashboard**,
展示了这个notebook在当前目前下，可获取的notebooks文件。
(默认情况下，这个目录是notebook服务开启的地方).

你现在可以从这个dashboard，通过``New Notebook``按钮来创建一个新的notebooks文件，
或者通过点击文件名来打开一个已经存在的文件。
你也可以拖放 ``.ipynb`` notebooks和标准的``.py`` Python源代码文件到notebook列表。

当从命令行开始一个notebook服务，你可以打开一个具体notebook目录，通过这个dashboard，
如 ``jupyter notebook
my_notebook.ipynb``. 如果你没有变更扩展名，则默认扩展名为 ``.ipynb``

当你在一个打开的notebook应用内，这个`File | Open...`菜单选项将能够打开一个新的dashboard标签页，
将允许你从另外的notebook目录或者创建一个新的notebook来打开另外一个notebook。


.. note::
   你可以在同一时间开启多个notebook服务，如果你想要在不同的目录下运行notebooks，
   默认第一个notebook服务端口为8888，之后启动的notebook服务端口，会搜索之前服务的端口在其附近确定。
   你可以通过``--port`` 选项来手动指定服务端口

创建一个新的notebook文件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

一个新的notebook可以再任何时间被创建，通过这个dashboard或者使用这个
:menuselection:`File --> New` 菜单选项。
在同一个目录下创建一个新的notebook，将会打开一个新的浏览器标签。
他将反馈为dashboard界面上的notebook列表里面的新条目。

.. image:: _static/images/new-notebook.gif


打开notebooks
~~~~~~~~~~~~~~~~~
打开的笔记本只有一个连接到kernel的交互式会话，它将执行用户发送的代码并传回结果。
如果Web浏览器窗口关闭，此kernel将保持活动状态，
并从dashboard重新打开同一笔记本,会将Web应用程序连接到同一kernel。
在dashboard中，具有活动kernel的笔记本旁边有一个关机按钮，而没有活动kernel的笔记本在其位置有一个删除按钮。

其他客户端可能会链接到同一个kernel上。当每个kernel被启动，则这个notebook服务会打印到终端一条信息::

    [NotebookApp] Kernel started: 87f7d2c0-13e3-43df-8bb8-1bd37aaf3373
这个长字符串是kernel的ID，通过它足以获取连接到kernel所需的信息。
如果这个notebook使用的是IPython kernel，你可以通过执行``%connect_info`` :ref:`magic
<magics_explained>`，来看到这些链接数据，它将会打印同样的ID以及其他方面的信息。

例如，您可以通过传递ID的一部分来手动启动从命令行连接到同一kernel的Qt控制台::

    $ jupyter qtconsole --existing 87f7d2c0

没有ID时，``--existing``将连接到最近启动的kernel。

在使用IPython kernel情况下，你可以在你的notebook打开Qt控制台运行``%qtconsole``
:ref:`magic <magics_explained>`来链接到同一个kernel。

.. seealso::

    :ref:`ipythonzmq`

Notebook 的用户界面
-----------------------

当你创建一个新的notebook文件，你将会看到
**文件名：notebook name**, 一个 **菜单栏：menu bar**, 一个 **工具栏：toolbar** 以及一个空的 **代码单元：code cell**.

.. image:: ./_static/images/blank-notebook-ui.png

**Notebook name**: 显示在页面顶部，Jupyter logo旁边的名称反映了.ipynb文件的名称。
单击笔记本名称会弹出一个对话框，允许您重命名它。
因此，在浏览器中将笔记本从"Untitled0"重命名为"My first notebook"，
将``Untitled0.ipynb``文件重命名为``My first notebook.ipynb``。

**Menu bar**: 菜单栏提供了可用于操纵notebook功能的不同选项

**Toolbar**: 通过单击图标，工具栏可以快速执行notebook中最常用的操作。

**Code cell**: 默认的单元格类型;继续阅读单元格的解释。


notebook 文档的结构
--------------------------------

notebook包含一系列单元格。单元格是多行文本输入字段，其内容可以使用:kbd:`Shift-Enter`执行，
也可以通过单击工具栏中的"Play"按钮或菜单栏中的:guilabel:`Cell`，:guilabel:`Run`来执行。
单元格的执行行为由单元格的类型决定。有三种类型的单元格：**code cells**，, **markdown cells**, **raw cells**.
每个单元格初始都是一个**code cell**，但可以通过工具栏上的下拉菜单或键盘快捷键:ref:`keyboard shortcuts <keyboard-shortcuts>`来更改其类型。

有关可以在notebook中执行的各种操作的更多信息，请参阅示例集合。
`collection of examples
<https://nbviewer.jupyter.org/github/jupyter/notebook/tree/master/docs/source/examples/Notebook/>`_.

Code cells
~~~~~~~~~~
*code cell* 允许您编辑和编写新代码，完整的语法突出显示和选项卡完成。
您使用的编程语言取决于*kernel*，默认kernel（IPython）运行Python代码。
A *code cell* allows you to edit and write new code, with full syntax
highlighting and tab completion. The programming language you use depends
on the *kernel*, and the default kernel (IPython) runs Python code.

执行code cell时，它包含的代码将发送到与notebook相关联的kernel。
然后，从该计算返回的结果将作为单元格的输出*output*显示在notebook中。
输出不限于文本，还可以使用许多其他可能的输出形式，包括``matplotlib``图和HTML表（例如，在``pandas``数据分析包中使用）。
这被称为IPython丰富的显示功能。

.. seealso::

   `Rich Output`_  example notebook

Markdown cells
~~~~~~~~~~~~~~
您可以使用富文本以有文化的方式记录计算过程，将描述性文本与代码交替。
在IPython中，这是通过使用Markdown语言标记文本来完成的。
相应的单元称为Markdown单元。 
Markdown语言提供了一种执行此文本标记的简单方法，即指定应强调文本的哪些部分（斜体），粗体，表单列表等。

如果要为文档提供结构，可以使用markdown标题。 
Markdown标题包含1到6个标记``#``，后跟空格和部分标题。
markdown标题将转换为笔记本部分的可点击链接。
在导出为PDF等其他文档格式时，它也可用作提示。

执行Markdown单元格时，Markdown代码将转换为相应的格式化富文本。 Markdown允许任意HTML代码进行格式化。

在Markdown单元格中，您还可以使用标准的LaTeX表示法以简单的方式包含数学：``$...$``用于内联数学，``$$...$$``用于显示数学。
执行Markdown单元格时，LaTeX部分会自动在HTML输出中呈现为具有高质量排版的方程式。
这是由MathJax实现的，它支持大量的LaTeX功能。
Within Markdown cells, you can also include *mathematics* in a straightforward
way, using standard LaTeX notation: ``$...$`` for inline mathematics and
``$$...$$`` for displayed mathematics. When the Markdown cell is executed,
the LaTeX portions are automatically rendered in the HTML output as equations
with high quality typography. This is made possible by MathJax_, which
supports a `large subset <mathjax_tex>`_ of LaTeX functionality

.. _mathjax_tex: https://docs.mathjax.org/en/latest/tex.html

Standard mathematics environments defined by LaTeX and AMS-LaTeX (the
``amsmath`` package) also work, such as
``\begin{equation}...\end{equation}``, and ``\begin{align}...\end{align}``.
New LaTeX macros may be defined using standard methods,
such as ``\newcommand``, by placing them anywhere *between math delimiters* in
a Markdown cell. These definitions are then available throughout the rest of
the IPython session.

.. seealso::

    `Working with Markdown Cells`_ example notebook

Raw cells
~~~~~~~~~

*Raw* cells provide a place in which you can write *output* directly.
Raw cells are not evaluated by the notebook.
When passed through nbconvert_, raw cells arrive in the
destination format unmodified. For example, you can type full LaTeX
into a raw cell, which will only be rendered by LaTeX after conversion by
nbconvert.


Basic workflow
--------------

The normal workflow in a notebook is, then, quite similar to a standard
IPython session, with the difference that you can edit cells in-place multiple
times until you obtain the desired results, rather than having to
rerun separate scripts with the ``%run`` magic command.


Typically, you will work on a computational problem in pieces, organizing
related ideas into cells and moving forward once previous parts work
correctly. This is much more convenient for interactive exploration than
breaking up a computation into scripts that must be executed together, as was
previously necessary, especially if parts of them take a long time to run.

To interrupt a calculation which is taking too long, use the :guilabel:`Kernel`,
:guilabel:`Interrupt` menu option, or the :kbd:`i,i` keyboard shortcut.
Similarly, to restart the whole computational process,
use the :guilabel:`Kernel`, :guilabel:`Restart` menu option or :kbd:`0,0`
shortcut.

A notebook may be downloaded as a ``.ipynb`` file or converted to a number of
other formats using the menu option :guilabel:`File`, :guilabel:`Download as`.

.. seealso::

    `Running Code in the Jupyter Notebook`_ example notebook

    `Notebook Basics`_ example notebook

.. _keyboard-shortcuts:

Keyboard shortcuts
~~~~~~~~~~~~~~~~~~
All actions in the notebook can be performed with the mouse, but keyboard
shortcuts are also available for the most common ones. The essential shortcuts
to remember are the following:

* :kbd:`Shift-Enter`:  run cell
    Execute the current cell, show any output, and jump to the next cell below.
    If :kbd:`Shift-Enter` is invoked on the last cell, it makes a new cell below.
    This is equivalent to clicking the :guilabel:`Cell`, :guilabel:`Run` menu
    item, or the Play button in the toolbar.

* :kbd:`Esc`: Command mode
    In command mode, you can navigate around the notebook using keyboard shortcuts.

* :kbd:`Enter`: Edit mode
    In edit mode, you can edit text in cells.

For the full list of available shortcuts, click :guilabel:`Help`,
:guilabel:`Keyboard Shortcuts` in the notebook menus.

Plotting
--------
One major feature of the Jupyter notebook is the ability to display plots that
are the output of running code cells. The IPython kernel is designed to work
seamlessly with the matplotlib_ plotting library to provide this functionality.
Specific plotting library integration is a feature of the kernel.

Installing kernels
------------------

For information on how to install a Python kernel, refer to the
`IPython install page <https://ipython.org/install.html>`__.

The Jupyter wiki has a long list of `Kernels for other languages
<https://github.com/jupyter/jupyter/wiki/Jupyter-kernels>`_.
They usually come with instructions on how to make the kernel available
in the notebook.


.. _signing_notebooks:

Trusting Notebooks
------------------

To prevent untrusted code from executing on users' behalf when notebooks open,
we store a signature of each trusted notebook.
The notebook server verifies this signature when a notebook is opened.
If no matching signature is found,
Javascript and HTML output will not be displayed
until they are regenerated by re-executing the cells.

Any notebook that you have fully executed yourself will be
considered trusted, and its HTML and Javascript output will be displayed on
load.

If you need to see HTML or Javascript output without re-executing,
and you are sure the notebook is not malicious, you can tell Jupyter to trust it
at the command-line with::

    $ jupyter trust mynotebook.ipynb

See :ref:`notebook_security` for more details about the trust mechanism.

Browser Compatibility
---------------------

The Jupyter Notebook aims to support the latest versions of these browsers:

* Chrome
* Safari
* Firefox

Up to date versions of Opera and Edge may also work, but if they don't, please
use one of the supported browsers.

Using Safari with HTTPS and an untrusted certificate is known to not work
(websockets will fail).

.. include:: links.txt
