# Pandas 操作Excel

## 1.导入数据源

```
#导入相关库
import pandas as pd 
import numpy as np 
import os 
from pandas import DataFrame,Series 
import re 
df =pd.read_csv(r'D:\ProjectCode\forestfires.csv') #打开文件 
```

## **二、数据基本处理**

**1**）查看列名和数据类型

```
print(df.columns) #查看列名print(df.dtypes) #查看各列数据类型
```

**2**）查看指定行列数据

```
print(df.head(20)) #查看前20行数据df=df.loc[:,'FFMC':'rain'] #选择FFMC到rain列所 有数据
```

**3**）删除行或列

```
df=df.drop(['wind', 'rain', 'area'],axis=1) #删除wind,rain和area三列 df_an=df_an.loc[-(df_an['qudao']=='Total')] #删除qudao列等于'Total'的行
```

**4**）移除重复数据

```
df_new=df.drop_duplicates(['month','day']) #移除month和day列包含重复值得行，保留第一 个df_new=df.drop_duplicates(['month'],take_last=True )#移除month列包含重复值得行，保留 最后一个
```

**5**）更改列名

```
df.rename(columns={'ISI':'newIss'}, inplace = True) #ISI列列名改为newIss
```

## 三、筛选

**1**）条件筛选loc

```
df_sel=df.loc[(df['month']=='aug') & (df['DC']>=600)] #筛选month列等于aug且DC列大于 600的所有行
```

**2**）筛选并给新列赋值

这个多用于区间匹配，例如如果A列(0,100],C列为50；A列大于100 ,C列为A列的值

```
df.loc[(df['DC']>0) & (df['DC']<=100) ,'DC_na']=50 # 创建新列DC_na,DC列大于0且小于等 于100，DC列为50 df.loc[df['DC']>100,'DC_na']=df['DC']# 创建新列DC_na,DC列大于100等于原值，其他为NA
```

