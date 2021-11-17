# python

环境

python3.6.8   pip==18.1 setuptools==40.6.2

## 处理时间

### datetime

#### 时间的加减

```python
# 年月日 		 relativedelta
# 小时分钟 		datetime.timedelta
# 获取当前年 月 日
year = str(datetime.datetime.now().year)
month = str(datetime.datetime.now().month)
day = str(datetime.datetime.now().day)

# 当前时间 时间的加减
now_time = datetime.datetime.now()  # 2021-11-11 17:38:28.550028
# 加一年 一月一天
new_time = now_time + \
           relativedelta(years=1) + \
           relativedelta(months=1) + \
           relativedelta(days=1)  # 2022-12-12 17:40:18.709746
# +3小时 +9分钟
_new_time = new_time \
            + datetime.timedelta(hours=3) \
            + datetime.timedelta(minutes=9)  # 2022-12-12 20:51:18.859806
        

```

#### 字符串与时间

```python
# strptime str->datetime
# strftime datetime->str
demo_time = '2012-05-29 19:30:02'
# 字符串转时间
str_to_time = datetime.datetime.strptime(demo_time,
                                         "%Y-%m-%d %H:%M:%S")  # 2012-05-29 19:30:02 <class 'datetime.datetime'>
# 时间转字符串
time_to_str = datetime.datetime.strftime(datetime.datetime.now(), "%Y-%m")  # 2021-11 <class 'str'>
```

#### 时间的比较

```python
# 计算差多少天
des1 = datetime.datetime(2021, 11, 11)
des2 = datetime.datetime(2020, 8, 19)
days = (des1 - des2).days  # 449
```

## pymongo
