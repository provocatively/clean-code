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


