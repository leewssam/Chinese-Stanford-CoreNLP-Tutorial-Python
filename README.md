# Tutorial on how to install and use StanfordCore NLP Chinese on Windows and connect via Stanza [Python].


## Pre-requiste before installing StanfordCore NLP
1. Install Java Developemnt Kit (JDE)
https://www.oracle.com/java/technologies/javase-downloads.html
Please also remember add path to Java's bin or else it wont work https://stackoverflow.com/questions/25320625/jdk-appears-to-install-but-is-not-detected-and-commands-dont-work
To check whether Java is installed, try whether "javac -version" on command prompt works, if it returns Java's version, it is working. 

2. Install PyTorch
https://pytorch.org/get-started/locally/

3. Install Stanza 
https://github.com/stanfordnlp/stanza

4. Download StanfordCore NLP
https://stanfordnlp.github.io/CoreNLP/download.html
Please note, download CoreNLP Full [zipped] and its language model file [individual jar file]

## Install StanfordCore NLP
1. After download and install all the files, extract the StanfordCore NLP to [C:\StanfordCoreNLP], please also remember to extract the Chinese Model File ".jar" file to the extracted location with all the other models file.

2. Add a new system variable environment [Press Windows Key and Search "Edit the system environment variables"]
![alt text](https://raw.githubusercontent.com/leewssam/Chinese-Stanford-CoreNLP-Tutorial/master/system_variable.png?token=AFK46TMNFIVLUEGNRAID5U27K4VQA)
3. Check whether "cd %CORENLP_HOME%" on command prompt will direct to correct location, if no then its first or second step error. Google yourself.

4. Run the following command
```
cd %CORENLP_HOME%
FOR %i IN (*.jar) DO set classpath= %classpath%;%cd%\%i
```

5. After completion, run this command to run StanfordCore NLP's server
```
java -Xmx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -serverProperties StanfordCoreNLP-chinese.properties -port 9000 -timeout 15000
```

6. If it works, you should be able to see StanfordCore NLP interface at http://localhost:9000/

## Connecting Python to StanfordCore NLP
1. Run your python
2. Execute the following command
```
from  stanza.server.client import CoreNLPClient
client = CoreNLPClient(server='http://localhost:9000', annotators=['ssplit', 'lemma', 'tokenize', 'pos', 'ner'],start_server=False)
```
3. Your Python is now connected to StanfordCore NLP via Stanza, to test, try the following:
```
test_element = "深蓝的天空中挂着一轮金黄的圆月，下面是海边的沙地，都种着一望无际的碧绿的西瓜，其间有一个十一二岁的少年，项带银圈，手捏一柄钢叉，向一匹猹尽力的刺去，那猹却将身一扭，反从他的胯下逃走了。"
annotated = client.annotate(test_element)
for sentence in annotated.sentence:
    for i in sentence.token:
        print(i.word,i.ner)
```
4. Happy NLP-ing