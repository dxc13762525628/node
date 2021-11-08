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



