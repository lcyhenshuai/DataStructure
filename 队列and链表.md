# Queue
队列：FIFO (先进先出)    
explain:queue is a order list,you can use array or linked list implemnet it    
默认情况下：front=0;rear=0;当插入新的队尾元素时,尾指针加一，删除队列头元素时，头指针增1    
在队列存数据的时候rear增加,front不变    
在队列取数据的时候rear不变,front往上面移动    
加数据在队列的尾部加，取数据是在队列的尾部取    
notice:如果不理解的话，就多想一想水平方向的队列，大家手拉着手，x axis     
环形队列思考:    
1.front #front就指向队列的第一个元素,也就是array[front]是队列的第一个元素,front现在初始值为0,不再是-1了.    
2.rear  #rear指向队列的最后一个元素的后一个位置. 因为希望空出一个空间作为约定.rear的初始值也为0.     
3.队列满的条件现在是(rear+1)%maxsize=front(队列满) #分析:因为rear指的是队列满的后一个元素,因为环形队列根本分不清楚到底rear=front是满了还是空的，所以空出一个位置，脑
子要想着环形，就明白了，一个circle而不是去想一个栈式的结构   
4.当队列为空的条件,rea==front空    
5.队列中的有效数据个数:(rear+maxsize-front)%maxsize   



# 链表
链表是一种存储结构，逻辑解构是顺序存储，物理结构可能是顺序存储也可能不是,增删快，查询慢   
链表是以节点的方式来存储的，每个节点中都包含data域，next域，链表不一定是链式存储的。    
链表的逻辑结构是有序的，但是链表的物理解构是不一定连续的.    
链表就两个数据域，实现的时候无非就是看坐是一个节点，每个节点都有数据域和节点域，所以每个节点类中都必须有一个节点类型的next指向下一个节点类，而他们
可以这些节点可以   
模拟单链表(java)代码：

```java
package linklist;
public class SingleLinkListDemoTest {
	//创建一个链表
	 public static void main(String[] args) {
	LinkNode node1=new LinkNode(1,"lcy");
	LinkNode node2=new LinkNode(2,"xxx");
	LinkNode node3=new LinkNode(3,"gui");
	LinkNode node4=new LinkNode(3,"梁朝阳");
	LinkedList linkList=new LinkedList();
	  linkList.add(node1);
	  linkList.add(node2);
	  linkList.add(node3);
//      linkList.update(node4);
	  linkList.list();
	  linkList.del(4);

	}

}
//链表
class LinkedList{
	public LinkNode head=new LinkNode(0, "");
	
	public void add(LinkNode linkNode) {
	//临时节点代替头节点，因为头节点不能随意移动
		LinkNode temp=head;
		while(true) {
			if(temp.next==null) {
				break;
			}
			temp=temp.next;
		}
		temp.next=linkNode;
	}
	//按照编号来添加数据
	public void addOrder(LinkNode linkNode) {
	   LinkNode temp=head;
	   boolean flag=false;//flag用来判断该编号是否已经存在
	   while(true) {
		   if(temp.next==null) {
			   break;
		   }
		   if(temp.next.n>linkNode.n) {//位置找到,就在temp后面插入
			   break;
		   }
		   else if(temp.next.n==linkNode.n) {
			   flag=true;
			   break;
		   }
		   //后移
		   temp=temp.next;
	   }
	   //判断是否存在相同的编号
	   if(flag) {
		   System.out.printf("该编号%d数据已经存在\n",temp.next.n);
	   }else {
		   linkNode.next=temp.next;
		   temp.next=linkNode;
	   }
	}
	//修改节点
	public void update(LinkNode newLinkNode) {
		LinkNode temp=head;
		boolean flag=false;
		if(head.next==null) {
		   return;
		}
		while(true) {
			//注意这里不能再是temp.next为null,这样会导致要查的数据刚好是最后一个，但是查的时候.next=null，直接return了   
			if(temp==null) {
				break;
			}
			if(temp.n==newLinkNode.n) {
				//找到数据
				flag=true;
				break;
			}
			//继续遍历
			temp=temp.next;
		}
		//根据flag找到要修改的节点
		if(flag) {
			temp.username=newLinkNode.username;
		}else {
			System.out.printf("没有找到对应的编号%d号数据\n",newLinkNode.n);
		}
		
	}
	
	//删除数据
	public void del(int n) {
		LinkNode temp=head;
		boolean flag=false;//判断是否找到该节点
		while(true) {
			if(temp.next==null) {
				System.out.println("已经没有数据了");
				return;
			}
			if(temp.next.n==n) {
				flag=true;
				break;
			}
			temp=temp.next;
		}
		if(flag) {
			temp.next=temp.next.next;
		}else {
			System.out.printf("没有你想要%d号数据\n",n);
		}
		
	}
	//展示数据
	public void list() {
		//判断链表是否为空
		if(head.next==null) {
			System.out.println("链表为空");
		}
		LinkNode temp=head;
		while(true) {
			if(temp.next==null) {
				break;
			}
			temp=temp.next;
			System.out.println(temp);
		}

	}
	
}

//节点类
class LinkNode{
     public int n;
     public String username;
     public LinkNode next;
	public LinkNode(int n, String username) {
		super();
		this.n = n;
		this.username = username;
	}
	@Override
	public String toString() {
		return "LinkNode [n=" + n + ", username=" + username + "]";
	}
	
}
```

* 去单链表的中有效节点的个数

* 查找单链表中的倒数第k个节点【新浪面试题】

  ```java
  //展示单链表中有效节点的个数
  	public static int getSize() {
  		
  		//用来计算有效节点的个数
  		int count=0;
  		if(head.next==null) {
  			return 0;
  		}
  		//头节点不算数
  	 LinkNode temp=head.next;
  	 while(temp!=null) {
  		count++;
  		//后移
  		temp=temp.next;
  	 }
  	 return count;
  	}
  	//寻找倒数第k个节点的数据，思路：先遍历求出size,再用size-index
  	public LinkNode getDescK(int k) {
  		LinkNode temp=head;
  		int size = getSize();
  		if(k<0||k>size) {
  			throw new RuntimeException("该节点数据不存在");
  		}
  		for(int i=0;i<size+1-k;i++) {
  			//后移
  			temp=temp.next;
  		}
  		return temp;
  	}
  ```

  + 单链表的反转（腾讯面试题）

    思路：

    1.定义一个节点reverseHead=new LinkNode();

    2.从头到尾遍历原来的链表，每遍历一个节点，就将其取出，并放在新的链表reverseHead的最前端.

    3.原来的链表的head.next=reverseHead.next.

  个人想法:可能我比较笨，这个单链表的反转我是头一天看了3个小时，没看懂，又查了百度，在有源码的
  	 情况下我依然看不懂，第二天中午上完课后，又自己在纸上一个劲地画图，（这个图）我昨晚也
  	 画了好几遍，但是看不懂，我c学的不好，所以当时大一的时候数据结构就听不懂，现在好不容易
  	 粗略学完了java大致的体系，依然感到空虚，就是因为算法我不会，第二天就顿悟了，希望大家也
  	 坚持下去，这种东西需要每天坚持去看       
     4.我自己顿悟后的理解：
        LinkNode cur=head.next;//这个用来操作数据的，
  //这个是真的用来保存数据的，你想，你的cur移走了，又要连在新的reverseHead的后面又要在自己后面连
  //reverseHead原本的next，才算拼接进去，那么你自己移玩了，怎么继续对原链表遍历呢，所以需要next来
  //保存原来的链表遍历的位置，这样就可以继续往后面遍历，然后不断后移

  ```	java
  LinkNode next=null;
  while(cur!=null){
  //保存原链表遍历的位置
  next=cur.next;
  //拼接到新的链表
  cur.next=reverHead.next;
  cur=reverHead.next;
  //后移
  cur=next;
  }
  ```
  +    从尾到头打印单链表【百度,要求方式1：反向遍历。方式2：Stack栈】    
      思路：
      首先，人家说的是反向遍历，没有说是反转，也可以进行反转，但是反转之后就破坏了
      链表的结构，显然不是最优的，这时候我们可以用到栈，栈先进后出，把链表遍历压栈
      再弹栈。
      这个思路还是比较简单的，因为栈用来这样逆序遍历非常好    
      
  ```java
  public static void reversePrint() {
  	if(head.next==null) {
  		return;
  	}
  	LinkNode cur=head.next;
  	Stack<LinkNode> stack=new Stack<LinkNode>();
  	while(cur!=null) {
  		stack.push(cur);
  		cur=cur.next;
  	}
  	while(stack.size()>0) {
  		System.out.println(stack.pop());
  	}
  }
  ```
  +    合并两个有序的单链表，合并之后的链表依然有序【课后练习】
   	思路：和单链表反转的想法一致，复制一个头，进行大小插入,cur来操作当前链表，next指针来保存当前节点位置，
           我的想法是先判断谁是长表谁是段表，在长表中遍历有序的短表，如果cur2.n<cur1.n，就插cur2的数据到 新的链表，但是cur2的值需要每次初始化，因此，shortList.head.next2=next;即原来的链表头部随着遍历 不断后移，达到原链表移出一个节点的效果，否则就是死循环。每次进行遍历端链表的时候都需要初始化cur2=shortList.head.next,中间一个小优化，因为链表是有序的，如果当前节点插不进去，那么后面的值肯定也插不进去直接break,省不少时间。
            ps:为了方便插入，我写了一个getRear()方法，并对以前的方法进行了优化，因为以前head头节点是static,导致,插入的时候，多个对象使用一个成员变量。我现在把他变成每个对象的，就没有问题了。
       
       ```java
       //考虑到插入数据要插到尾部，应该抽取出来一个方法，得到最后一个数据
  public LinkNode getRear(LinkedList list) {
     LinkNode cur=list.head.next;
  if(cur==null) {
  	return list.head;
  }
  	while(cur.next!=null) {
  		cur=cur.next;
  	}
  	return cur;
  	}
  	```
  	
  	插入的代码：


  ```	java
     //讲两个有序的单链表进行有序插入
        public LinkedList insertByOrder(LinkedList list1,LinkedList list2) {
      LinkedList target=new LinkedList();
  	//判断谁是长表谁是短表
  	LinkedList longList=(list1.getSize()>=list2.getSize()?list1:list2);
  	LinkedList shortList=(list1.getSize()<list2.getSize()?list1:list2);
  	LinkNode cur1=longList.head.next;
  	//保存当前遍历位置
  	LinkNode next1=null;
  	//		LinkNode cur2=shortList.head.next;
              //保存当前遍历位置
  	LinkNode next2=null;
  	//遍历长表
  	while(cur1!=null) {
  		//遍历短链表
  		LinkNode cur2=shortList.head.next;
  		while(cur2!=null) {
  			next2=cur2.next;
  			if(cur2.n<cur1.n) {
  			LinkNode rear = this.getRear(target);
  			System.out.println("cur2插后的rear:"+rear);
  			shortList.head.next=next2;
  			cur2.next=rear.next;//rear.next其实就是null,链表的最后一个元素其实就是null嘛
  			//插入到新的链表的尾部
  			rear.next=cur2;
  			}else {
  			//优化，因为链表是有序，前面的节点插不进去，后面的也不用插了
  				break;
  			}
  			//后移
  			cur2=next2;
  		}
  	 //此时说明cur1.n<cur2.n，该cur1插了
  		LinkNode rear = this.getRear(target);
  		System.out.println("cur1插后的rear:"+rear);
  		next1=cur1.next;
  		longList.head.next=next1;
  		cur1.next=rear.next;//curl.next=null
  		rear.next=cur1;
  		cur1=next1;
  	}
  	
  	return target;
  		
  	}
  ```
  + 完整代码如下：

    ```
    package linklist;
    
    import java.util.Stack;
    
    public class SingleLinkListDemoTest {
    	//创建一个链表
    	 public static void main(String[] args) {
    	LinkNode node1=new LinkNode(1,"lcy");
    	LinkNode node2=new LinkNode(4,"xxx");
    	LinkNode node3=new LinkNode(6,"gui");
    	LinkNode node4=new LinkNode(2,"haha");
    	LinkNode node5=new LinkNode(3,"baba");
    	LinkNode node6=new LinkNode(5,"xixi");
    //	LinkNode node4=new LinkNode(3,"梁朝阳");
    	LinkedList linkList=new LinkedList();
    	LinkedList linkList1=new LinkedList();
    	  linkList.add(node1);
    	  linkList.add(node2);
    	  linkList.add(node3);
    	  linkList1.add(node4);
    	  linkList1.add(node5);
    	  linkList1.add(node6);
    //      linkList.update(node4);
    //	  linkList.list();
    //	  linkList.del(4);
    //	  System.out.println(LinkedList.getSize());
    //	  System.out.println(linkList.getDescK(1));
    //	  linkList.reverseList();
    //	  linkList.reversePrint();
    	  linkList.list();
    	     System.out.println("-----");
         linkList1.list();
    
    	  System.out.println("-----");
    	  System.out.println(linkList.getRear(new LinkedList()));
    	  LinkedList insertByOrder = linkList.insertByOrder(linkList, linkList1);
    	  insertByOrder.list();
    	}
    
    }
    //链表
    class LinkedList{
    	public  LinkNode head=new LinkNode(0, "");
    	
    	public void add(LinkNode linkNode) {
    	//临时节点代替头节点，因为头节点不能随意移动
    		LinkNode temp=head;
    		while(true) {
    			if(temp.next==null) {
    				break;
    			}
    			temp=temp.next;
    		}
    		temp.next=linkNode;
    	}
    	//按照编号来添加数据
    	public void addOrder(LinkNode linkNode) {
    	   LinkNode temp=head;
    	   boolean flag=false;//flag用来判断该编号是否已经存在
    	   while(true) {
    		   if(temp.next==null) {
    			   break;
    		   }
    		   if(temp.next.n>linkNode.n) {//位置找到,就在temp后面插入
    			   break;
    		   }
    		   else if(temp.next.n==linkNode.n) {
    			   flag=true;
    			   break;
    		   }
    		   //后移
    		   temp=temp.next;
    	   }
    	   //判断是否存在相同的编号
    	   if(flag) {
    		   System.out.printf("该编号%d数据已经存在\n",temp.next.n);
    	   }else {
    		   linkNode.next=temp.next;
    		   temp.next=linkNode;
    	   }
    	}
    	//修改节点
    	public void update(LinkNode newLinkNode) {
    		LinkNode temp=head;
    		boolean flag=false;
    		if(head.next==null) {
    		   return;
    		}
    		while(true) {
       //注意这里不能再是temp.next为null,这样会导致要查的数据刚好是最后一个，但是查的时候.next=null，直接return了
    			if(temp==null) {
    				break;
    			}
    			if(temp.n==newLinkNode.n) {
    				//找到数据
    				flag=true;
    				break;
    			}
    			//继续遍历
    			temp=temp.next;
    		}
    		//根据flag找到要修改的节点
    		if(flag) {
    			temp.username=newLinkNode.username;
    		}else {
    			System.out.printf("没有找到对应的编号%d号数据\n",newLinkNode.n);
    		}
    		
    	}
    	
    	//删除数据
    	public void del(int n) {
    		LinkNode temp=head;
    		boolean flag=false;//判断是否找到该节点
    		while(true) {
    			if(temp.next==null) {
    				System.out.println("已经没有数据了");
    				return;
    			}
    			if(temp.next.n==n) {
    				flag=true;
    				break;
    			}
    			temp=temp.next;
    		}
    		if(flag) {
    			temp.next=temp.next.next;
    		}else {
    			System.out.printf("没有你想要%d号数据\n",n);
    		}
    		
    	}
    	//展示数据
    	public void list() {
    		//判断链表是否为空
    		if(head.next==null) {
    			System.out.println("链表为空");
    		}
    		LinkNode temp=head;
    		while(true) {
    			if(temp.next==null) {
    				break;
    			}
    			temp=temp.next;
    			System.out.println(temp);
    		}
    
    	}
    	//展示单链表中有效节点的个数
    	public  int getSize() {
    		
    		//用来计算有效节点的个数
    		int count=0;
    		if(head.next==null) {
    			return 0;
    		}
    		//头节点不算数
    	 LinkNode temp=head.next;
    	 while(temp!=null) {
    		count++;
    		//后移
    		temp=temp.next;
    	 }
    	 return count;
    	}
    	//寻找倒数第k个节点的数据，思路：先遍历求出size,再用size-index
    	public LinkNode getDescK(int k) {
    		LinkNode temp=head;
    		int size = getSize();
    		if(k<0||k>size) {
    			throw new RuntimeException("该节点数据不存在");
    		}
    		for(int i=0;i<size+1-k;i++) {
    			//后移
    			temp=temp.next;
    		}
    		return temp;
    	}
    	//单链表反转
    	public  void reverseList() {
    		LinkNode reverseHead=new LinkNode(0,"");
    		LinkNode cur=head.next;
    		LinkNode next=null;
    		while(cur!=null) {
    			//保存原来节点遍历的位置
    			next=cur.next;
    			cur.next=reverseHead.next;
    			reverseHead.next=cur;
    			cur=next;
    		}
    		head.next=reverseHead.next;
    		
    	}
    	//单链表进行逆序输出，用栈
    	public  void reversePrint() {
    		if(head.next==null) {
    			return;
    		}
    		LinkNode cur=head.next;
    		Stack<LinkNode> stack=new Stack<LinkNode>();
    
    		while(cur!=null) {
    			stack.push(cur);
    			cur=cur.next;
    		}
    		while(stack.size()>0) {
    			System.out.println(stack.pop());
    		}
    	}
    	//讲两个有序的单链表进行有序插入
    	public LinkedList insertByOrder(LinkedList list1,LinkedList list2) {
    		LinkedList target=new LinkedList();
    		//判断谁是长表谁是短表
    		LinkedList longList=(list1.getSize()>=list2.getSize()?list1:list2);
    		LinkedList shortList=(list1.getSize()<list2.getSize()?list1:list2);
    		LinkNode cur1=longList.head.next;
    		//保存当前遍历位置
    		LinkNode next1=null;
    //		LinkNode cur2=shortList.head.next;
    		//保存当前遍历位置
    		LinkNode next2=null;
    		//遍历长表
    		while(cur1!=null) {
    			//遍历短链表
    			LinkNode cur2=shortList.head.next;
    			while(cur2!=null) {
    				next2=cur2.next;
    				if(cur2.n<cur1.n) {
    				LinkNode rear = this.getRear(target);
    				System.out.println("cur2插后的rear:"+rear);
    				shortList.head.next=next2;
    				cur2.next=rear.next;//rear.next其实就是null,链表的最后一个元素其实就是null嘛
    				//插入到新的链表的尾部
    				rear.next=cur2;
    				}else {
    					//优化，因为链表是有序，前面的节点插不进去，后面的也不用插了
    					break;
    				}
    				//后移
    				cur2=next2;
    			}
    		 //此时说明cur1.n<cur2.n，该cur1插了
    			LinkNode rear = this.getRear(target);
    			System.out.println("cur1插后的rear:"+rear);
    			next1=cur1.next;
    			longList.head.next=next1;
    			cur1.next=rear.next;//curl.next=null
    			rear.next=cur1;
    			cur1=next1;
    		}
    		
    		return target;
    			
    		}
    		
    	
    	//考虑到插入数据要插到尾部，应该抽取出来一个方法，得到最后一个数据
    	public LinkNode getRear(LinkedList list) {
    		LinkNode cur=list.head.next;
    		if(cur==null) {
    			return list.head;
    		}
    		while(cur.next!=null) {
    			cur=cur.next;
    		}
    		return cur;
    	}
    }
    
    //节点类
    class LinkNode{
         public int n;
         public String username;
         public LinkNode next;
    	public LinkNode(int n, String username) {
    		super();
    		this.n = n;
    		this.username = username;
    	}
    	@Override
    	public String toString() {
    		return "LinkNode [n=" + n + ", username=" + username + "]";
    	}
    	
    }
    ```

    

  + 个人想法：
    可能我真的不适合当程序员，我感觉我好菜，这么简单的逻辑想了这么久，别人都学的会，而我不行，我只想继续努力，我真的很喜欢计算机，但是梦我却实现不了，希望你不要
    像我一样迷茫，一名迷茫的大四学生。

    #####  双向链表

    1.   遍历，添加，修改，删除

    2.   遍历

       ​	从前往后遍历

    3.   修改

    4.   添加

       ​	添加到双向链表的尾部

       ​	temp.next=new LinkNode;

       ​	new LinkNode.pre=temp;

    5.   删除

       ​	双向链表能够实现自我删除，不需要借助辅助节点，因为单链表的删除要考虑到前一个节点的next指向下一个节点。双向链表：

       temp.pre.next=temp.next;

       //如果删除的是尾节点的话，不需要在为null来指定pre节点了

       if(temp==null){}

       else{

       temp.next.pre=temp.pre;

       }

       课堂作业：

       实现双向链表的有序添加，思路：参照单向链表的有序添加即可

       ```java
       public void addOrder(LinkNode2 linkNode) {
       	   LinkNode2 temp=head;
       	   boolean flag=false;//flag用来判断该编号是否已经存在
       	   while(true) {
       		   if(temp.next==null) {
       			   break;
       		   }
       		   if(temp.next.n>linkNode.n) {//位置找到,就在temp后面插入
       			   break;
       		   }
       		   else if(temp.next.n==linkNode.n) {
       			   flag=true;
       			   break;
       		   }
       		   //后移
       		   temp=temp.next;
       	   }
       	   //判断是否存在相同的编号
       	   if(flag) {
       		   System.out.printf("该编号%d数据已经存在\n",temp.next.n);
       	   }else {
       		   linkNode.next=temp.next;
       		   temp.next=linkNode;
       		   linkNode.pre=temp;
       		   
       	   }
       	}
       ```

       

       总的代码：ps:其实就是多加了一个pre指针的问题，大部分都和单向链表一样。

       ```
       package linklist;
       
       public class DoubleLinkListDemo {
         	
       	public static void main(String[] args) {
       		DoubleLinkList list=new DoubleLinkList();
       		// TODO Auto-generated method stub
       		LinkNode2 node1=new LinkNode2(1,"lcy");
       		LinkNode2 node2=new LinkNode2(4,"xxx");
       		LinkNode2 node3=new LinkNode2(6,"gui");
       		LinkNode2 node4=new LinkNode2(5,"wyl");
       		list.add(node1);
       		list.add(node2);
       		list.add(node3);
       //		list.update(node4);
       //		list.del(6);
       		list.addOrder(node4);
       		list.list();
       	}
       
       }
       class DoubleLinkList{
       	public LinkNode2 head=new LinkNode2(0,"");
       	public void add(LinkNode2 linkNode) {
       	//临时节点代替头节点，因为头节点不能随意移动
       		LinkNode2 temp=head;
       		while(true) {
       			if(temp.next==null) {
       				break;
       			}
       			temp=temp.next;
       		}
       		//已经到达双向链表的尾部了，进行添加操作
       		temp.next=linkNode;
       		linkNode.pre=temp;
       	}
       	//按照编号来添加数据
       
       	public void addOrder(LinkNode2 linkNode) {
       	   LinkNode2 temp=head;
       	   boolean flag=false;//flag用来判断该编号是否已经存在
       	   while(true) {
       		   if(temp.next==null) {
       			   break;
       		   }
       		   if(temp.next.n>linkNode.n) {//位置找到,就在temp后面插入
       			   break;
       		   }
       		   else if(temp.next.n==linkNode.n) {
       			   flag=true;
       			   break;
       		   }
       		   //后移
       		   temp=temp.next;
       	   }
       	   //判断是否存在相同的编号
       	   if(flag) {
       		   System.out.printf("该编号%d数据已经存在\n",temp.next.n);
       	   }else {
       		   linkNode.next=temp.next;
       		   temp.next=linkNode;
       		   linkNode.pre=temp;
       		   
       	   }
       	}
       	//展示数据
       	public void list() {
       		//判断链表是否为空
       		if(head.next==null) {
       			System.out.println("链表为空");
       		}
       		LinkNode2 temp=head;
       		while(true) {
       			if(temp.next==null) {
       				break;
       			}
       			temp=temp.next;
       			System.out.println(temp);
       		}
       
       	}
       	//修改节点
       	public void update(LinkNode2 newLinkNode) {
       		LinkNode2 temp=head;
       		boolean flag=false;
       		if(head.next==null) {
       		   return;
       		}
       		while(true) {
          //注意这里不能再是temp.next为null,这样会导致要查的数据刚好是最后一个，但是查的时候.next=null，直接return了
       			if(temp==null) {
       				break;
       			}
       			if(temp.n==newLinkNode.n) {
       				//找到数据
       				flag=true;
       				break;
       			}
       			//继续遍历
       			temp=temp.next;
       		}
       		//根据flag找到要修改的节点
       		if(flag) {
       //			temp.username=newLinkNode.username;
       			newLinkNode.next=temp.next;
       			newLinkNode.pre=temp.pre;
       			temp.pre.next=newLinkNode;	
       			
       		}else {
       			System.out.printf("没有找到对应的编号%d号数据\n",newLinkNode.n);
       		}
       		
       	}
       	//删除数据
       	public void del(int n) {
       		//不需要借助辅助节点来定位到要删除节点的上一个节点了，直接找当前节点就可以了
       		LinkNode2 temp=head.next;
       		boolean flag=false;//判断是否找到该节点
       		while(true) {
       			if(temp==null) {
       				System.out.println("已经没有数据了");
       				return;
       			}
       			if(temp.n==n) {
       				flag=true;
       				break;
       			}
       			temp=temp.next;
       		}
       		if(flag) {
       			//要删除的节点操作
       			temp.pre.next=temp.next;
       			//但是如果是尾节点的temp.next会报NullPointExcepiton异常，所以需要加一个if来判断是否是结尾了
       			if(temp.next==null) {
       			}else {
       			temp.next.pre=temp.pre;
       			}
       
       		}else {
       			System.out.printf("没有你想要%d号数据\n",n);
       		}
       		
       	}
       }
       //节点类2
       class LinkNode2{
           public int n;
           public String username;
           public LinkNode2 next;
           public LinkNode2 pre;
       	public LinkNode2(int n, String username) {
       		super();
       		this.n = n;
       		this.username = username;
       	}
       	@Override
       	public String toString() {
       		return "LinkNode [n=" + n + ", username=" + username + "]";
       	}
       	
       }
       
       ```

       

     #####  Josephus problem(约瑟夫问题)

  问题描述：一群人围成一个圈子，其中从第一个开始数一个数如果是m,第m个人出队列，接着从下一个人开始继续数数，数到m,出列，下一个接着数，数到m出队列，直到全部出队列为止。

  思路：使用单向环形列表来实现

  ​			如何制作单项循环列表呢，first节点一开始指向自己，制作一个环形，借助cur指针，来使得cur.next不断后移，但是first指针位置不变。

  思路：这个实现其实很简单，因为是单链表，所以需要你构建一个环，删除的时候，就借助两个指针，因为你需要考虑当前节点删掉后，上一个节点的next要指向被删除节点的next域，和单链表没什么区别，难点再遇构建环状链表：

  ```java
  package linklist;
  
  public class JosephusProbleam {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		CircleLinkList circlelist=new CircleLinkList();
  	     circlelist.addNum(6);
  	     circlelist.outofCircle(3);
  	     circlelist.list();
  	}
  	
  
  
  }
  class CircleLinkList{
  	public CircleLinkNode first=null;
  
  	public void addNum(int num) {
  	
  		if(num<1) {
  			System.out.println("请添加正确的节点数字");
  		}
  		//辅助指针,帮助构建环形链表
  		CircleLinkNode cur=null;
  			for(int i=1;i<=num;i++) {
  				CircleLinkNode node=new CircleLinkNode(i);
  			 //如果是第一个节点
  			 if(i==1) {
  				 first=node;
  				 first.next=first;
  				 cur=first;//让cur指向第一个第一个节点
  			 }else {
  			 cur.next=node;
  			 node.next=first;
  			 cur=node;}
  		}
  	}
  	//遍历
  	public void list() {
  		CircleLinkNode cur=first;
  		if(cur==null) {
  			System.out.println("当前环形链表没有数据");
  			return;
  		}
  		while(true) {
  			System.out.println(cur);
  			if(cur.next==first) {
  				break;
  			}
  			cur=cur.next;
  		}
  	}
  	//出圈问题
  	public void outofCircle(int m) {
  		if(first.next==null||m<0) {
  			System.out.println("当前数据不能进行Josephus操作");
  		}
  	CircleLinkNode helper=first;
  	//遍历找到最后一个元素
  	while(true) {
  		if(helper.next==first) {
  			break;
  		}
  		helper=helper.next;
  	}
  	//出圈是一个循环，因为最后只能有一个人活下来
  	while(true) {
  		//说明圈内只剩下一个元素二楼
  		if(first.next==first) {
  			break;
  		}
  		//除去自身，遍历m-1次 2
  		for(int i=0;i<m-1;i++) {
  			first=first.next;
  			helper=helper.next;	
  		}
  		//出圈
  		System.out.println("当前出圈的是:"+first.no);
  		helper.next=first.next;
  		first=first.next;
  	}
  		
  		
  		
  	}
  	
  }
  	
  class CircleLinkNode {
  	public int no;
  	public CircleLinkNode next;
  	public CircleLinkNode(int no) {
  		this.no=no;	
  	}
  	@Override
  	public String toString() {
  		return "LinkNode [no=" + no + "]";
  	}
  	
  }
  ```

  小节：至此，链表的操作我已经跟着视频按照自己的逻辑独立敲完，每一个bug都是自己调的，因为我觉得人学习就要学习思路，而不是抄代码，而我这方面也很薄弱，每次都耗费好久时间。我觉得链表操作，考验的就是遍历，如果是单链表就要考虑，被删除节点的上一个节点的Next域的问题，双向链表操作简便了很多，真的很开心，写代码，虽然不像做项目那样cv,cv块，有页面效果，但是这个逻辑需要自己用代码实现，挺有意思，我还学会了用图片区分析，写一些伪代码，我觉得这对我来说就是进步。


























