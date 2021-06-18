**安装openpyxl模块**

>jupyter已经安装了opengxyl模块

**Excel读取**

    from openpyxl import load_workbook
    exl = load_workbook(filename = '实验名单.xlsx')
    print(exl.sheetnames)

**读取单元格**

    cell = sheet.cell(row=1,column=2) #指定行列数
    print(cell.value)
    cell_1 = sheet['A2'] #指定坐标
    print(cell_1.value)

**读取多个格子的值**

```cells = sheet['A1:C8'] #指定坐标范围
Row = sheet[1] #指定行的值
Rows = sheet[1:2] #第1到2行的值

Column = sheet['A'] #指定列的值
Columns = sheet['A:C'] #第A到C列
```

**指定范围的值**
```for row in sheet.iter_rows(min_row = 1, max_row = 5,
                            min_col = 2, max_col = 6):
    print(row)
    # 一列由多个单元格组成，若需要获取每个单元格的值则循环获取即可
    for cell in row:
        print(cell.value)

# 列获取
for col in sheet.iter_cols(min_row = 1, max_row = 5,
                             min_col = 2, max_col = 6):
    print(col)

    for cell in col:
        print(cell.value)
```

**Excel读取**

```
from openpyxl import load_workbook

exl = load_workbook(filename = '实验名单.xlsx')
sheet = exl.active
sheet['A1'] = 'xxxx'       
#或者cell = sheet['A1'] 
#cell.value = 'hello word'
exl.save(filename = 'test.xlsx') 
```

**Excel 样式**

设置字体样式

    Font(name=“字体名”,size=字体大小,bold=是否加粗,italic=是否斜体,color=字体颜色)
        Font 注意首字母大写
```
from openpyxl import Workbook
from openpyxl.styles import Font
# 创建新表   
workbook = Workbook()
sheet = workbook.active
cell = sheet['A1']
font = Font(name='草书', size=10, bold=True, italic=True, color='FF0000')
cell.font = font
workbook.save('excel样式设置试运行.xlsx')
```
