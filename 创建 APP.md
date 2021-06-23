Python办公自动化 Task4
使用语言：python
使用到的两个库，分别是：PyPDF2 和 pdfplumbe

## 一、批量拆分

将一个完整的 PDF 拆分成几个小的 PDF，因为主要涉及到 PDF 整体的操作，拆分的大概思路如下：

1. 读取 PDF 的整体信息、总页数等
2. 遍历每一页内容，以每个 step 为间隔将 PDF 存成每一个小的文件块
3. 将小的文件块重新保存为新的 PDF 文件

```python
import os
from PyPDF2 import PdfFileReader
from PyPDF2 import PdfFileWriter

def split_pdf(filename, filepath, save_dirpath, step=10):
    if not os.path.exists(save_dirpath):
        os.mkdir(save_dirpath)
    pdf_reader = PdfFileReader(filepath)
    # 读取每一页的数据
    pages = pdf_reader.getNumPages()
    for page in range(0, pages, step):
        pdf_writer = PdfFileWriter()
        # 拆分pdf，每 step 页的拆分为一个文件
        for index in range(page, page+step):
            if index < pages:
                pdf_writer.addPage(pdf_reader.getPage(index))
        # 保存拆分后的小文件
        save_path = os.path.join(save_dirpath, filename+str(int(page/step)+1)+'.pdf')
        print(save_path)
        with open(save_path, "wb") as out:
            pdf_writer.write(out)
    print("文件已成功拆分，保存路径为："+save_dirpath)

split_pdf('MySQL','D:/Study/SXH/MySQL/MySQL.pdf','D:/Study/Datawhale/OfficeAutomation/拆分/')

>>>
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL1.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL2.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL3.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL4.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL5.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL6.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL7.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL8.pdf
D:/Study/Datawhale/OfficeAutomation/拆分/MySQL9.pdf
文件已成功拆分，保存路径为：D:/Study/Datawhale/OfficeAutomation/拆分/
```

## 二、批量合并

合并的大概思路如下：
1. 确定要合并的 文件顺序
2. 循环追加到一个文件块中
3. 保存成一个新的文件

```python
import os
from PyPDF2 import PdfFileReader
from PyPDF2 import PdfFileWriter

def concat_pdf(filename, read_dirpath, save_filepath):
    pdf_writer = PdfFileWriter()
    # 对文件名进行排序
    list_filename = os.listdir(read_dirpath)
    list_filename.sort(key=lambda x: int(x[:-4].replace(filename, "")))
    for filename in list_filename:
        print(filename)
        filepath = os.path.join(read_dirpath, filename)
        # 读取文件并获取文件的页数
        pdf_reader = PdfFileReader(filepath)
        pages = pdf_reader.getNumPages()
        # 逐页添加
        for page in range(pages):
            pdf_writer.addPage(pdf_reader.getPage(page))
    # 保存合并后的文件
    with open(save_filepath, "wb") as out:
        pdf_writer.write(out)
    print("文件已成功合并，保存路径为："+save_filepath)

concat_pdf('MySQL','D:/Study/Datawhale/OfficeAutomation/拆分/','D:/Study/Datawhale/OfficeAutomation/MySQL.pdf')

>>>
MySQL1.pdf
MySQL2.pdf
MySQL3.pdf
MySQL4.pdf
MySQL5.pdf
MySQL6.pdf
MySQL7.pdf
MySQL8.pdf
MySQL9.pdf
文件已成功合并，保存路径为：D:/Study/Datawhale/OfficeAutomation/MySQL.pdf
```

## 三、提取文字内容

涉及到具体的 PDF 内容 操作，本小节需要用到 `pdfplumber` 这个库
在进行文字提取的时候，主要用到 `extract_text` 这个函数

但是内容会自动换行，写入word格式不太理想，但是如果用split，全文的换行符都会去掉，还没想到好的办法解决。

```python
import pdfplumber
from docx import Document
def extract_text_info(filepath):
    with pdfplumber.open(filepath) as pdf:
        # 获取第2页数据
        page = pdf.pages[1]
        return page.extract_text()

text = extract_text_info(r'D:\Study\Datawhale\OfficeAutomation\易方达中小盘混合型证券投资基金2020年中期报告.pdf')
text = text.split('\n')
```

### 3.1 写入Word文件

```python
from docx import Document
from docx.shared import RGBColor, Pt,Inches,Cm
from docx.enum.text import WD_PARAGRAPH_ALIGNMENT
from docx.oxml.ns import qn
doc = Document()
doc.styles['Normal'].font.name = u'宋体'
doc.styles['Normal']._element.rPr.rFonts.set(qn('w:eastAsia'), u'宋体')
# text[0]设置为页眉，text[-1]设置为页脚
header = doc.sections[0].header
para_header = header.paragraphs[0]
para_header.add_run(text[0])
para_header.alignment = WD_PARAGRAPH_ALIGNMENT.RIGHT
footer = doc.sections[0].footer
para_foot = footer.paragraphs[0]
para_foot.add_run(text[-1])
para_foot.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER
# text[1] text[2]设置为标题
h1 = doc.add_heading(text[1], level=1)
h1.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER
h2 = doc.add_paragraph()
h2.paragraph_format.first_line_indent = Cm(0.75)
h2.paragraph_format.space_after =  Cm(0.75)
rh2 = h2.add_run(text[2])
rh2.font.bold =True
# 写入正文
def main(text):
    para = doc.add_paragraph()
    para.paragraph_format.first_line_indent = Cm(0.75)
    para.paragraph_format.line_spacing =  1.5
    para.paragraph_format.space_after =  Inches(0)
    r = para.add_run(text)
    return r
# 内容
text_1 = ''.join([text[3],text[4],text[5]])
text_2 = ''.join([text[6],text[7],text[8]])
text_3 = ''.join([text[10],text[11]])
main(text_1)
main(text_2)
main(text[9])
main(text_3)
main(text[12])
main(text[13])
doc.save('易方达.docx')
```



## 四、提取表格内容

涉及到具体的 PDF 内容 操作，本小节需要用到 `pdfplumber` 这个库
在进行表格提取的时候，主要用到 `extract_table` 这个函数

```python
import pdfplumber
import pandas as pd

def extract_table_info(filepath):
    with pdfplumber.open(filepath) as pdf:
        # 获取第18页数据
        page = pdf.pages[17]
        # 如果一页有一个表格，设置表格的第一行为表头，其余为数据
        table_info = page.extract_table()
        df_table = pd.DataFrame(table_info[1:], columns=table_info[0])
        df_table.to_csv('demo.csv', index=False, encoding='utf-8-sig')
        
extract_table_info(r'D:\Study\Datawhale\OfficeAutomation\易方达中小盘混合型证券投资基金2020年中期报告.pdf')
```



如果一个页面中有多个表格，因为读取的表格会被存成二维数组，而多个二维数组就组成一个三维数组
遍历这个三位数组，就可以得到该页的每一个表格数据，对应的将 `extract_table` 函数 改成 `extract_tables` 即可

```python
import pdfplumber
import pandas as pd

def extract_table_info(filepath):
    with pdfplumber.open(filepath) as pdf:
        # 获取第18页数据
        page = pdf.pages[19]
        # 如果一页有多个表格，对应的数据是一个三维数组
        tables_info = page.extract_tables()
        for index in range(len(tables_info)):
            # 设置表格的第一行为表头，其余为数据
            df_table = pd.DataFrame(tables_info[index][1:], columns=tables_info[index][0])
            df_table.to_csv('dmeo_1.csv', mode='a', index=False, encoding='utf-8-sig')
        
extract_table_info(r'D:\Study\Datawhale\OfficeAutomation\易方达中小盘混合型证券投资基金2020年中期报告.pdf')
```
