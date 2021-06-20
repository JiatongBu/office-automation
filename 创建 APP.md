Python办公自动化 Task3
使用语言：python


题目理解：

本次学习主要针对办公过程中的python进行word、excel、pdf的相关操作，了解通过python进行文件批量处理、电子邮件收发、数据爬取。
Task03—WORD 主要是以下几个点：

    认识Python–word的基本结构



```
# 新建空白文档

doc_1 = Document()

# 添加标题（0相当于文章的题目，默认级别是1，级别范围为0-9）

doc_1.add_heading('新建空白文档标题，级别为0',level = 0)
doc_1.add_heading('新建空白文档标题，级别为1',level = 1)
doc_1.add_heading('新建空白文档标题，级别为2',level = 2)

# 新增段落

paragraph_1 = doc_1.add_paragraph('这是第一段文字的开始\n请多多关照！')

# 加粗

paragraph_1.add_run('加粗字体').bold = True
paragraph_1.add_run('普通字体')

# 斜体

paragraph_1.add_run('斜体字体').italic =True

# 新段落（当前段落的下方）

paragraph_2 = doc_1.add_paragraph('新起的第二段文字。')

# 新段落（指定端的上方）

prior_paragraph = paragraph_1.insert_paragraph_before('在第一段文字前插入的段落')

# 添加分页符(可以进行灵活的排版）

doc_1.add_page_break()

# 新段落（指定端的上方）

paragraph_3 = doc_1.add_paragraph('这是第二页第一段文字！')

# 保存文件（当前目录下）

doc_1.save('doc_1.docx')
```

> 进行自定义函数，方便后续修改：字体、边距、页眉页脚…… 

```
#定义字体设置函数
def font_setting(doc,text,font_cn):
       style_add = doc.styles.add_style(font_cn, WD_STYLE_TYPE.CHARACTER)
       style_add.font.name = font_cn
       doc.styles[font_cn]._element.rPr.rFonts.set(qn('w:eastAsia'), font_cn)
       par = doc.add_paragraph()
       text = par.add_run(text, style=font_cn)

doc = Document()
a = 'aaaaaa'
b = 'ssssss'
c = 'dddddd'

font_setting(doc,a,'宋体')
font_setting(doc,b,'华文中宋')
font_setting(doc,c,'黑体')
```

项目实战：以某次实验通知为例
调用Excel表格中的具体信息；设定模板格式；进行for循环迭代

```
n = 0
for row in sheet.rows:
    if n:
        company = row[0].value
        office = row[1].value
        name = row[2].value
        date = str(row[3].value).split()[0]
        print(company, office, name, date)
        
        doc = Document()
        heading_1 = '实验报告'
        paragraph_1 = doc.add_heading(heading_1, level=1)
        #居中对齐
        paragraph_1.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER
        #单独修改较大字号
        for run in paragraph_1.runs:
            run.font.size = Pt(17)
        greeting_word_1 = '各位'
        greeting_word_2 = '同学'
        greeting_word_3 = ',注意： '
        paragraph_2 = doc.add_paragraph()
        
        paragraph_2.add_run(greeting_word_2)
        r_1 = paragraph_2.add_run(company)
        r_1.font.bold = True #加粗
        r_1.font.underline = True #下划线
        
        paragraph_2.add_run(greeting_word_2)
        r_2 = paragraph_2.add_run(office)
        r_2.font.bold = True  # 加粗
        r_2.font.underline = True    #下划线
    
        r_3 = paragraph_2.add_run(name)
        r_3.font.bold = True  # 加粗
        r_3.font.underline = True    #下划线
        paragraph_2.add_run(greeting_word_3)
        paragraph_3 = doc.add_paragraph()
        paragraph_3.add_run('6月20日实验安排')
        paragraph_3.paragraph_format.first_line_indent = Cm(0.75)
        paragraph_3.paragraph_format.alignment = WD_PARAGRAPH_ALIGNMENT.LEFT
        paragraph_3.paragraph_format.space_after = Inches(1.0)
        paragraph_3.paragraph_format.line_spacing = 1.5
    
        paragraph_4 = doc.add_paragraph()
        date_word_1 = '时间：'
        paragraph_4.add_run(date_word_1)
        paragraph_4.alignment = WD_PARAGRAPH_ALIGNMENT.RIGHT
        sign_date = "{}年{}月{}日".format(date.split('-')[0], date.split('-')[1], date.split('-')[2])
        paragraph_4.add_run(sign_date).underline = True
        paragraph_4.alignment = WD_PARAGRAPH_ALIGNMENT.RIGHT
        
        #设置全文字体
        for paragraph in doc.paragraphs:
            for run in paragraph.runs:
                run.font.color.rgb = RGBColor(0, 0, 0)
                run.font.name = '楷体'
                r = run._element.rPr.rFonts
                r.set(qn('w:eastAsia'),'楷体')
        doc.save(path + "\{}-aaa.docx".format(name))
    n = n + 1

```