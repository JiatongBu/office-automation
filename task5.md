# 前言

对于自动化办公而言，网络数据的批量获取完数据可以节约相当的时间，因此爬虫在自动化办公中占据了一个比较重要的位置。

因而本节针对爬虫项目进行一个介绍，力求最大程度还原实际的办公场景。

# Requests简介

Requests是一款目前非常流行的http请求库，使用python编写，能非常方便的对网页Requests进行爬取，也是爬虫最常用的发起请求第三方库。

安装方法：

```
pip install requests
或者conda安装
conda install requests
re.status_code 响应的HTTP状态码
re.text 响应内容的字符串形式
rs.content 响应内容的二进制形式
rs.encoding 响应内容的编码
```

## 访问百度

试一试对百度首页进行数据请求：

项目难度：⭐

In [1]:

```
import requests
# 发出http请求
re=requests.get("https://www.baidu.com")
# 查看响应状态
print(re.status_code)
#输出：200
#200就是响应的状态码，表示请求成功
#我们可以通过res.status_code的值来判断请求是否成功。
200
```



## 下载txt文件

例：用爬虫下载孔乙己的文章，网址是https://apiv3.shanbay.com/codetime/articles/mnvdu

我们打开这个网址 可以看到是鲁迅的文章

我们尝试着用爬虫保存文章的内容

项目难度：⭐

In [2]:

```
import requests
# 发出http请求
re = requests.get('https://apiv3.shanbay.com/codetime/articles/mnvdu')
# 查看响应状态
print('网页的状态码为%s'%re.status_code)
with open('鲁迅文章.txt', 'w') as file:
  # 将数据的字符串形式写入文件中
  print('正在爬取小说')
  file.write(re.text)
网页的状态码为200
正在爬取小说
```



re.txt就是网页中的内容，将内容保存到txt文件中

## 下载图片

re.text用于文本内容的获取、下载 re.content用于图片、视频、音频等内容的获取、下载

项目难度：⭐⭐

In [3]:

```
import requests
# 发出http请求
#下载图片
res=requests.get('https://img-blog.csdnimg.cn/20210424184053989.PNG')
# 以二进制写入的方式打开一个名为 info.jpg 的文件
with open('datawhale.png','wb') as ff:
    # 将数据的二进制形式写入文件中
    ff.write(res.content)
```



re.encoding 爬取内容的编码形似，常见的编码方式有 ASCII、GBK、UTF-8 等。如果用和文件编码不同的方式去解码，我们就会得到一些乱码。





## 实践项目1：自如公寓数据抓取



```
#这里增加了很多user_agent
#能一定程度能保护爬虫
user_agent = [
    "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:38.0) Gecko/20100101 Firefox/38.0",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; InfoPath.3; rv:11.0) like Gecko",
    "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)",
    "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)",
    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
    "Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
    "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11",
    "Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)"]
```





```
def get_info():
    csvheader=['名称','面积','朝向','户型','位置','楼层','是否有电梯','建成时间',' 门锁','绿化']
    with open('wuhan_ziru.csv', 'a+', newline='') as csvfile:
        writer  = csv.writer(csvfile)
        writer.writerow(csvheader)
        for i in range(1,50):  #总共有50页
            print('正在爬取自如第%s页'%i)
            timelist=[1,2,3]
            print('有点累了，需要休息一下啦（￢㉨￢）')
            time.sleep(random.choice(timelist))   #休息1-3秒，防止给对方服务器过大的压力！！！
            url='https://wh.ziroom.com/z/p%s/'%i
            headers = {'User-Agent': random.choice(user_agent)}
            r = requests.get(url, headers=headers)
            r.encoding = r.apparent_encoding
            soup = BeautifulSoup(r.text, 'lxml')
            all_info = soup.find_all('div', class_='info-box')
            print('开始干活咯(๑>؂<๑）')
            for info in all_info:
                href = info.find('a')
                if href !=None:
                    href='https:'+href['href']
                    try:
                        print('正在爬取%s'%href)
                        house_info=get_house_info(href)
                        writer.writerow(house_info)
                    except:
                        print('出错啦，%s进不去啦( •̥́ ˍ •̀ू )'%href)
```



通过研究发现了你需要定位的信息 通过标签头 h1 li span 和class的值对标签进行定位

```
<h1 class="Z_name"><i class="status iconicon_sign"></i>自如友家·电建地产盛世江城·4居室-05卧</h1>
----
<div class="Z_home_info">
<div class="Z_home_b clearfix">
    <dl class="">
        <dd>8.4㎡</dd>
        <dt>使用面积</dt>
    </dl>
    <dl class="">
        <dd>朝南</dd>
        <dt>朝向</dt>
    </dl>
    <dl class="">
        <dd>4室1厅</dd>
        <dt>户型</dt>
    </dl>
</div>
</div>
----
<ul class="Z_home_o">
    <li>
        <span class="la">位置</span><span class="va">
        <span class="ad">小区距2号线长港路站步行约231米</span>
     </li>
        <span class="la">楼层</span><span class="va">6/43</span>
    </li>
    <li>
        <span class="la">电梯</span><span class="va">有</span>
    </li>
    <li>
        <span class="la">年代</span><span class="va">2016年建成</span>
    </li>
    <li>
        <span class="la">门锁</span><span class="va">智能门锁</span>
    </li>
    <li>
        <span class="la">绿化</span><span class="va">35%</span>
    </li>          
</ul>
```

通过对上面标签的研究你完成了所有的代码

In [10]:

```
import requests
from bs4 import BeautifulSoup
import random
import time
import csv

#这里增加了很多user_agent
#能一定程度能保护爬虫
user_agent = [
    "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:38.0) Gecko/20100101 Firefox/38.0",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; InfoPath.3; rv:11.0) like Gecko",
    "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)",
    "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)",
    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
    "Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
    "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11",
    "Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)"]

def get_info():
    csvheader=['名称','面积','朝向','户型','位置','楼层','是否有电梯','建成时间',' 门锁','绿化']
    with open('wuhan_ziru.csv', 'a+', newline='') as csvfile:
        writer  = csv.writer(csvfile)
        writer.writerow(csvheader)
        for i in range(1,50):  #总共有50页
            print('正在爬取自如第%s页'%i)
            timelist=[1,2,3]
            print('有点累了，需要休息一下啦（￢㉨￢）')
            time.sleep(random.choice(timelist))   #休息1-3秒，防止给对方服务器过大的压力！！！
            url='https://wh.ziroom.com/z/p%s/'%i
            headers = {'User-Agent': random.choice(user_agent)}
            r = requests.get(url, headers=headers)
            r.encoding = r.apparent_encoding
            soup = BeautifulSoup(r.text, 'lxml')
            all_info = soup.find_all('div', class_='info-box')
            print('开始干活咯(๑>؂<๑）')
            for info in all_info:
                href = info.find('a')
                if href !=None:
                    href='https:'+href['href']
                    try:
                        print('正在爬取%s'%href)
                        house_info=get_house_info(href)
                        writer.writerow(house_info)
                    except:
                        print('出错啦，%s进不去啦( •̥́ ˍ •̀ू )'%href)

def get_house_info(href):
    #得到房屋的信息
    time.sleep(1)
    headers = {'User-Agent': random.choice(user_agent)}
    response = requests.get(url=href, headers=headers)
    response=response.content.decode('utf-8', 'ignore')
    soup = BeautifulSoup(response, 'lxml')
    name = soup.find('h1', class_='Z_name').text
    sinfo=soup.find('div', class_='Z_home_b clearfix').find_all('dd')
    area=sinfo[0].text
    orien=sinfo[1].text
    area_type=sinfo[2].text
    dinfo=soup.find('ul',class_='Z_home_o').find_all('li')
    location=dinfo[0].find('span',class_='va').text
    loucen=dinfo[1].find('span',class_='va').text
    dianti=dinfo[2].find('span',class_='va').text
    niandai=dinfo[3].find('span',class_='va').text
    mensuo=dinfo[4].find('span',class_='va').text
    lvhua=dinfo[5].find('span',class_='va').text
    ['名称','面积','朝向','户型','位置','楼层','是否有电梯','建成时间',' 门锁','绿化']
    room_info=[name,area,orien,area_type,location,loucen,dianti,niandai,mensuo,lvhua]
    return room_info

if __name__ == '__main__':
    get_info()
正在爬取自如第1页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/807988393.html
正在爬取https://wh.ziroom.com/x/807004676.html
正在爬取https://wh.ziroom.com/x/807953610.html
正在爬取https://wh.ziroom.com/x/808037785.html
正在爬取https://wh.ziroom.com/x/807919198.html
正在爬取https://wh.ziroom.com/x/793749918.html
正在爬取https://wh.ziroom.com/x/767210815.html
正在爬取https://wh.ziroom.com/x/789408295.html
正在爬取https://wh.ziroom.com/x/808031730.html
正在爬取https://wh.ziroom.com/x/768041620.html
正在爬取https://wh.ziroom.com/x/795938432.html
正在爬取https://wh.ziroom.com/x/807215719.html
正在爬取https://wh.ziroom.com/x/807853636.html
正在爬取https://wh.ziroom.com/x/779302932.html
正在爬取https://wh.ziroom.com/x/808088052.html
正在爬取https://wh.ziroom.com/x/765812075.html
正在爬取https://wh.ziroom.com/x/807251426.html
正在爬取https://wh.ziroom.com/x/785748194.html
正在爬取https://wh.ziroom.com/x/745251276.html
正在爬取https://wh.ziroom.com/x/790382757.html
正在爬取https://wh.ziroom.com/x/792641596.html
正在爬取https://wh.ziroom.com/x/807917147.html
正在爬取https://wh.ziroom.com/x/793643315.html
正在爬取https://wh.ziroom.com/x/775808119.html
正在爬取https://wh.ziroom.com/x/790675697.html
正在爬取https://wh.ziroom.com/x/807995540.html
正在爬取https://wh.ziroom.com/x/807734055.html
正在爬取https://wh.ziroom.com/x/750362109.html
正在爬取https://wh.ziroom.com/x/786190029.html
正在爬取自如第2页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/793785032.html
正在爬取https://wh.ziroom.com/x/783071479.html
正在爬取https://wh.ziroom.com/x/785079088.html
正在爬取https://wh.ziroom.com/x/807926604.html
正在爬取https://wh.ziroom.com/x/807116914.html
正在爬取https://wh.ziroom.com/x/807309673.html
正在爬取https://wh.ziroom.com/x/808060521.html
正在爬取https://wh.ziroom.com/x/808091671.html
正在爬取https://wh.ziroom.com/x/739173256.html
正在爬取https://wh.ziroom.com/x/759004324.html
正在爬取https://wh.ziroom.com/x/808010576.html
正在爬取https://wh.ziroom.com/x/793694919.html
正在爬取https://wh.ziroom.com/x/745849572.html
正在爬取https://wh.ziroom.com/x/745569921.html
正在爬取https://wh.ziroom.com/x/756115664.html
正在爬取https://wh.ziroom.com/x/778614911.html
正在爬取https://wh.ziroom.com/x/792494059.html
正在爬取https://wh.ziroom.com/x/745041465.html
正在爬取https://wh.ziroom.com/x/753656520.html
正在爬取https://wh.ziroom.com/x/769194950.html
正在爬取https://wh.ziroom.com/x/790162276.html
正在爬取https://wh.ziroom.com/x/807112539.html
正在爬取https://wh.ziroom.com/x/808098671.html
正在爬取https://wh.ziroom.com/x/740199516.html
正在爬取https://wh.ziroom.com/x/785605895.html
正在爬取https://wh.ziroom.com/x/807862050.html
正在爬取https://wh.ziroom.com/x/750320787.html
正在爬取https://wh.ziroom.com/x/744464897.html
正在爬取https://wh.ziroom.com/x/736876684.html
正在爬取自如第3页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/756651104.html
正在爬取https://wh.ziroom.com/x/807132433.html
正在爬取https://wh.ziroom.com/x/743092153.html
正在爬取https://wh.ziroom.com/x/778289088.html
正在爬取https://wh.ziroom.com/x/772025022.html
正在爬取https://wh.ziroom.com/x/782053367.html
正在爬取https://wh.ziroom.com/x/752866164.html
正在爬取https://wh.ziroom.com/x/788092684.html
正在爬取https://wh.ziroom.com/x/750746617.html
正在爬取https://wh.ziroom.com/x/782996110.html
正在爬取https://wh.ziroom.com/x/788945217.html
正在爬取https://wh.ziroom.com/x/781732782.html
正在爬取https://wh.ziroom.com/x/807039389.html
正在爬取https://wh.ziroom.com/x/807783923.html
正在爬取https://wh.ziroom.com/x/734991780.html
正在爬取https://wh.ziroom.com/x/795081049.html
正在爬取https://wh.ziroom.com/x/807306019.html
正在爬取https://wh.ziroom.com/x/807176302.html
正在爬取https://wh.ziroom.com/x/750813644.html
正在爬取https://wh.ziroom.com/x/807005271.html
正在爬取https://wh.ziroom.com/x/771925403.html
正在爬取https://wh.ziroom.com/x/768246775.html
正在爬取https://wh.ziroom.com/x/807096579.html
正在爬取https://wh.ziroom.com/x/752367875.html
正在爬取https://wh.ziroom.com/x/808095486.html
正在爬取https://wh.ziroom.com/x/808010555.html
正在爬取https://wh.ziroom.com/x/808098748.html
正在爬取https://wh.ziroom.com/x/767907663.html
正在爬取https://wh.ziroom.com/x/781621620.html
正在爬取自如第4页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/760991272.html
正在爬取https://wh.ziroom.com/x/808012102.html
正在爬取https://wh.ziroom.com/x/770291438.html
正在爬取https://wh.ziroom.com/x/780031402.html
正在爬取https://wh.ziroom.com/x/807333795.html
正在爬取https://wh.ziroom.com/x/782734210.html
正在爬取https://wh.ziroom.com/x/792780209.html
正在爬取https://wh.ziroom.com/x/807862008.html
正在爬取https://wh.ziroom.com/x/783410203.html
正在爬取https://wh.ziroom.com/x/807221921.html
正在爬取https://wh.ziroom.com/x/807882756.html
正在爬取https://wh.ziroom.com/x/754817707.html
正在爬取https://wh.ziroom.com/x/757588803.html
正在爬取https://wh.ziroom.com/x/779658534.html
正在爬取https://wh.ziroom.com/x/777815922.html
正在爬取https://wh.ziroom.com/x/807776454.html
正在爬取https://wh.ziroom.com/x/807121982.html
正在爬取https://wh.ziroom.com/x/746489481.html
正在爬取https://wh.ziroom.com/x/794677820.html
正在爬取https://wh.ziroom.com/x/773202893.html
正在爬取https://wh.ziroom.com/x/774563124.html
正在爬取https://wh.ziroom.com/x/779071781.html
正在爬取https://wh.ziroom.com/x/808090138.html
正在爬取https://wh.ziroom.com/x/807121954.html
正在爬取https://wh.ziroom.com/x/782082855.html
正在爬取https://wh.ziroom.com/x/807753459.html
正在爬取https://wh.ziroom.com/x/744260324.html
正在爬取https://wh.ziroom.com/x/784651027.html
正在爬取https://wh.ziroom.com/x/746916572.html
正在爬取https://wh.ziroom.com/x/782836545.html
正在爬取自如第5页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/794426978.html
正在爬取https://wh.ziroom.com/x/757115831.html
正在爬取https://wh.ziroom.com/x/791448011.html
正在爬取https://wh.ziroom.com/x/770636176.html
正在爬取https://wh.ziroom.com/x/807753620.html
正在爬取https://wh.ziroom.com/x/751669475.html
正在爬取https://wh.ziroom.com/x/792641305.html
正在爬取https://wh.ziroom.com/x/792878470.html
正在爬取https://wh.ziroom.com/x/758453849.html
正在爬取https://wh.ziroom.com/x/769739702.html
正在爬取https://wh.ziroom.com/x/807072681.html
正在爬取https://wh.ziroom.com/x/773203184.html
正在爬取https://wh.ziroom.com/x/808095514.html
正在爬取https://wh.ziroom.com/x/795576913.html
正在爬取https://wh.ziroom.com/x/790547075.html
正在爬取https://wh.ziroom.com/x/761116499.html
正在爬取https://wh.ziroom.com/x/807056021.html
正在爬取https://wh.ziroom.com/x/796786018.html
正在爬取https://wh.ziroom.com/x/807072653.html
正在爬取https://wh.ziroom.com/x/747617203.html
正在爬取https://wh.ziroom.com/x/796462717.html
正在爬取https://wh.ziroom.com/x/807713755.html
正在爬取https://wh.ziroom.com/x/807061320.html
正在爬取https://wh.ziroom.com/x/807290367.html
正在爬取https://wh.ziroom.com/x/807261198.html
正在爬取https://wh.ziroom.com/x/780494189.html
正在爬取https://wh.ziroom.com/x/765194088.html
正在爬取https://wh.ziroom.com/x/784247119.html
正在爬取https://wh.ziroom.com/x/772679772.html
正在爬取https://wh.ziroom.com/x/747160721.html
正在爬取自如第6页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/750579971.html
正在爬取https://wh.ziroom.com/x/793405665.html
正在爬取https://wh.ziroom.com/x/807990913.html
正在爬取https://wh.ziroom.com/x/772206606.html
正在爬取https://wh.ziroom.com/x/783605561.html
正在爬取https://wh.ziroom.com/x/808097593.html
正在爬取https://wh.ziroom.com/x/738323439.html
正在爬取https://wh.ziroom.com/x/755406885.html
正在爬取https://wh.ziroom.com/x/753869823.html
正在爬取https://wh.ziroom.com/x/785606865.html
正在爬取https://wh.ziroom.com/x/775601994.html
正在爬取https://wh.ziroom.com/x/796173560.html
正在爬取https://wh.ziroom.com/x/762855515.html
正在爬取https://wh.ziroom.com/x/807758352.html
正在爬取https://wh.ziroom.com/x/782576973.html
正在爬取https://wh.ziroom.com/x/789471830.html
正在爬取https://wh.ziroom.com/x/807860104.html
正在爬取https://wh.ziroom.com/x/768775037.html
正在爬取https://wh.ziroom.com/x/763378539.html
正在爬取https://wh.ziroom.com/x/807721364.html
正在爬取https://wh.ziroom.com/x/777336548.html
正在爬取https://wh.ziroom.com/x/807267792.html
正在爬取https://wh.ziroom.com/x/807993216.html
正在爬取https://wh.ziroom.com/x/759747150.html
正在爬取https://wh.ziroom.com/x/796793681.html
正在爬取https://wh.ziroom.com/x/787352962.html
正在爬取https://wh.ziroom.com/x/779775807.html
正在爬取https://wh.ziroom.com/x/807106582.html
正在爬取https://wh.ziroom.com/x/746827720.html
正在爬取https://wh.ziroom.com/x/752818731.html
正在爬取自如第7页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/778491624.html
正在爬取https://wh.ziroom.com/x/795104329.html
正在爬取https://wh.ziroom.com/x/746748859.html
正在爬取https://wh.ziroom.com/x/785348942.html
正在爬取https://wh.ziroom.com/x/791679744.html
正在爬取https://wh.ziroom.com/x/790041899.html
正在爬取https://wh.ziroom.com/x/808044547.html
正在爬取https://wh.ziroom.com/x/807921039.html
正在爬取https://wh.ziroom.com/x/774859265.html
正在爬取https://wh.ziroom.com/x/766012477.html
正在爬取https://wh.ziroom.com/x/747378389.html
正在爬取https://wh.ziroom.com/x/807063511.html
正在爬取https://wh.ziroom.com/x/780549479.html
正在爬取https://wh.ziroom.com/x/772253069.html
正在爬取https://wh.ziroom.com/x/795250314.html
正在爬取https://wh.ziroom.com/x/786424963.html
正在爬取https://wh.ziroom.com/x/777946969.html
正在爬取https://wh.ziroom.com/x/807255402.html
正在爬取https://wh.ziroom.com/x/792621614.html
正在爬取https://wh.ziroom.com/x/747955927.html
正在爬取https://wh.ziroom.com/x/778008661.html
正在爬取https://wh.ziroom.com/x/781692527.html
正在爬取https://wh.ziroom.com/x/807398944.html
正在爬取https://wh.ziroom.com/x/790945260.html
正在爬取https://wh.ziroom.com/x/763197343.html
正在爬取https://wh.ziroom.com/x/807740943.html
正在爬取https://wh.ziroom.com/x/807196833.html
正在爬取https://wh.ziroom.com/x/745922904.html
正在爬取https://wh.ziroom.com/x/807129731.html
正在爬取https://wh.ziroom.com/x/808015224.html
正在爬取自如第8页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/796013025.html
正在爬取https://wh.ziroom.com/x/807126189.html
正在爬取https://wh.ziroom.com/x/807276780.html
正在爬取https://wh.ziroom.com/x/788907484.html
正在爬取https://wh.ziroom.com/x/757443109.html
正在爬取https://wh.ziroom.com/x/789176271.html
正在爬取https://wh.ziroom.com/x/742486097.html
正在爬取https://wh.ziroom.com/x/779930425.html
正在爬取https://wh.ziroom.com/x/737811861.html
正在爬取https://wh.ziroom.com/x/808088038.html
正在爬取https://wh.ziroom.com/x/771210804.html
正在爬取https://wh.ziroom.com/x/787770838.html
正在爬取https://wh.ziroom.com/x/807551068.html
正在爬取https://wh.ziroom.com/x/807553686.html
正在爬取https://wh.ziroom.com/x/748004427.html
正在爬取https://wh.ziroom.com/x/795972867.html
正在爬取https://wh.ziroom.com/x/807105245.html
正在爬取https://wh.ziroom.com/x/807997584.html
正在爬取https://wh.ziroom.com/x/779054709.html
正在爬取https://wh.ziroom.com/x/807574504.html
正在爬取https://wh.ziroom.com/x/776968821.html
正在爬取https://wh.ziroom.com/x/787931955.html
正在爬取https://wh.ziroom.com/x/783368784.html
正在爬取https://wh.ziroom.com/x/767103339.html
正在爬取https://wh.ziroom.com/x/807553847.html
正在爬取https://wh.ziroom.com/x/807713468.html
正在爬取https://wh.ziroom.com/x/768174122.html
正在爬取https://wh.ziroom.com/x/770298810.html
正在爬取https://wh.ziroom.com/x/795000539.html
正在爬取https://wh.ziroom.com/x/765217465.html
正在爬取自如第9页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/788563037.html
正在爬取https://wh.ziroom.com/x/807054978.html
正在爬取https://wh.ziroom.com/x/807727846.html
正在爬取https://wh.ziroom.com/x/807039711.html
正在爬取https://wh.ziroom.com/x/781599504.html
正在爬取https://wh.ziroom.com/x/780725146.html
正在爬取https://wh.ziroom.com/x/745319952.html
正在爬取https://wh.ziroom.com/x/808062432.html
正在爬取https://wh.ziroom.com/x/758995012.html
正在爬取https://wh.ziroom.com/x/807336217.html
正在爬取https://wh.ziroom.com/x/778396273.html
正在爬取https://wh.ziroom.com/x/764543218.html
正在爬取https://wh.ziroom.com/x/779711108.html
正在爬取https://wh.ziroom.com/x/766087943.html
正在爬取https://wh.ziroom.com/x/807882749.html
正在爬取https://wh.ziroom.com/x/785117306.html
正在爬取https://wh.ziroom.com/x/807978005.html
正在爬取https://wh.ziroom.com/x/752555473.html
正在爬取https://wh.ziroom.com/x/808053045.html
正在爬取https://wh.ziroom.com/x/775146385.html
正在爬取https://wh.ziroom.com/x/808042636.html
正在爬取https://wh.ziroom.com/x/781312287.html
正在爬取https://wh.ziroom.com/x/763785260.html
正在爬取https://wh.ziroom.com/x/747321547.html
正在爬取https://wh.ziroom.com/x/757472694.html
正在爬取https://wh.ziroom.com/x/737462176.html
正在爬取https://wh.ziroom.com/x/782051815.html
正在爬取https://wh.ziroom.com/x/791764037.html
正在爬取https://wh.ziroom.com/x/747688595.html
正在爬取https://wh.ziroom.com/x/773842899.html
出错啦，https://wh.ziroom.com/x/773842899.html进不去啦( •̥́ ˍ •̀ू )
正在爬取自如第10页
有点累了，需要休息一下啦（￢㉨￢）
开始干活咯(๑>؂<๑）
正在爬取https://wh.ziroom.com/x/807307958.html
正在爬取https://wh.ziroom.com/x/807926534.html
正在爬取https://wh.ziroom.com/x/785882054.html
出错啦，https://wh.ziroom.com/x/785882054.html进不去啦( •̥́ ˍ •̀ू )
正在爬取https://wh.ziroom.com/x/783948456.html
正在爬取https://wh.ziroom.com/x/776655705.html
```



## 实践项目2：36kr信息抓取与邮件发送

本节内容为作者原创的项目，课程难度为5星，建议读者跟着课程一步一步的来，如果有不明白的地方，可以在群里面与其他伙伴进行交流。

在输出本节内容时，请注明来源，Datawhale自动化办公课程，谢谢~

如果没有多个邮箱，可以百度搜索临时邮箱进行实践学习

项目难度：⭐⭐⭐⭐⭐

完成了上面的实践项目1后，你膨胀到不行，觉得自己太厉害了。通过前面的学习，你了解到使用python进行电子邮件的收发，突然有一天你想到，如果我用A账户进行发送，同时用B账户进行接受，在手机上安装一个邮件接受的软件，这样就能完成信息从pc端投送到移动端。

在这样的思想上，就可以对动态变化的信息进行监控，一旦信息触发了发送的条件，可以将信息通过邮件投送到手机上，从而让自己最快感知到。

具体路径是：

python爬虫-->通过邮件A发送-->服务器--->通过邮件B接收

因此我们本节的内容就是爬取36kr的信息然后通过邮件发送

36kr官网：https://36kr.com/newsflashes

通过python发送邮件需要获得pop3的授权码

具体获取方式可参考：

https://blog.csdn.net/wateryouyo/article/details/51766345

接下来就爬取36Kr的网站

通过观察我们发现 消息的标签为

```
<a class="item-title" rel="noopener noreferrer" target="_blank" href="/newsflashes/1218249313424001" sensors_operation_list="page_flow">中国平安：推动新方正集团聚集医疗健康等核心业务发展</a>
```

因此我们爬取的代码为

需要注意的是，邮箱发送消息用的HTML的模式，而HTML模式下换行符号为 < br>

In [1]:

```
def main(): 
    print('正在爬取数据')
    url = 'https://36kr.com/newsflashes'
    headers = {'User-Agent': random.choice(user_agent)}
    response = requests.get(url, headers=headers)
    response=response.content.decode('utf-8', 'ignore')
    soup = BeautifulSoup(response, 'lxml')
    news = soup.find_all('a', class_='item-title')  
    news_list=[]
    for i in news:
        title=i.get_text()
        href='https://36kr.com'+i['href']
        news_list.append(title+'<br>'+href)
    info='<br></br>'.join(news_list)
```



接下来就是配置邮箱的发送信息

In [ ]:

```
smtpserver = 'smtp.qq.com'

# 发送邮箱用户名密码
user = ''
password = ''

# 发送和接收邮箱
sender = ''
receive = ''

def send_email(content):
    # 通过QQ邮箱发送
    title='36kr快讯'
    subject = title
    msg = MIMEText(content, 'html', 'utf-8')
    msg['Subject'] = Header(subject, 'utf-8')
    msg['From'] = sender
    msg['To'] = receive
    # SSL协议端口号要使用465
    smtp = smtplib.SMTP_SSL(smtpserver, 465)  # 这里是服务器端口！
    # HELO 向服务器标识用户身份
    smtp.helo(smtpserver)
    # 服务器返回结果确认
    smtp.ehlo(smtpserver)
    # 登录邮箱服务器用户名和密码
    smtp.login(user, password)
    smtp.sendmail(sender, receive, msg.as_string())
    smtp.quit()
```

In [ ]:

```
import requests
import random
from bs4 import BeautifulSoup
import smtplib  # 发送邮件模块
from email.mime.text import MIMEText  # 定义邮件内容
from email.header import Header  # 定义邮件标题

smtpserver = 'smtp.qq.com'

# 发送邮箱用户名密码
user = ''
password = ''

# 发送和接收邮箱
sender = ''
receive = ''

user_agent = [
    "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
    "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:38.0) Gecko/20100101 Firefox/38.0",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729; InfoPath.3; rv:11.0) like Gecko",
    "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)",
    "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)",
    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
    "Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
    "Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11",
    "Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)",
    "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)"]

def main():
    print('正在爬取数据')
    url = 'https://36kr.com/newsflashes'
    headers = {'User-Agent': random.choice(user_agent)}
    response = requests.get(url, headers=headers)
    response=response.content.decode('utf-8', 'ignore')
    soup = BeautifulSoup(response, 'lxml')
    news = soup.find_all('a', class_='item-title')  
    news_list=[]
    for i in news:
        title=i.get_text()
        href='https://36kr.com'+i['href']
        news_list.append(title+'<br>'+href)
    info='<br></br>'.join(news_list)
    print('正在发送信息')
    send_email(info)

def send_email(content):
    # 通过QQ邮箱发送
    title='36kr快讯'
    subject = title
    msg = MIMEText(content, 'html', 'utf-8')
    msg['Subject'] = Header(subject, 'utf-8')
    msg['From'] = sender
    msg['To'] = receive
    # SSL协议端口号要使用465
    smtp = smtplib.SMTP_SSL(smtpserver, 465)  # 这里是服务器端口！
    # HELO 向服务器标识用户身份
    smtp.helo(smtpserver)
    # 服务器返回结果确认
    smtp.ehlo(smtpserver)
    # 登录邮箱服务器用户名和密码
    smtp.login(user, password)
    smtp.sendmail(sender, receive, msg.as_string())
    smtp.quit()

if __name__ == '__main__':
    main()
```
