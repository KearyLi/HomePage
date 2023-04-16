# 数据结构与算法

> 高性能、快速、优雅                数据结构+算法=代码优化    掌握数据结构原理      锻炼算法思维

> 在考虑算法时，想想空间复杂度(解法需要的内存空间)和时间复杂度(解法需要的时间)，从而得到最优算法。
>
> 解决一件事时，想想每种数据结构的特性，由于每种特性有优点缺点，和多线程使用一样，这时就得组合用好每种数据结构的优点，从而得到最佳解决办法。就像一个团体一样，每个人发表自己独特看法，从而得到最好的解决方案。

## 空间时间效率

> 空间复杂度与时间复杂度

编程的领域中，代码效率的瓶颈有两个维度，就是空间和时间，空间就是数据存放的地方，时间就是代码从执行到结果产生所用的时间；所以可以感觉到，空间其实很廉价，用点大容量的内存或者磁盘就行，但是时间上就得快了，正所谓寸金难买寸光阴。但是话说回来，用最少的空间和时间才是最佳解决方案，除非得取舍时才多考虑时间。

空间复杂度的优化就得巧用数据结构，时间复杂度的优化就得算法思维好

**空间复杂度和时间复杂度：**空间和时间对于输入数据量的关系

- O(1)的含义其实就是当处理10个数据会花掉空间和时间4和2，处理10000的数据也是只用空间和时间4和2，比如数组按下标查找、栈的压栈和出栈、队列的队头删除和队尾的插入都是O(1)，还比如在一个算法中缓存一个变量值来记录值变化，这样的空间复杂度就是O(1)，无论输入多少数据都只用了一个小变量的内存
- 如果是两个嵌套的for循环，那就是O(n^2^)，依次递增
- 如果两个for循环是分开的，可以说是O(n)+O(n) = O(2n)，根据复杂度与具体系数无关原则，最终为O(n)
- 如果两个for循环是分开始，如果是O(n^2^)+O(n)，用系数大的表示复杂度，即O(n^2^)
- 二分查找时间复杂度是O(logn)

其实处理**算法**就是思考用什么样的代码对目标参数**执行怎样的操作**，一般脱离不了增删查，然后在思考用什么的**数据结构**来**组织**算法操作的数据；总体来说前者就是算法的思想，后者就是数据结构的合理使用。

## 数据的计算机表示

> 整数：java中基本类型(byte、short、int、long等)有不同的空间限制，处理时考虑空间溢出的问题，且负数转正数容易溢出，比如-2^31^~2^31^-1，它最小负数转最大正数就多了1，溢出发生。
>
> 二进制：二进制数主要用法就是0和1的与、或、非、异或、左移、右移操作。

## 数据结构之数组

> 下面的栈、队列有用数组来实现，那都是数组的限制版，所以直接操作数组的增删查会更灵活，就是限制少，想加在哪就加在哪，想删除哪个就删除哪个

特性：

1. 相同类型元素的集合
2. 内存中连续顺序存放
3. 每次创建得预先固定数组空间大小，用下标获取元素
4. 读O(1)，查O(n)，插O(n)，删O(n)

数组的增删查：注意在数组中间插入或删除数据如果考虑后面数据位置移动会让时间复杂度变成O(n)，和只在数组最后插入是不同的 ，读的话就不用说了，直接按照下标O(1)直接读出来，这和查又不一样了

所以这就有个问题了，数组和链表用哪个好呢，其实这就得看他们各自优点了，数组是规定长度的，链表是很动态扩增的，链表读的话没下标就没数组快，删除插入的话数组又没链表好，所以用的时候看场景来选择

**场景1：**双指针，两个指向数组元素位置的指针，两个指针分别/同时指向数组头元素和尾元素

```java
//在递增数组中找两个和为目标值的两个下标并返回
public static int[] twoSum(int[] ints, int targetValue) {
        int i = 0;
        int j = ints.length-1;
        while (i < j && ints[i] + ints[j] != targetValue) {
            if (ints[i] + ints[j] > targetValue) {
                j--;
            } else {
                i++;
            }
        }
        return new int[]{i,j};
    }
```



## 数据结构之字符串

> 字符串就是个String类，里面封装了一个字符数组，还有一些方法对字符操作，就这么个东西

**特性：**

1. 内部采用连续字符数组实现
2. 不变性，对字符串的修改不影响最初字符串，而是产生新字符串，比如str.toUpperCase()后str的值不变

反正用的时候就用里面的一些基础方法来实现字符串的操作，比如charAt(int index)、split(" ")什么的

反转英文句子：就是"I love you"，结果为"you love I"的实现就可以用split(" ")，然后依次入栈，再出栈就得到了

## 数据结构之链表

> - 单向链表
> - 双向链表：在单向链表中添加指向前节点的指针
> - 循环链表：在单向链表中添加尾节点指向头结点
> - 双向循环链表：结合双向链表与循环链表

**特性：**

1. 内存中的链表不是连续的，它是由节点指向来指向下一个节点(内存中就是指向下一个节点的地址)
2. 创建不需要一次分配内存，可以动态添加删除
3. 增删改查都是改变节点的指向
4. 查O(n)，增O(1)，删O(1)；虽然增和删是O(1)，但实际执行的时候还是得先找，所以可以说都是O(n)

**单向链表的增删查：**

```java
//在p节点后面增加s节点
s.next = p.next;
p.next = s;
//删除s节点后面的一个节点
s.next = s.next.next;
//查就只能一个一个next的值去查
//用下面哨兵节点
```

**单向链表：**哨兵节点：用哨兵节点遍历链表，而且能防止链表为空时

```java
    public static void main(String[] args) {
        ListNode<String> aa = new ListNode<>("aa");
        ListNode<String> bb = new ListNode<>("bb");
        ListNode<String> cc = new ListNode<>("cc");
        aa.nextListNode = bb;
        bb.nextListNode = cc;

        ListNode<String> node = aa;//这里是将第一个节点当做哨兵节点，在写算法时考虑输入链表为空的情况

        while (node.nextListNode != null) {
            System.out.print(node.nextListNode.value+ " ");
            node = node.nextListNode;
        }
    }
```

**用哨兵节点删除指定节点：**删除时将指定节点的前一个节点指向指定节点的下一个节点

```java
//删除的同时遍历输出
while (node.next != null) {
            if (node.next.value == "dd") {
                node.next = node.next.next;
            }
            System.out.print(node.next.value+ " ");
            node = node.next;
        }
```

**前后双指针：**经典应用是查找链表倒数第N的节点，办法就是将第一个指针指向头结点并向后走N-1步，随后将第二个指针指向头结点，然后两个节点以相同速度向后移动，当第一个指针指向末尾节点时，第二个指针指向位置就是链表倒数第N的节点。

**快慢双指针：**经典应用是查找链表中间节点；AB指针指向头结点，然后A向后走两步，B向后走一步，当A走向尾节点时，B的位置就只链表中间节点。

```java
while(fast && fast.next && fast.next.next){
    fast = fast.next.next;
    slow = slow.next;
}
```

**反转链表：**由于单向链表只能从前到后遍历，所以有的情况需要从后到前遍历，就用反转链表

```java
while(curr){
    next = curr.next;
    curr.next = prev；
    prev = curr;
    curr = next;
}
```

**判断链表是否有环：**环就是单向链表中有两个节点多了个指向其他节点的指针

```java
这个同样用快慢双指针可以解决，因为只要有环路，就像操场中的跑道一样，两个指针一起跑的，肯定会有一次快的追上慢的
```



**双向链表：**单向链表其实没啥，但是双向链表处理是得多注意指向的问题，防止丢失指向问题



**循环链表：**循环链表主要就是注意死循环问题





## 数据结构之哈希表

> HashSet和HashMap；哈希表在读查插删挺快的，但是在对元素排序上不如TreeSet/TreeMap，在求元素中最大值最小值时没有堆好，而且哈希表查找元素得用完整Key来找，所以在使用部分key查找的情况用前缀树

特性：

1. 
2. 读O(1)，查O(1)，插O(1)，删O(1)

哈希表的设计：这个哈希表是新事物，为了让它存元素值后对元素操作的时间复杂度都为O(1)，刚好的方式是用数组来存元素的哈希值，数组的长度一开始是给定的，元素的存放位置为哈希值除数组长度取余，比如哈希值为5存入长度为4的数组中就存在数组1的位置，然后依次按这个规律存；完事会发现元素过多后导致多个元素存在数组的同一位置，这时就将这同一数组位置的元素用链表存放，就是为了让哈希表时间复杂度为O(1)，但是链表太长也会违背这个时间复杂度，所以得在当元素多于数组的一定程度后，用新的更长的数组来存放元素。这就是哈希表的设计原理。



## 数据结构之栈

> Stack的压栈push、出栈pop，在需要后入先出的存数据场景下使用

**特性：**

- 它和线性表的不同就是它主要操作是增删操作
- 后进先出的数据操作
- 虽然它看起来和用起来限制很大，只能在一端插入，删除；但其实这正是它的优点，这样的话在某些场景使用栈会让数据更安全，单端操作效率高(比如浏览器，IDEA使用鼠标的前进后退功能)
- 其实栈也属于线性表，表尾是栈顶top，表头是栈底bottom
- 栈有顺序表示和链式表示，分别叫顺序栈和链栈

**顺序栈：**

就是用数组来实现的栈，在创建栈的时候规定容量大小，top指向栈顶元素的位置，因为是顺序栈，所以top为0就代表栈中只有一个数据，在插入数据的时候top值不能超过容量大小，删除元素就相当于top-1，查找也是需要遍历全部

**链栈：**

就是用链表实现的栈，top指向第一个节点，然后每次压栈就相当在链表尾部增加一个节点，然后top向后移动指向新节点，同理删除节点就是top指向top.next，查找也是需要全部遍历，没办法的呀

**场景一：**

判断字符串‘{’，‘}’，‘(’，‘)’，‘<’，‘>’是否是有效的，这样就是有效的，就是左能匹配右，像【<><>】就是不合法的

```java
//这样的就得用数据结构中的栈来操作，遍历字符串把左压栈，下一个字符是右就弹栈，并判断弹出的和当前字符是否匹配
public static void main(String[] args) {
    String s = "{[()()]}";
    System.out.println(isLegal(s));
}
private static int isLeft(char c) {
    if (c == '{' || c == '(' || c == '[') {
        return 1;
    } else {
        return 2;
    }
}
private static int isPair(char p, char curr) {
    if ((p == '{' && curr == '}') || (p == '[' && curr == ']') || (p == '(' && curr == ')')) {
        return 1;
    } else {
        return 0;
    }
}
private static String isLegal(String s) {
    Stack stack = new Stack();
    for (int i = 0; i < s.length(); i++) {
        char curr = s.charAt(i);
        if (isLeft(curr) == 1) {
            stack.push(curr);
        } else {
            if (stack.empty()) {
                return "非法";
            }
            char p = (char) stack.pop();
            if (isPair(p, curr) == 0) {
                return "非法";
            }
        }
    }
    if (stack.empty()) {
        return "合法";
    } else {
        return "非法";
    }
}
```

场景二：

实现浏览器或IDEA中前进后退功能，用两个栈来放数据，每当点击新链接显示新网页时就把这个新链接压左边栈，如果想后退到旧的页面就将左边的栈弹出，放到右边栈，这样就实现了这个功能

**场景三：**

用后缀表达式计算结果，就是用{1,2,3,*,+}来计算，这种计算得先将1,2,3入栈，然后发现下一个是操作符就把栈里面的2,3取出来计算得到结果6继续放到栈里面，然后发现下一个还是操作符，就将栈里面的1,6取出计算得到结果7。

```java
    public static int pushPop(String[] strings) {
        Stack<Integer> stack = new Stack<>();
        for (String string : strings) {
            switch (string) {
                case "+":
                case "-":
                case "*":
                case "/":
                    int num1 = stack.pop();
                    int num2 = stack.pop();
                    stack.push(calculate(num1,num2,string));break;
                default:stack.push(Integer.parseInt(string));break;
            }
        }
        return stack.pop();
    }
    public static int calculate(int num1,int num2,String string) {
        switch (string) {
            case "+": return num1+num2;
            case "-": return num1-num2;
            case "*": return num1*num2;
            case "/": return num1/num2;
            default:return 0;
        }
    }
```

小总结：栈其实就是限制的数组或链表，只是这个限制优点还是很好的，能实现一些后进先出的场景，效率高，安全，而且栈删除插入的时间复杂度也是真的O(1)

<br>

## 数据结构之队列

> 有先进先出的场景使用
>
> 这个不能直接用Queue，它是一个接口，java使用它的实现类LinkedList，数组队列ArrayQueue，PriorityQueue

**特性：**

- 和栈也是属于限制版的数组或链表，所以就有了顺序队列和链式队列
- 在初始化栈时得固定数组或链表的大小
- 被限制成元素只能在队列尾rear插入，队列头front删除

**顺序队列：**

就是把front指向数组的第一个元素，就是下标0的元素，然后rear指向数组最后一个元素，当添加元素时就把rear向后指，但是删除就尴尬了，因为它是个数组，所以删除0元素会让所有元素向前移动一个位置，所以时间复杂度挺高O(n)

时间复杂度高的问题可用循环队列来实现，其实本质也是个数组，只是对front和rear的操作不同：具体就是当队列空时，就是数组空时让front和rear指向同一个地方，插入元素后让rear向后移动一个位置，删除一个元素就是删除front指向的元素，删除后front也向后移动一个位置，其中会发现一个问题就是front和rear在队列空和满时都指向同一个位置，其实这就是实现循环队列的关键，只要在程序中设置一个flag判断就知道当前队列是空还是满了

**链式队列：**

就是把front指向链表空头节点，rear指向链表最后一个节点，当front和rear指向同一个节点时就说明链式队列只剩一个头节点，然后删除就是将front指向的空头节点的next节点，插入元素就是rear指向最后一个节点的下一个节点

链式队列也有个问题，front指向的链表空头结点其实是为了解决野指针的问题，其实会发现这个是用链表实现的队列，当队列按照上面的步骤删除完后，front和rear指向空头结点，如果没有这个空头节点那front和rear就让队列没意义了，有的话还能让两个指针有地方可指，还能继续执行队列的操作，不然front和rear就不知道跑哪去了，就像爱情中两个相爱的人没有头结点联系起来，总会相忘于江湖



场景一：实现滑动窗口，滑动窗口在数字字符串上滑动一次就算一次窗口内数字的和，一开始就指定窗口大小

```java
static class MovingSum{
        LinkedList<Integer> queues;
        int size;
        int sum;
        int num;
        MovingSum(int size) {
            queues = new LinkedList<>();
             this.size = size;
        }
        void next(int next) {
            queues.offer(next);
            sum+=next;
            try {
                if (queues.size() > size) {
                    sum -= queues.poll();
                }
            }catch (Exception e){
                System.out.println("null EEE!");
            }
            System.out.println("向右移动到" + ++num+"，窗口数字和为："+sum);
        }
    }
    public static void main(String[] args) {
        int[] ints = {1,2,3,4,5,6,7,8,9,10};
        MovingSum movingSum = new MovingSum(3);
        for (int ins : ints) {
            movingSum.next(ins);
        }
    }
//结果
向右移动到1，窗口数字和为：1
向右移动到2，窗口数字和为：3
向右移动到3，窗口数字和为：6
向右移动到4，窗口数字和为：9
向右移动到5，窗口数字和为：12
向右移动到6，窗口数字和为：15
向右移动到7，窗口数字和为：18
向右移动到8，窗口数字和为：21
向右移动到9，窗口数字和为：24
向右移动到10，窗口数字和为：27
```

**场景二：**用队列解决约瑟夫环问题，就是有n个人围着圆桌坐着，然后其中一个人开始说1，依次加1传递说下去，说到m的人出局依次坐另外一个长凳子上，就这么循环直到圆桌上所有人到长凳子上，这个长凳子上人的顺序就是结果。这个就得用循环队列实现，让队头开始出队列然后从1开始计数，如果这个人小于m就再次入队列，如果等于m就出队列

```java
public static void main(String[] args) {
    ring(10, 5);
}
public static void ring(int n, int m) {
    LinkedList<Integer> q = new LinkedList<Integer>();
    for (int i = 1; i <= n; i++) {
        q.add(i);
    }
    int k = 2;
    int element = 0;
    int i = 1;
    for (; i<k; i++) {
        element = q.poll();
        q.add(element);
    }
    i = 1;
    while (q.size() > 0) {
        element = q.poll();
        if (i < m) {
            q.add(element);
            i++;
        } else {
            i = 1;
            System.out.println(element);
        }
    }
}
```

小总结：队列的实现和栈相似，都是限制版的数组或链表，查肯定都是O(n)，删和增都是O(1)，但是在循环队列中可以看出如果开始限制了队列长度，在使用过程会有空间浪费的情况，所以在一些队列长度不确定的场景还是用链式队列会好一些。然后呢，队列确实和栈一样，都有他们的优点，在考虑数据结构的时候想想每个数据结构的优点，然后用其优点，就会得到很好的数据结构选择

<br>

## 数据结构之树

> 树就是一个根节点没有父节点，叶子节点没有子节点的结构
>

**特性：**

- 它的实现和链表相似，一个类中有三个值：当前值变量，指向左子节点的引用，指向右子节点的引用
- 设计模式中的组合模式就相当于用的树结构实现
- 里面的节点都有名字，比如根节点、兄弟节点、叶节点、子节点、父节点
- 

**树的分类：**二叉树，链式存储树，数组的顺序存储树

- 满二叉树：就是除最后一层叶子节点，其他每个节点都有两个子节点
- 完全二叉树：就是满二叉树少几个叶子节点，但是叶子节点都按照左边连接着，为什么叫完全二叉树就是因为这样的树在数组中存储是很紧凑的，没有空的位置
- 链式存储树：每个节点有三个字段，一个存储数据，另外两个存储左右子节点地址
- 数组的顺序存储树：结合树的结构和数组的下标位置来存储节点，比如根节点放在1位置，两个子节点分别放在2和3的位置，这样会发现，当前节点位置如果是n，那它左子节点在数组中的下标就是2n，右子节点的位置就是2n+1

**二叉查找树：**和上面的不同，这种树在存储数据上有**要求**，是有顺序的，左子节点的值必须小于父节点，右子节点值必须大于父节点，这样的实现在中序遍历下来，数据就是从小到大，插入操作还行，删除就优点麻烦了

- 遍历：和正常前中后序查找一样
- 插入：从根结点开始，如果要插入的数据比根结点的数据大，且根结点的右子结点不为空，则在根结点的右子树中继续尝试执行插入操作。直到找到为空的子结点执行插入动作，时间复杂度为O(logn)
- 删除：
  - 如果是叶子节点就直接删除，让父节点指向null就行
  - 如果是删除的节点只有一个子节点就让该父节点指向该子节点
  - 如果删除的节点有两个节点：可以选择两种方式操作
    - 找到删除节点的左子树中最大值，然后删除点，替换到删除节点位置
    - 找到删除节点的右子树中最小值，然后删除点，替换到删除节点位置

**字典树：**就是根节点值为空，然后第一层的节点到每个叶子节点路程中节点能组成一个单词，可以用这个来匹配字符串是否在字符集中出现过，时间复杂度为O(logn)

```java
//实现一个节点类
public class TreeNode {
    int value;
    TreeNode leftNode;
    TreeNode rightNode;
    public TreeNode(int value) {
        this.value = value;
    }
    @Override
    public String toString() {
        return "value:"+value+",leftNode: "+leftNode.value+"rightNode: "+rightNode.value;
    }
}
```

**深度优先搜索：**前序遍历，中序遍历，后序遍历三种遍历方式

**前序遍历：**

```java
public static List<Integer> inorderTraversal(TreeNode root) {
        LinkedList<Integer> nodesLink = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                nodesLink.add(root.value);
                stack.push(node);
                node = node.leftNode;
            }
            node = stack.pop();
            node = node.rightNode;
        }
        return nodes;
    }

//递归实现
public static void preOrderTraverse(Node node) {
    if (node == null)
        return;
    System.out.print(node.data + " ");
    preOrderTraverse(node.left);
    preOrderTraverse(node.right);
}
```

**中序遍历：**

> 常用在在遍历树得到排序数值时

```java
//这中是不用递归调用的方式，比递归容易理解，还不怕栈溢出
public static List<Integer> inorderTraversal(TreeNode root) {
        LinkedList<Integer> nodes = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode node = root;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                stack.push(node);
                node = node.leftNode;
            }
            node = stack.pop();
            nodes.add(node.value);
            node = node.rightNode;
        }
        return nodes;
    }
// 递归实现
public static void inOrderTraverse(Node node) {
    if (node == null)
        return;
    inOrderTraverse(node.left);
    System.out.print(node.data + " ");
    inOrderTraverse(node.right);
}
```

**后序遍历：**

```java
    public static List<Integer> inorderTraversal(TreeNode root) {
        LinkedList<Integer> nodesLink = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode node = root;
        TreeNode prevNode = null;
        while (node != null || !stack.isEmpty()) {
            while (node != null) {
                stack.push(node);
                node = node.leftNode;
            }
            node = stack.peek();
            if (node.rightNode != null && node.rightNode != prevNode) {
                node = node.rightNode;
            } else {
                
                stack.pop();
                nodesLink.add(node.value);
                prevNode = node;
                node = null;
            }
        }
        return nodesLink;
    }

// 后序遍历
public static void postOrderTraverse(Node node) {
    if (node == null)
        return;
    postOrderTraverse(node.left);
    postOrderTraverse(node.right);
    System.out.print(node.data + " ");
}
```



**广度优先搜索：**



## 数据结构之堆

> 堆这个东西搞明白最大堆，最小堆的样子，和删除插入节点值后数据怎么移动就行了、堆的使用主要就是得到最大值或最小值；
>
> 最大堆就是父节点大于等于字节点，最小堆就是子节点大于等于父节点；
>
> java中堆用PriorityQueue实现，记得它不是队列，它最大堆最小堆是由构造函数中传入的比较规则设置的，默认最小堆

特性：

1. 由于最大堆最小堆的根节点就是最大值最小值，所以得到最大值最小值的时间复杂度为O(1)
2. 插入删除节点的时间复杂读为O(logn)

```java
//抛异常方法
boolean add(E e);//插入新元素
E remove();//删除堆顶元素
E element();//得到堆顶元素
//不抛异常方法
boolean offer(E e);//插入新元素
E poll();//删除堆顶元素
E peek();//得到堆顶元素
```



## 数据结构之前缀树
