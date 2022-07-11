```java
package com.chenxi.huawei;

import org.junit.Test;

import java.lang.reflect.Parameter;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.*;

/**
 * @Author cxi
 * @Date 2022/7/11 18:51
 */
public class StringTest {
    public static void main(String[] args) {

    }

    @Test
    public void founction() {
        String str = "ChenxiWangying";
        //3.charAt(int index)从字符中取出指定索引的值
        System.out.println(str.charAt(3));
        //4.indexOf(String str)方法,查找对应字符在字符串中的索引位置，如果没有则返回-1，常与3配合使用，
        System.out.println(str.indexOf("i"));
        //lastIndexOf(String str)方法，查找对应字符最后在字符串中出现的索引位置，如果没有则返回-1
        System.out.println(str.lastIndexOf("i"));

        //toCharArray()方法，将字符串变成一个数组
        char[] charArray = str.toCharArray();
        for (char c : charArray) {
            System.out.print(c);
        }
        System.out.println(Arrays.toString(charArray));

        //toUpperCase()将字符串全部转换为大写
        String strBig = str.toUpperCase();
        System.out.println(strBig);

        //8.toLowerCase()将字符串全部转换为小写
        String strSmall = str.toLowerCase();
        System.out.println(strSmall);

        //split("字符")，根据给定的正则表达式来拆分字符串，形成一个String数组
        String[] strArray = str.split("");
        System.out.println(Arrays.toString(strArray));

        //trim()方法，去除字符串左右两端的空白，该方法只能去除左右，中间的没办法
        String strSpaces = "  Chenxi Wangying  ";
        System.out.println(strSpaces.trim());

        //11.substring(int beginIndex,int endIndex)截取字符串
        System.out.println(str.substring(1, 3));
        System.out.println(str.substring(4));

        //12.equalsIgnoreCase(String str),忽略字符串大小比较字符串的值，
        String strA = "absC";
        String strB = "absc";
        System.out.println(strA.equalsIgnoreCase(strB));

        // concat(String str),将str的字符串的内容添加到字符串的后面，效果等同于+
        System.out.println(str.concat("addTest"));

        //replace(char oldChar,char newChar),该方法用字符newChar替换掉当前字符串中所有的oldChar。
        System.out.println(str.replace("i", "???"));

        //replaceFirst(String regex,String replacement),该方法用字符replacement替换掉当前字符串中第一个匹配regex。
        System.out.println(str.replaceFirst("h", "???"));

        //17.startsWith(String prefix)，比较该字符串是否以prefix子字符串开始的
        System.out.println(str.startsWith("Chen"));
        System.out.println(str.startsWith("chen"));

        //endsWith(String prefix)，比较该字符串是否以prefix结尾的
        System.out.println(str.endsWith("ing"));

        //19.valueOf(Type type)用于将基本数据类型转换为String类型，补充一点，type不能为null，不然会报空指针异常
        String strType = String.valueOf(21);
        System.out.println(strType);

        //20.getBytes()，将该字符串转换为字节数组
        String strByte = "123421421";
        byte[] bytes = strByte.getBytes();
        System.out.println(Arrays.toString(bytes));

        //21.String.format()方法，字符串类型格式话
        //format(String format,Object obj)，新字符串使用本地语言环境，制定字符串格式和参数生成格式的新字符串
        //format(Locale locale,String format,Object obj),使用指定语言环境，制定字符串格式和参数生成格式的新字符串

        Date date = new Date();
        //c的使用
        System.out.printf("全部日期和时间信息：%tc%n", date);
        //f的使用
        System.out.printf("年-月-日格式：%tF%n", date);
        //d的使用
        System.out.printf("月/日/年格式：%tD%n", date);
        //r的使用
        System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n", date);
        //t的使用
        System.out.printf("HH:MM:SS格式（24时制）：%tT%n", date);
        //R的使用
        System.out.printf("HH:MM格式（24时制）：%tR", date);

    }

}
```