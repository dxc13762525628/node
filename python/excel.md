## Excel

中文文档地址:https://openpyxl.readthedocs.io/en/stable/

较好的博客地址:https://juejin.cn/post/6844903816412954638

## 模块准备

常见的操作excel的模块有xlrd(读),xlwt(写),xlutils(复制，筛选等对文档操作)

我们使用openxl可读可写可操作非常强大，但是非常吃内存

### 安装模块

```python
# 先安装openpyxl
pip install openpyxl
# 再安装图片库 用于后续写图片进去
pip install pillow
# 再安装一个可以导出excel图片的库
pip install openpyxl_image_loader
```

## 基本操作

```python
# 打开一个excel
wb = load_workbook('sample.xlsx')
# 打开一个sheet表
shweet = wb["sheet1"]
# 获取所有的有效行数
rows = sheet.max_row
# 获取所有的列数
cols = sheet.max_column
# 读数据
value = sheet.cell(row=1, column=1).value
# 将测试内容写在 (1,1)这个位置
sheet.cell(row=1, column=1, value="测试")
# 保存
wb.save('test.xlsx')
```

## demo

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：exec_demo 
@File    ：get_images.py
@Author  ：dxc
@Date    ：2021/11/8 13:39 
"""
from openpyxl import load_workbook
from openpyxl.drawing.image import Image
from openpyxl_image_loader import SheetImageLoader


class ExcelOpt:
    """
    操作excel
    """

    def __init__(self, excel_path, excel_sheet):
        self.wb = load_workbook(excel_path)
        self.sheet = self.wb[excel_sheet]
        self.image_loader = None

    def excel_opt(self):
        """
        一些基本操作
        :return:
        """
        # 获取所有的有效行数
        rows = self.sheet.max_row
        # 获取所有的列数
        cols = self.sheet.max_column
        # 读数据
        value = self.sheet.cell(row=1, column=1).value
        # 将测试内容写在 (1,1)这个位置
        self.sheet.cell(row=1, column=1, value="测试")
        # 保存
        self.wb.save('test.xlsx')

    def export_img(self):
        """
        导出图片
        :return:
        """
        self.image_loader = SheetImageLoader(self.sheet)
        image = self.image_loader.get('B1')
        # image.show()
        image.save("xxx01.png")

    def check_is_images(self):
        """
        检查该单元格图片是否存在
        :return:
        """
        return True if self.image_loader.image_in('C15') else False

    def write_img(self):
        """
        将图片写进excel
        :return:
        """
        img = Image('xxx01.png')
        self.sheet.add_image(img, 'C1')
        self.wb.save('test.xlsx')
```

## 绝对定位图片

```python
wb = Workbook()
ws = wb.active

img = Image(imgPath)  # 缩放图片
ws.column_dimensions['C'].width = img.width * 0.14  # 修改列A的宽
ws.row_dimensions[2].height = img.height * 0.78  # 修改列第1行的高

# 插入的第一张
x, y = 9 * 2 // 0.14, 13.5 // 0.65
w, h = img.width, img.height

p2e = pixels_to_EMU  # openpylx自带的像素转EMU
pos = XDRPoint2D(p2e(x), p2e(y))  # 设置绝对位置
size = XDRPositiveSize2D(p2e(w), p2e(h))  # 图片大小
anchor = AbsoluteAnchor(pos=pos, ext=size)

ws.add_image(img, anchor=anchor)

wb.save('test.xlsx')  # 新的结果保存输出
```

## 相对定位图片

```python
wb = Workbook()
ws = wb.active

# 第一张图片 设置图片宽高
img_width = 120
img_height = 160

img = Image(imgPath)  # 缩放图片
img.width = img_width
img.height = img_height

# 高大于宽
# 设置宽高 从1开始
ws.column_dimensions['C'].width = img.width * 0.2  # 修改列A的宽
ws.row_dimensions[6].height = img.height * 0.78  # 修改列第1行的高


c2e = cm_to_EMU
cellh = lambda x: c2e((x * 49.77) / 99)
cellw = lambda x: c2e((x * (18.65 - 1.71)) / 10)
column = 2
coloffset = cellw(0.15)
row = 5
rowoffset = cellh(1)
marker = AnchorMarker(col=column, colOff=coloffset, row=row, rowOff=rowoffset)
p2e = pixels_to_EMU
h, w = img.height, img.width
size = XDRPositiveSize2D(p2e(h), p2e(w))
img.anchor = OneCellAnchor(_from=marker, ext=size)
ws.add_image(img)
wb.save('test.xls')
```



