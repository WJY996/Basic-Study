# Java学习笔记
## 基本操作
### 异常处理机制(try,catch,finally)


Java通过面向对象的方法来处理例外。在一个方法的运行过程中，如果发生了例外，则这个方法生成代表该例外的一个对象，并把它交给运行时系统，运行时系统寻找相应的代码来处理这一例外。我们把生成例外对象并把它提交给运行时系统的过程称为**抛弃(throw)一个例外**。运行时系统在方法的调用栈中查找，从生成例外的方法开始进行回朔，直到找到包含相应例外处理的方法为止，这一个过程称为**捕获(catch)一个例外**。

    boolean test() throws Exception
    {
        try
        {
        //代码区
        }
        catch(Exception e)
        {
        //异常处理
        }
        finally
        {
        //提供统一出口,可进行资源的清除工作
        }
    }
代码区如果有错误，就会返回所写异常的处理。

如果try由于其他原因R突然中止（而非抛出异常v），那么finally块被执行，分为两种情况：
 
- 如果finally块执行顺利，那么整个try-catch-finally程序块的结局是“由于原因R突然中止（completes abruptly）”。
- 如果finally块由于原因S突然中止，那么整个try-catch-finally程序块的结局是“由于原因S突然中止（completes abruptly）”，原因R将被抛弃。
