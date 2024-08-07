## 排序

1. 冒泡：

   * 嵌套for循环
   * 外层循环次数从 i=0 到 数组长度-1，内层比较次数从 j=0 到 数组长度-i-1
   * 比较 J 和 J+1，大于则交换 

2. 工具类

   * 正序：Collections.sort (list)

   * 倒序：Collections.sort (list, 

     ​		new Comparator<Integer>() {
     ​        	@Override
     ​        	public int compare(Integer o1, Integer o2) {
     ​            
     ​            	return o2 - o1;
     ​        	}
     ​    })

## 查找

1. 二分查找：
   * 一个有序数组
   * 设置 left 变量默认为第一个，right 变量默认为最后一个
   * 获取居中，判断是否大于居中。如果大于则left为居中+1，right不变；如果小于居中，则left不变，righ为居中-1
   * 一直循环直至找到

## 数学

1. 是否质数
   * 除了1和本身没有其他约数（取余为零），又称素数
   * 任何一个数字n，都可以写成 n = a×b的形式。那么必然会有一个数字是小于等于根号n的。所以我们只需要求该数字的[开根号](https://so.csdn.net/so/search?q=开根号&spm=1001.2101.3001.7020)，这样子效率更高
   * 循环从 2 到 Math.sqrt(num)，判断如果存在 n%i==0，则不是