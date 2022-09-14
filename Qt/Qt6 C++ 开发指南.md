# Qt

## 第一章

1. 安装组件
   1. Qt x.x.x
      1. macOS
      2. Qt Shader Tools
      3. Additional Libraries
   2. Developer and Designer Tools
      1. Qt Creator x.x.x
      2. Qt Creator x.x.x Plugin Development
      3. Qt Installer Framework x.x
2. Shadow Build
   1. 只有在需要输出多种构建版本的输出文件时才勾选

## 第二章

1. QBaseclass 代指创建项目过程中的基类, 如 QWidget、QDialog、QMainWindow 等
2. UI 文件是一个 XML 文件,存储界面上各组件的属性和布局
3. ui_baseclass.h 是基于 QBaseclass 的 UI 文件, 经 UIC 编译后生成

   1. `void setupUi(QBaseclass * Baseclass);`
   2. `void retranslateUi(QBaseclass * Baseclass)`
   3. 定义命名空间`Ui`, 并定义一个从`Ui_Baseclass`继承的类`Baseclass`

      ```c++
         namespace Ui{
            class Baseclass : public Ui_Baseclass{};
         }
      ```

4. baseclass.h 继承自 QBaseclass
   1. 引入`Ui_Baseclass`外部声明
   2. 声明一个指向`Ui_Baseclass`的指针`*ui`
   3. 通过`ui`指针和界面交互
5. UI 文件扩充 QBaseclass 的界面, baseclass.h 扩充相应的功能

## 第三章

1. 遍历容器
   1. `begin()`使迭代器指向容器的第一个数据项
   2. `end()`表示容器最后数据项后面的位置, 数据项是无效的、一般用作循环结束条件
   3. `rbegin()`反向的起始位置
   4. `rend()`反向的结束位置
   5. `constBegin()`只读迭代器的起始位置
   6. `constEnd()`只读迭代器的结束位置

## 第四章

1.
