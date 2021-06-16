# office-automation

作业

**1.如果已有的文件以写模式打开，会发生什么？**

文件若存在则从文件尾部以追加的方式开始写，文件原来存在的内容不会清除（除了文件尾标志EOF），若不存在则根据文件名创建新文件并只写打开

**2.read() 和 readlines() 方法之间的区别是什么？**

read()方法从文件当前位置起读取size个字节，若无参数size，则表示读取至文件结束为止，它范围为字符串对象。readlines()方法读取整个文件所有行，保存在一个列表(list)变量中，每行作为一个元素，但读取大文件会比较占内存

**3.综合练习：
一、生成随机的测验试卷文件
假如你是一位地理老师， 班上有 35 名学生， 你希望进行美国各州首府的一个小测验。不妙的是，班里有几个坏蛋， 你无法确信学生不会作弊。你希望随机调整问题的次序， 这样每份试卷都是独一无二的， 这让任何人都不能从其他人那里抄袭答案。当然，手工完成这件事又费时又无聊。 好在， 你懂一些 Python。
下面是程序所做的事：

+ 创建 35 份不同的测验试卷。
+ 为每份试卷创建 50 个多重选择题，次序随机。
+ 为每个问题提供一个正确答案和 3 个随机的错误答案，次序随机。
+ 将测验试卷写到 35 个文本文件中。
+ 将答案写到 35 个文本文件中。
这意味着代码需要做下面的事：
+ 将州和它们的首府保存在一个字典中。
+ 针对测验文本文件和答案文本文件，调用 open()、 write()和 close()。
+ 利用 random.shuffle()随机调整问题和多重选项的次序。

代码如下：
> 使用jupyter编辑
```import random

capitals = {'Alabama': 'Montgomery', 'Alaska': 'Juneau', 'Arizona': 'Phoenix',
            'Arkansas': 'Little Rock', 'California': 'Sacramento', 'Colorado':
            'Denver','Connecticut': 'Hartford', 'Delaware': 'Dover', 'Florida':
            'Tallahassee','Georgia': 'Atlanta', 'Hawaii': 'Honolulu', 'Idaho':
            'Boise', 'Illinois': 'Springfield', 'Indiana': 'Indianapolis', 'Iowa':
            'Des Moines', 'Kansas': 'Topeka', 'Kentucky': 'Frankfort', 'Louisiana':
            'Baton Rouge', 'Maine': 'Augusta', 'Maryland': 'Annapolis', 'Massachusetts':
            'Boston', 'Michigan': 'Lansing', 'Minnesota': 'Saint Paul', 'Mississippi':
            'Jackson', 'Missouri': 'Jefferson City', 'Montana': 'Helena', 'Nebraska':
            'Lincoln', 'Nevada': 'Carson City', 'New Hampshire': 'Concord',
            'New Jersey': 'Trenton', 'New Mexico':'Santa Fe', 'New York': 'Albany', 'North Carolina': 'Raleigh',
            'North Dakota': 'Bismarck', 'Ohio': 'Columbus', 'Oklahoma': 'Oklahoma City',
            'Oregon': 'Salem', 'Pennsylvania': 'Harrisburg', 'Rhode Island': 'Providence',
            'South Carolina': 'Columbia', 'South Dakota': 'Pierre', 'Tennessee':
            'Nashville', 'Texas': 'Austin', 'Utah': 'Salt Lake City', 'Vermont':
            'Montpelier', 'Virginia': 'Richmond', 'Washington': 'Olympia', 'West Virginia':
            'Charleston', 'Wisconsin': 'Madison', 'Wyoming': 'Cheyenne'}


for quizNum in range(35):
    # 创建试卷和答案文件
    quizFile = open('capticalsquiz%s.txt' % (quizNum + 1), 'w')
    answerKeyFile = open('capticalsquiz_answers%s.txt' % (quizNum + 1), 'w')

    # 写标题和开头
    quizFile.write('Name:\n\nDate:\n\nClass:\n\n')
    quizFile.write(' ' * 20 + 'State Capticals Quiz (From %s)' % (quizNum + 1))
    quizFile.write('\n\n')

    # 打乱capital字典
    states = list(capitals.keys())
    random.shuffle(states)  # 打乱states的顺序

    # 循环states，制造50个问题
    for questionNum in range(50):
        correctAnswer = capitals[states[questionNum]]
        wrongAnswers = list(capitals.values())
        del wrongAnswers[wrongAnswers.index(correctAnswer)]  # 删除列表中的正确答案
        # random.sample()函数使得这种选择很容易，它的第一个参数是你希望选择的列表，第二个参数是你希望选择的值的个数。
        wrongAnswers = random.sample(wrongAnswers, 3)
        answerOption = wrongAnswers + [correctAnswer]
        random.shuffle(answerOption)
        # 在文件中写入问题
        quizFile.write('%s. What is the capital of %s?\n' % (questionNum + 1,
                        states[questionNum]))
        for i in range(4):
            quizFile.write(' %s. %s\n' % ('ABCD'[i], answerOption[i]))
        quizFile.write('\n')

        # 在答案卷中写入答案
        answerKeyFile.write('%s. %s\n' % (questionNum + 1, 'ABCD'[
            answerOption.index(correctAnswer)]))

    quizFile.close()
    answerKeyFile.close()
```
