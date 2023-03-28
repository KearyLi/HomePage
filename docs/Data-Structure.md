# 数据结构与算法

> 高性能、快速、优雅、思想

> 整数：java中基本类型(byte、short、int、long等)有不同的空间限制，处理时考虑空间溢出的问题，且负数转正数容易溢出，比如-2^31^~2^31^-1，它最小负数转最大正数就多了1，溢出发生。
>
> 二进制：二进制数主要用法就是0和1的与、或、非、异或、左移、右移操作。
>
> 在考虑算法时，想想空间复杂度(解法需要的内存空间)和时间复杂度(解法需要的时间)，从而得到最优算法。
>
> 解决一件事时，想想每种数据结构的特性，由于每种特性有优点缺点，和并发处理一样，这时就得组合用好每种数据结构的优点，从而得到最佳解决办法。就像一个团体一样，每个人发表自己独特看法，从而得到最好的解决方案。



## 数据结构之数组

特性：

1. 相同类型元素的集合
2. 内存中连续顺序存放
3. 每次创建得预先固定数组空间大小，用下标获取元素
4. 读O(1)，查O(n)，插O(n)，删O(n)

常用操作：

1. 双指针，两个指向数组元素位置的指针，两个指针分别/同时指向数组头元素和尾元素

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

特性：

1. 内部采用连续字符数组实现
2. 不变性，对字符串的修改不影响最初字符串，而是产生新字符串，比如str.toUpperCase()后str的值不变







## 数据结构之链表

**特性：**

1. 内存中的链表不是连续的，它是由节点指向来指向下一个节点
2. 创建不需要一次分配内存，可以动态添加删除
3. 增删改查都是改变节点的指向
4. 读O(n)，查O(n)，插O(1)，删O(1)

**单向链表：**

> 哨兵节点：用哨兵节点遍历链表，而且能防止链表为空时

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

用哨兵节点删除指定节点：删除时将指定节点的前一个节点指向指定节点的下一个节点

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

> 双指针：

1. 前后双指针：经典应用是查找链表倒数第N的节点，办法就是将第一个指针指向头结点并向后走N-1步，随后将第二个指针指向头结点，然后两个节点以相同速度向后移动，当第一个指针指向末尾节点时，第二个指针指向位置就是链表倒数第N的节点。
2. 快慢双指正：经典应用是查找链表中间节点；AB指针指向头结点，然后A向后走两步，B向后走一步，当A走向尾节点时，B的位置就只链表中间节点。

>  反转链表：由于单向链表只能从前到后遍历，所以有的情况需要从后到前遍历，就用反转链表



**双向链表：**

> 单向链表其实没啥，但是双向链表处理是得多注意指向的问题，防止丢失指向问题

**循环链表：**

> 循环链表主要就是注意死循环问题



## 数据结构之哈希表

> HashSet和HashMap；哈希表在读查插删挺快的，但是在对元素排序上不如TreeSet/TreeMap，在求元素中最大值最小值时没有堆好，而且哈希表查找元素得用完整Key来找，所以在使用部分key查找的情况用前缀树

特性：

1. 
2. 读O(1)，查O(1)，插O(1)，删O(1)

哈希表的设计：这个哈希表是新事物，为了让它存元素值后对元素操作的时间复杂度都为O(1)，刚好的方式是用数组来存元素的哈希值，数组的长度一开始是给定的，元素的存放位置为哈希值除数组长度取余，比如哈希值为5存入长度为4的数组中就存在数组1的位置，然后依次按这个规律存；完事会发现元素过多后导致多个元素存在数组的同一位置，这时就将这同一数组位置的元素用链表存放，就是为了让哈希表时间复杂度为O(1)，但是链表太长也会违背这个时间复杂度，所以得在当元素多于数组的一定程度后，用新的更长的数组来存放元素。这就是哈希表的设计原理。



## 数据结构之栈

> Stack的push、pop操作栈，在需要后入先出的存数据场景下使用





场景一：用后缀表达式计算结果，就是用{1,2,3,*,+}来计算，这种计算得先将1,2,3入栈，然后发现下一个是操作符就把栈里面的2,3取出来计算得到结果6继续放到栈里面，然后发现下一个还是操作符，就将栈里面的1,6取出计算得到结果7。

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







## 数据结构之队列

> 有先进先出的场景使用
>
> 这个不能直接用Queue，它是一个接口，java使用它的实现类LinkedList，数组队列ArrayQueue，优先级队列PriorityQueue

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



## 数据结构之树

> 树就是一个根节点没有父节点，叶子节点没有子节点的结构
>
> 它的实现和链表相似，一个类中有三个值：当前值变量，指向左子节点的引用，指向右子节点的引用

```java
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

前序遍历：

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
```

中序遍历：

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
```

后序遍历：

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
```



**广度优先搜索：**



## 数据结构之堆





