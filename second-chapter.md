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

比如：

    class DtaRcrd102 {
      private Date genymdhms;
      private Date modymdhms;
      private final String pszqint = "102";
    ...
    }
 
    class Customer {
      private Date generationTimeStamp;
      private Date modificationTimeStamp;
      private final String recordId = "102";
    ...
    }

在阅读的时候能够清楚的感受到两个类在表达性上的区别，后者显然更好。

## 2.5 使用可搜索的名称

比较代码：

    for (int j = 0;j < 34;j++) {
      s+= (t[j] * 4) / 5;
    }

和

    int realDaysPerIdealDay = 4;
    const int WORK_DAYS_PER_WEEK = 5;
    int sum = 0;
    for (int j = 0;j < NUMBER_OF_TASKS;j++) {
      int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
      int realTaskWeekDays = (realdays / WORK_DAYS_PER_WEEK);
      sum += realTaskWeeks;
    }

其中的sum并非特别有用的名称，不过还是可以用于搜索的，采用能够表达意图且用于搜索的方式，代码的长度似乎被拉长了，不过WORK_DAYS_PER_WEEK要比数字5好找的多。

## 2.6 避免使用编码

* 使用匈牙利标记法标记类型(很早以前的编译器没有类型检查)
* 使用成员前缀
* 接口和实现类（不必在接口上标记，如使用IShapeFactory，如果接口和实现必须选一个来编码的话选择ShapeFactoryImpl会比对接口编码来的好）

对于不对接口的命名进行编码，作者在书中阐述的理由是："这个I被滥用之后，说的好听点是干扰，说的不好听就是废话。我不想让用户知道我给他们的是接口，我就想让他们知道那是个ShapeFactory。"
我对于这一部分的理解可能还不是非常到位，不过都是约定俗成的使用 接口 + Impl 来进行命名。

## 2.7 类名

类名和对象名应该是名词或者名词短语，如Customer,WikiPage,Account和AddressPaser。避免使用Manager,Processor,Data和Info这样的名称。类名不应当是动词。

主要强调的还是命名的准确性和表达性，类似Manager等命名过于泛指，所以不推荐使用。

## 2.7 方法名

方法名应当是动词或者动词短语，如postPayment, deletePage或save。属性访问器、修改器和断言应该根据其值命名，并依照Javabean标准加上get、set和is前缀。

    String name = employee.getName();
    customer.setName("Mike");
    if (paycheck.isPosted())...

重载构造器时，使用描述了参数的静态工厂方法名。例如，

    Complex fulcrumPoint = Complex.FromRealNumber(23.0);

通常好于：

    Complex fulcrumPoint = new Complex(23.0);

可以考虑将相应的构造器设置为private，强制使用这种命名手段。

书中只是提出了这个规约，总结一下，一个类提供静态工厂方法有如下好处：
* 不需要根据参数类型和顺序区分重载方法，提升代码可读性
* 工厂方法内和返回返回类型的任何子类型，在选择返回对象上有很大的灵活性
* 减少创建参数化类型时的冗余信息，如，

没有使用静态工厂时：

    Map<String, List<String>> m = new HashMap<String, List<String>>();

使用静态工厂后：

   public static <K, V> HashMap<K, V> newInstance() {
     return new HashMap<K, V>
   }
   
   Map<String, List<String>> m = HashMap.newInstance();

不过，标准的集合类实现中并没有这样定义的工厂方法。

缺点：
* 只提供静态工厂方法的类的缺点在于不能被子类化
* 静态工厂方法和其他静态方法不容易区分（主要是静态工厂方法不像构造器一样明显，所以需要有静态工厂的命名约定比如：newInstance、getInstance等）
    
## 2.8 每个概念对应一个词

使用fetch、retrieve和get类给多个类中的同种方法命名。你怎么记得住哪个类中的是哪个方法呢？开发Web应用的时候这种命名方式可能比较常见，这种时候使用这些方法的人可能就需要花费时间去搞清楚这个方法的意思。
然而如果参数能够从方法名表现出来，譬如Example::getUserById(int id)，在使用这个方法时可能就减少了思考的时间，简直就是天降福音。

## 2.9 使用解决方案领域名称

记住，只有程序员才会读你的代码，所以尽管使用CS的术语是没有问题的。对于熟悉访问者模式（VISITOR）的程序员来说，AccountVisitor富有意义。哪一个程序员会不知道JobQueue呢？使用技术性命名是最靠谱的做法。

## 2.10 添加有意义的语境

先看代码，

    private void printGuessStatistics(char candidate, int count) {
      String number;
      String verb;
      String pluralModifier;
      if (count == 0) {
        number = "no";
        verb = "are";
        pluralModifier = "s";
      } else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
      } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
      }
      String guessMessage = String.format(
        "There %s %s %s%s", verb, number, candidate, pluralModifier
      );
      print(guessMessage);
    }

再对比下面的代码：

    public class GuessStatisticMessage {
      private String number;
      private String verb;
      private String pluralModifier;
      
      public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format(
          "There %s %s %s%s", verb, number, candidate, pluralModifier
        );
      }
   
      private void createPluralDependentMessageParts(int count) {
        
        if (count == 0) {
          thereAreNoLetters();
        } else if (count == 1) {
          thereIsOneLetter();
        } else {
          thereAreManyLetters(count);        
        }
      }

      private void thereAreManyLetters (int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s"
      }
  
      private void thereIsOneLetter() {
        number = "1" 
        verb = "is";
        pluralModifier = "";
      }

      private void thereIsNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
      }
    }

增强语境后函数也分割的更小，更加干净。

## 2.11 最后的话

取好的名字最难的地方在于需要良好的描述技巧和共有文化背景。与其说这是一种技术或者管理问题，还不如说是一种教学问题。结果是这个领域里的很多人都没有学的很好。
我们有时会怕其他开发者反对重新命名。如果讨论一下就会知道，如果名称改的更好，所有人都会很感激这件事。多数时候我们并不记忆类名和方法名，我们使用现代工具来对付这些细节。好让自己集中精力于把代码写得就像诗词一样。改名是一种代码改善工作，别让这种事阻挡你前进的步伐。
不妨试一下上面的这些规则，看看代码的可读性是否有所提升，如果你是在维护别人的代码，使用重构工具来解决问题。效果立竿见影，而且会持续下去。
