# 第2章 有意义的命名

整洁编码的命名要求大致有以下几点

## 2.1 名副其实

先举一个例子
    
    int d // 消逝的时间，以日记

名称d是没有怕是没有说明什么东西，根本就不能让你有任何时间消失的感觉。然后再给一些指明了计量对象和计量单位的名称：

    int elapsedTimeInDays;
    int daysSinceCreation;
    int daysSinceModification;
    int fileAgeInDays;
    
选择能够体现本意的名称能够让人更容易理解和修改代码，请看下面的代码说一说它的目的何在：

    public List<int[]> getThem() {
      List<int[]> list1 = new ArrayList<int[]>();
      for (int[] x : theList)
        if (x[0] == 4)
          list1.add(x);
      return list1;
    }

我想不难会有以下的疑问：
* theList里面是什么变量类型
* theList的零下标条目的意义是什么
* 值4的意义是什么
* 我们如何使用返回的列表

如果代码修改为如下，这些问题又如何：

    public List<Cell> getFlaggedCells() {
      List<Cell> flaggedCells = new ArrayList<Cell>();
      for (Cell cell : gameBoard)
        if (cell.isFlagged())
          flaggedCells.add(cell)
      return flaggedCells;
    }

简单的改一下名称就能够轻易地知道发生了什么，好名称的力量可见一斑。起一个好名字说起来简单，但是确实会花费不少时间，我认为完全有必要去花这个时间去做出一个好的命名。实际上在前期花去的修改命名的时间，或者是有好名称又去修改的时间，比起后期团队成员阅读的时间还是微不足道的，对于一个好名称的评判标准，我想如果能够不用注释就能够让人明白意思的话，那么就是一个好的名称。

## 2.2 避免误导

书上给出了一个例子，别用accountList来指称一组账号，除非它真的是List类型，因为List一词对于程序员来说有特殊的含义。如果这个容器不是真的List就会引起错误的判断。这时候用accountGroup或者bunchOfAccounts，甚至用accounts都会好一些。其实可以简单的认为会和类名相似的用于命名时，如果其实并非一致类，那么就存在引起误解的可能性。同时还有相似命名，譬如XYZControllerForEfficientHandlingOfStrings和XYZControllerForEfficientStorageOfStrings在区分的时候需要多少时间呢？
书上还举了一个使用l和1，O和0，可能是使用字体的原因会出现混淆，但是可能其影线在现在来说并不是那么明显。

## 2.3 做有意义的区分

没有意义的区分，给一个例子：

    public static void copyChars(char a1[], char a2[]) {
      for (int i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
      }
    }

如果a1改为source，a2改为destination的话意义就明显很多了。

还有一种无意义的区分，譬如你有一个Product类，如果还有一个ProductInfo或者ProductData类，他们名称虽然不同，但是意思却没什么区别，Info和Data就像a、an和the一样，只是意义混含的废话。
这里的意思是Product和ProductInfo实际上的区别可能不是那么明显。

废话都是冗余。Variable永远不该出现在变量名中。Table一词永远不应当出现在表名中。NameString会比Name好？难道Name会是一个浮点数不成？如果是这样，就触犯了关于误导的规则。设想有一个Customer类，还有一个CustomerObject类，区别何在？

有一个应用反映了这种情况，犯错的代码如下：

    getActiveAccount();
    getActiveAccounts();
    getActiveAccountInfo;

我们怎么知道应该调用哪一个。如果缺少明确的约定，customer和customerInfo就没区别，theMessage和message没区别，所以说想要直接的区分就需要使用读者能够鉴别的方式。

这边说的命名区分，实际上如果团队内部有对应的规约，那么一般来说不容易产生歧义。首先，对于新的阅读者来说，如果尚未熟悉规约的话对于其理解、上手代码都会存在很大的难度。其次，作为表达需求的需要，代码本身实质上就是对于需求的规约，如果能够将区分命名的规约加入到代码本身，而不是另外采用约定的方式，想必是会事半功倍的。

## 2.4 使用读得出来的名称

