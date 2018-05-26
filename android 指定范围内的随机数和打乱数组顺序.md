# Android 指定范围内的随机数和打乱数组顺序


####1、指定范围内的随机数

```
Random random = new Random();
int s = random.nextInt(max) % (max - min + 1) + min;
```

####2、打乱数组顺序

```
//  打乱数组顺序
    public String[] array(String[] data) {
        String[] strings = new String[data.length];        //   打乱顺序之后的数组
        int count = data.length;                           //   数组长度
        int randCount = 0;                                 //   索引
        int position = 0;                                  //   位置
        int k = 0;                                         //   初始值                     
        do {
            Random rand = new Random();
            int r = count - randCount;                   //   定义一个中间变量,数组长度减去索引
            position = rand.nextInt(r);                  //   
            strings[k++] = data[position];               //   新数组第k++个元素的值等于原数组经过random打乱的值
            randCount++;
            data[position] = data[r - 1];                // 将最后一位数值赋值给已经被使用的position
        } while (randCount < count);
        return strings;
    }
```
