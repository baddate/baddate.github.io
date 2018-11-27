---
title: Lexical_analyzer
date: 2018-11-27
lastmod: 2018-11-27 20:44:01
url:
tags:
    - Compiler
categories:
    - 2018-11
---
# Lexical Analyzer Report
## DFA
![dfa](/static/All_in_one.jpeg "DFA")
**Note:**       
```
DEC代表十进制数
OCT代表八进制数
HEX代表十六进制数
SYMBOL代表操作符或者分隔符
IDENTIFIER代表标识符
DELIMITER代表代表分隔符
STRING_DELIM代表 “ 和 ‘
OPERATOR代表操作符
OTHER代表其他情况
```

## Test Code
```java
public class test{
        private int num = 3;//test comment
        public static void main(String[] args) {
        
        try {
            Lexical_analyzer analyzer = new Lexical_analyzer(new File("res/test.txt"));
            while (analyzer.hasNextWord()) {
                System.out.println(analyzer.nextWord());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
};
```
## Result
    Undefined=0
    Keyword=1
    Identifier=2
    Operator=3
    Constant=4
    Delimiter-5
    Comment=6

```
Word [ type=1, value="public"]
Word [ type=1, value="class"]
Word [ type=2, value="test"]
Word [ type=5, value="{"]
Word [ type=1, value="private"]
Word [ type=1, value="int"]
Word [ type=2, value="num"]
Word [ type=3, value="="]
Word [ type=4, value="3"]
Word [ type=5, value=";"]
Word [ type=6, value="//test comment
"]
Word [ type=1, value="public"]
Word [ type=1, value="static"]
Word [ type=1, value="void"]
Word [ type=2, value="main"]
Word [ type=5, value="("]
Word [ type=2, value="String"]
Word [ type=5, value="["]
Word [ type=5, value="]"]
Word [ type=2, value="args"]
Word [ type=5, value=")"]
Word [ type=5, value="{"]
Word [ type=1, value="try"]
Word [ type=5, value="{"]
Word [ type=2, value="Lexical_analyzer"]
Word [ type=2, value="analyzer"]
Word [ type=3, value="="]
Word [ type=1, value="new"]
Word [ type=2, value="Lexical_analyzer"]
Word [ type=5, value="("]
Word [ type=1, value="new"]
Word [ type=2, value="File"]
Word [ type=5, value="("]
Word [ type=4, value=""res/test.txt""]
Word [ type=5, value=")"]
Word [ type=5, value=")"]
Word [ type=5, value=";"]
Word [ type=1, value="while"]
Word [ type=5, value="("]
Word [ type=2, value="analyzer"]
Word [ type=5, value="."]
Word [ type=2, value="hasNextWord"]
Word [ type=5, value="("]
Word [ type=5, value=")"]
Word [ type=5, value=")"]
Word [ type=5, value="{"]
Word [ type=2, value="System"]
Word [ type=5, value="."]
Word [ type=2, value="out"]
Word [ type=5, value="."]
Word [ type=2, value="println"]
Word [ type=5, value="("]
Word [ type=2, value="analyzer"]
Word [ type=5, value="."]
Word [ type=2, value="nextWord"]
Word [ type=5, value="("]
Word [ type=5, value=")"]
Word [ type=5, value=")"]
Word [ type=5, value=";"]
Word [ type=5, value="}"]
Word [ type=5, value="}"]
Word [ type=1, value="catch"]
Word [ type=5, value="("]
Word [ type=2, value="Exception"]
Word [ type=2, value="e"]
Word [ type=5, value=")"]
Word [ type=5, value="{"]
Word [ type=2, value="e"]
Word [ type=5, value="."]
Word [ type=2, value="printStackTrace"]
Word [ type=5, value="("]
Word [ type=5, value=")"]
Word [ type=5, value=";"]
Word [ type=5, value="}"]
Word [ type=5, value="}"]
Word [ type=5, value="}"]
Word [ type=5, value=";"]

```
