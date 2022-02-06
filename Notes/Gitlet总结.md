# Gitlet总结

## 继续实现

1.

## 乐子

1. 在实现`merge`命令时看到了这样一句话。

   > This is fine; people who produce non-standard, pathological files because they don’t know the difference between a line terminator and a line separator deserve what they get.

   ![image-20220204154705657](https://gitee.com/dongramesez/typora-img/raw/master/img/image-20220204154705657.png)

   这挺好的。这（丑陋的）结果是他们应得的。那些人产生了许多不标准且病态的文件，就因为他们不知道行终止符和行分隔符区别。\^__\^ xswl

   我赶紧去查了一下分隔符(separator)终止符(terminator)的区别。我没有查到特别清晰明确的定义。但我们要了解的是：

   - 分隔符与定界符(delimiter)是同义词，但常常用于不用的语境。分隔符常用于说明字段之间的分隔。
   - 终止符用于表示字段的结束。`\0`表示字符串的终止，`\n`表示一行的终止。在unix中`\n`既是终止符，又是行与行首尾分隔的分隔符。

