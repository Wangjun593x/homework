# homework
Individual Project
Compare
package 统计单词个数;

import java.util.Comparator;
import java.util.Map;
import java.util.Map.Entry;
 
public class Compare implements Comparator<Map.Entry<String, Integer>> {
    public int compare(Entry<String, Integer> left, Entry<String, Integer> right) {
        return left.getValue().compareTo(right.getValue());
    }
}
Counts
package 统计单词个数;

import java.io.BufferedReader;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Counts {
    public static void main(String[] args) throws Exception {
 
        BufferedReader reader = new BufferedReader(new FileReader(
                "D:\\main.txt"));
        StringBuffer buffer = new StringBuffer();
        String line = null;
        while ((line = reader.readLine()) != null) {
            buffer.append(line);
        }
        reader.close();
        Pattern expression = Pattern.compile("[a-zA-Z']+");// 定义正则表达式匹配单词
        String string = buffer.toString();
        Matcher matcher = expression.matcher(string);//
        Map<String, Integer> map = new TreeMap<String, Integer>();
        String word = "";
        int times = 0;
        while (matcher.find()) {// 是否匹配单词
            word = matcher.group();// 得到一个单词-树映射的键
            if (map.containsKey(word)) {// 如果包含该键，单词出现过
                times = map.get(word);// 得到单词出现的次数
                map.put(word, times + 1);
            } else {
                map.put(word, 1);// 否则单词第一次出现，添加到映射中
            }
        }
        /*
         * 核心：如何按照TreeMap 的value排序而不是key排序.将Map.Entry放在集合里， 重写比较器，在用
         * Collections.sort(list, comparator);进行 排序
         */
 
        List<Map.Entry<String, Integer>> list = new ArrayList<Map.Entry<String, Integer>>(
                map.entrySet());
        /*
         * 重写比较器
         * 取出单词个数（value）比较
         */
        Comparator<Map.Entry<String, Integer>> comparator = new Comparator<Map.Entry<String, Integer>>() {
            public int compare(Map.Entry<String, Integer> left,
                    Map.Entry<String, Integer> right) {
                return (left.getValue()).compareTo(right.getValue());
            }
        };
        Collections.sort(list, comparator);// 排序
        // 打印最多五个
        int last = list.size() - 1;
        for (int i = last; i > last - 5; i--) {
            String key = list.get(i).getKey();
            Integer value = list.get(i).getValue();
            System.out.println(key + " :" + value);
        }
    }
}
Frequency
package 统计单词个数;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.util.Collections;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Frequency {
    public void count() throws Exception {
        File file = new File("D:\\main.txt");
        FileReader fileReader = new FileReader(file);
        BufferedReader reader = new BufferedReader(fileReader);
        StringBuilder builder = new StringBuilder();
        String line = "";
        while ((line = reader.readLine()) != null) {
            builder.append(line);
        }
        Pattern pattern = Pattern.compile("[a-zA-Z]+");
        String content = builder.toString();
        Matcher matcher = pattern.matcher(content);
        Map<String, Integer> map = new HashMap<String, Integer>();
        String word = "";
        Integer times = 0;
        while (matcher.find()) {
            word = matcher.group();
            if (map.containsKey(word)) {
                times = map.get(word);
                map.put(word, times+1);
            } else {
                map.put(word, 1);
            }
        }
        List<Map.Entry<String, Integer>> list = new LinkedList<Map.Entry<String, Integer>>(
                map.entrySet());// put Entry to List
        Compare compare = new Compare();//rewrite Comparator
        for (int i = 0; i < 5; i++) {
            Map.Entry<String, Integer> entry = Collections.max(list, compare);// max
            String key = entry.getKey();
            Integer value = entry.getValue();
            int index = list.indexOf(entry);//get max's index
            System.out.println(key + " " + value);
            list.remove(index);//remove max
 
        }
    }
 
    public static void main(String[] args) {
 
        try {
            Frequency frequency = new Frequency();
            frequency.count();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
