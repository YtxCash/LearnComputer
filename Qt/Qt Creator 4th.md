# QtCreator 4th

## 第3章 窗口

QWidget

1. 没有嵌入到其他部件中的部件，因没有父部件，又称为顶级部件
2. 一般都有边框和标题栏；构造函数有两个参数，`parent`默认为`nullptr`，另一个参数`f`，是`Qt::WindowFlags`类型，由`Qt::WindowType`枚举类型值的或组合
3. 几何布局对窗口的大小和位置，根据是否`包含边框和标题栏`两种情况，用不同的函数来获取
   1. 包含框架`x()`, `y()`, `pos()`, `frameGeometry()`, `move()`等
   2. 不包含框架`geometry()`, `width()`, `height()`, `size()`, `rect()`等
4. `Ctrl`加`Table`快速切换已打开文件
5. widget1
   1. 创建控制台程序，cmakelist替换Core为Widgets
   2. 创建QApplication
   3. 创建QWidget w，设置标题
   4. 创建QLabel l1，设置标题，文字和大小
   5. 创建QLabel l2，以w为父，设置文字和大小
   6. 显示w和l1，运行程序；清理指针，返回运行结果
6. 调试包含头文件`QDebug`，然后使用输出流的方式一次输出多个值，`Qt::endl`换行

   ```Qt
   #include <QDebug>
   qDebug() << "geometry" << geometry << Qt::endl;
   ```

QDialog

1. 按照运行时对话框是否可以同其他对话框交互分类

   1. 模态不可以交互，调用对话框的`exec()`函数

      ```Qt
      QDialog dialog(this);
      dialog.exec();
      ```

   2. 非模态可以交互，`new`创建，`show()`显示；`setModal(true)`可以变为模态

      ```Qt
      QDialog *dialog = new QDialog(this);
      // dialog -> setModal(true);变为模太
      dialog -> show();
      ```

2. 信号和槽

   1. 在对象的头文件里声明公开槽函数、源文件定义，接着在构造函数里、通过`connect`联接`ui`操作和槽函数

      ```Qt
      private slots:
          void on_showChildButton_clicked();
          void showChildButton();

      void Widget::on_showChildButton_clicked()
      {
          QDialog *dialog1 = new QDialog(this);
          dialog1->resize(200,200);
          dialog1->setWindowTitle("Modalless 1");
          dialog1->show();
      }

      void Widget::showChildButton()
      {
          QDialog *dialog1 = new QDialog(this);
          dialog1->resize(200,200);
          dialog1->setWindowTitle("Modalless 2");
          dialog1->show();
      }

      connect(ui->showChildButton, &QPushButton::clicked,this,&Widget::showChildButton);
      ```

3. 自定义对话框

   1. `accept()`会隐藏对话框、并返回`QDialog::accepted`这个值
   2. `reject()`返回`QDialog::rejected`这个值，可以用来做判断
   3. `close()`槽在仅有最后一个主界面时调用才会退出，其他情况是隐藏，`show()`会再次显示
   4. 登录界面通过`accept()`到主界面，主界面`close()`隐藏并返回登录界面，`show()`返回主界面
   5. 先生成`QWizardPage`，再添加到`QWizard`，以生成向导
   6. `Enter`快捷键`setShortcut(Qt::Key_Return)`，小键盘`Enter`为`setShortcut(Qt::Key_Enter)`

4. Frame，有四个属性

   1. `frameShape`边框形状，7种
   2. `frameShadow`边框阴影，3种
   3. `lineWidth`线宽
   4. `MidlineWidth`中线宽

5. Label

   1. `wordWrap`文本自动换行；若想在后面省略，使用`QFontMetrics`类的`elidedText()`函数
      1. 过长的原始文本
      2. 用标签的函数`ui->label->fontMetrics().elidedText(texts, Qt::ElideRight, 100)`处理，取得返回的文本
      3. `ui->label->setText(str)`，使用返回的文本
   2. `scaledContents`绽放标签中的内容

6. pushButton

   1. `checkable`选中和未选中两种状态
   2. `flat`不显示按钮的边框
   3. `toggled`在两个状态间切换
   4. `tristate`选中、未选中、不改变状态三种

7. lineEdit

   1. `echoMode`四种状态，`Normal`正常显示，`NoEcho`不显示，`Password`密码样式，`PasswordEchoOnEdit`编辑时正常显示、其他显示为密码
   2. `inputMask`输入掩码
   3. `text()`输出文本，`displayText()`输出显示的文本
   4. `validator`创建一个validator（int, double, RegularExtression三种），然后组件设置；超范围的数字是无效的，不能输出
   5. `QReguarExpressionValidator`先创建一个`QRegularExpression`，再通过`QRegularExpressionValidator`创建validator，最后组件使用
   6. `QCompleter`自动补全，先建stringList、录入内容，再根据list创建completer、设置completer属性，组件最后应用

8. date time
   1. `calendarPopup`弹出日历可以设置日期

## 第四章 布局

1. `sizeHint`大小提示，配合`QSizePolicy`使用
   1. `Fixed`无法伸缩
   2. `Minimum`sizeHint的值最小，只可拉伸
   3. `Maximum`sizeHint的值最大，只可压缩
   4. `Prefered`sizeHint的值最佳，可压缩或拉伸
   5. `Expanding`sizeHint的值合适，可被压缩，更倾向拉伸
   6. `MinimumExpanding`sizeHint的值是最小的，倾向拉伸
   7. `Ignored`sizeHint的值被忽略，尽可能拉伸
2. `minimumSizeHint`建议的最小大小揭示
3. `stretch factor`部件间比例
4. 定位器，快速帮助，`Ctrl + K`，`? QLabel`

## 第五章 主窗口

1. 通过`QMenu *editMenu = ui->menuBar->addMenu(tr("&Edit"))`创建并返回菜单
2. 通过`QAction *actionCut = editMenu->addAction(tr("Cut"))`添加并返回子功能
3. 设置子功能属性，添加到工具栏
4. 指定快捷键`setShortCut(QKeySequency(tr"Ctrl+X"))`，`Ctrl+X`，中间不能有空格，首字母大写
5. 使用资源文件，`QIcon(":/Images/images/1.png"`，双引号/前缀/文件夹/文件
6. `QActionGroup`，新建一个group，通过group添加action，最后再把action添加到菜单栏
7. `QToolBar`通过`addWidget()`向工具栏添加部件
8. `QMdiArea`把部件添加为mdi的子窗口，`ui->mdiArea->addSubWindow()`,然后显示
9. `QStatusBar`状态栏
   1. `showMessage`显示临时信息，在状态栏的最左边
   2. `addWidget()`添加一个label，显示正常信息，在最左边，可能会被临时消息覆盖
   3. `addPermanentWidget()`添加一个label，最右端，不会被覆盖
   4. `setSizeGripEnabled`，控制状态栏最右的小三角是否显示
10. 自定义菜单
    1. 继承`QWidgetAction`
    2. 重新实现`createWidget`
    3. 添加`Q_OBJECT`，声明函数`QWidgetAction *createWidget(QWidget *parent) override`，添加信号、私有槽、私有对象
11. 富本文，暂时跳过
12. 拖放
    1. 拖动，拖动时数据存储为MIME类型，判断`event->mimeData()`是否`hasUrls`，决定拖动进入事件是否成立`event->acceptProposedAction`，否则忽略
    2. 放下，获取`event->mimeData()`，获取`mimeData->urls()`列表，将第一个url表示为本地文件路径，判断文件路径，如果存存，只读打开；如果打开成功、建立流，使用文本
    3. 构造函数允许接受拖放`setAcceptDrops(true)`
13. 图片拖放，暂时跳过
14. 打印，在ui添加action，添加私有槽，槽里实现打印相关的动作

## 第六章 事件

1. 单击按钮，会产生鼠标事件；按钮被按下，发射单击信号
2. 类型，由`QEvent::Type`表示
3. `event(QEvent *event)`函数和事件处理函数是在焦点部件内部处理完成的，事件过滤器`eventFilter(QObject *ojb, QEvent *event)`是在焦点部件的父部件定义的
4. 事件过滤器
   1. 在一个部件中监控其他多个部件，父部件中声明和定义，在父部件的构造函数中，对目标安装过滤器`.installEventFilter(this)`，`this`表示在安装位置
   2. 先判断部件类型，再判断事件类型，如果是需要的事件就进行强制类型转换，然后进行相应的处理
   3. 如果需要对一个特定的事件进行处理，且不希望在后面的传递过程中再被处理，返回`true`，否则返回`false`

## 第七章

1. 信号和槽
   1. 更常用的做法是子类化部件，然后添加自定义的信号和槽来实现想要的功能
   2. 在文本中插入可变数值，`tr("获取的值是 %1").arg(value)`
   3. 信号只有声明，没有定义，且返回值为void
   4. `connect`第四个参数，一般是槽，也可以为信号
   5. 一般不使用自动关联
   6. 只读迭代器比读写迭代器要快很多，尽可能的多使用只读迭代器
   7. 如果在表达式中不会对返回值进行处理，最好使用前缀操作符`++i`和`--i`，效率更高
   8. `isEmpty()`判断一个字符串是否为空
   9. 正则表达式
      1. 在一个文本中匹配子字符串的一种模式：验证、搜索、查找替换、分割
      2. 由两部分组成，一个模式字符串`QRegularExpression re("")`和一组模式选项`.setPattern("")`
      3. 模式：表达式、量词、断言
         1. `^`作为第一个字符，需从字符串的开始匹配
         2. `$`作为最后一个字符，必需匹配到字符串的结尾
         3. `[0-9]`可以用`\d`代替
         4. 只出现一次的表达式，可以用表达式本身代替`x{1,1}`等价于`x`
         5. `{0,1}`表示量词是可选的，出现一次或者不出现，可用`?`代替
         6. 单词边界断言`\b`，匹配一个位置，而不是一个字符，是任何的非单词字符
         7. 原始字符串无需把斜杠转义`(R"()")`
         8. `("M(?!ail)")`匹配字母`M`，其后面不能是`ail`
         9. `E{n,m}`表示匹配出现的次数

## 第十五章

1. 构建`QFile`对象,同时指定文件名,此对象可以存储文件信息
2. 设置对象的打开方式,按位或`|`表示同时支持多个选项
3. 操作数据
4. 关闭文件`file.close()`
