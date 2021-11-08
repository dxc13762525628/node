# python

## 处理时间

```python
# 获取当前年 月 日
year = str(datetime.datetime.now().year)
month = str(datetime.datetime.now().month)
day = str(datetime.datetime.now().day)

# 将字符串时间转datetime格式
dt_1 = '2021-12'
create_time_start = datetime.datetime.strptime(dt_1, '%Y-%m')
# 将月份加1
create_time_end = create_time_start - relativedelta(months=-1)
```

