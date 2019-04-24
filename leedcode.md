# 图
## BFS
### 题一：单词接龙
```
给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：

每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:

如果不存在这样的转换序列，返回 0。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。
```
```
示例 1:

输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
```
```
示例 2:

输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```
* 【分析】：一层一层的来，看那一层找到了endword,其层数就是节点个数，beginword是第一层


![](https://note.youdao.com/yws/public/resource/383f28fda2c91ea9736e71a189c80718/xmlnote/7A708ED3F757417FBA33335539AF17D3/12977)
```java
class Solution {
       public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if(wordList == null || !wordList.contains(endWord)){
            return 0;
        }
        Queue<String> q = new LinkedList<>();
        Set<String> words = new HashSet<>(wordList);  //要用set更快才能不超时*******
        q.offer(beginWord);
        int count =1; //记录当前层数，现在头结点是第一层
        while (!q.isEmpty()){
            int size = q.size();
            count++; //我们即将遍历第二层
            for(int i=0; i<size; i++){ //我们每次只能遍历一层，一层一层的来才能统计层数
                String word = q.poll();//对当前层的每一个数，开始扫描它的下一层
                List<String> nexts = getNexts(word,words);
                //得到它的下一层节点，我们不需要判断是否之前已经存在在队列了，是因为之前入了队列的我们都从wordList删除了
                for(String next : nexts){
                    if(next.equals(endWord)){ //在当前层找到了，那么返回当前层层数
                        return count;
                    }
                    q.offer(next);
                }
            }
        }
        return 0; //BFS 宽度优先遍历完成了都没有找到，那么说明不存在这样的转换，返回0
    }

    private List<String> getNexts(String word, Set<String> wordList) {
        List<String> cardidates = new LinkedList<>();
        StringBuilder sb = new StringBuilder(word);
        for(int i=0; i<sb.length(); i++){ //对sb的每一个字母都用a-z替换一次，找wordList中有没有一样的，一样说明可以转换，是下一个
            char temp = sb.charAt(i);
            for(char c = 'a'; c<='z' ;c++){
                if(temp == c){
                    continue; //因为转换了还是自身，比如从hit转换成hit，没必要，也不符合转换的要求
                }
                sb.setCharAt(i,c);
                String newWord = sb.toString();
                if(wordList.remove(newWord)){ //返回true，说明删除成功，即这个转换是可以的
                    cardidates.add(newWord);
                }
            }
            sb.setCharAt(i,temp); // 记得要转回去，因为还要对下一位的字符进行变化
        }
        return cardidates;
    }
}
```