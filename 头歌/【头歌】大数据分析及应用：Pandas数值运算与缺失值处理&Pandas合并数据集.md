- # Pandas数值运算与缺失值处理

  - ## 第1关：Pandas数值运算方法

    ```python
    import pandas as pd
    from sklearn import datasets
    
    def demo1():
        iris = datasets.load_iris().data    # 鸢尾花数据集，返回的是array
        #********** Begin **********#
        df = pd.DataFrame(iris[:30], columns=list('abcd'))
        print(df.sub(df.iloc[0]))
        #********** End **********#
    ```

  - ## 第2关：Pandas缺失值类型

    1. 根据相关知识，计算`1 + np.nan`、`1 + None`、`np.nan + None`的结果。

       - A、'TypeError'、'TypeError'、'TypeError'

       - B、`nan`、`1` 、'TypeError'

       - C、'TypeError'、'TypeError'、'nan'

       - D、`nan` 、'TypeError'、'TypeError'

       - E、`nan` 、'1'、'nan'

       答案：D

    2. 假设`a = [1 ,2 ,None,4]`，`data1 = pd.Series(a)`，`data2 = pd.Series(np.array(a))`，`data1`和`data2`的结果分别是什么？

       - A、

         ```python
         data1: 
         0    1.0
         1    2.0
         2    NaN
         3    4.0
         dtype: object
         data2:
         0       1
         1       2
         2       None
         3       4
         dtype: object
         ```

       - B、

         ```python
         data1:
         0    1.0
         1    2.0
         2    None
         3    4.0
         dtype: float64
         data2:
         0       1
         1       2
         2       None
         3       4
         dtype: object
         ```

       - C、

         ```python
         data1:
         0    1.0
         1    2.0
         2    NaN
         3    4.0
         dtype: float64
         data2:
         0       1.0
         1       2.0
         2       NaN
         3       4.0
         dtype: float64
         ```

       - D、

         ```python
         data1:
         0    1
         1    2
         2    NaN
         3    4
         dtype: float64
         data2:
         0       1
         1       2
         2       None
         3       4
         dtype: object
         ```

       - E、

         ```python
         data1:
         0    1.0
         1    2.0
         2    NaN
         3    4.0
         dtype: float64
         data2:
         0       1
         1       2
         2       None
         3       4
         dtype: object
         ```

       - F、

         ```python
         data1:
         0    1.0
         1    2.0
         2    NaN
         3    4.0
         dtype: float64
         data2:
         0       1.0
         1       2.0
         2       None
         3       4.0
         dtype: float64
         ```

       答案：E

  - ## 第3关：缺失值处理

    ```python
    import numpy as np
    import pandas as pd
    from sklearn import datasets
    
    def demo3():
        iris = datasets.load_iris().data
        #********** Begin **********#
        d = pd.DataFrame(iris[:30],columns=list('abcd'))
        d = d.sub(d.iloc[0])
        d[d < 0] = np.nan
        d=d.dropna(axis=0, thresh=2)
    
        print(d.fillna(method='ffill'))
        #********** End **********#
    ```

- # Pandas合并数据集

  - ## 第1关：Concat与Append操作

    ```python
    import pandas as pd
    
    def task1():
        #********** Begin **********#
        data1 = pd.read_csv('step1/data.csv')
        data2 = pd.read_csv('step1/data1.csv')
        result = pd.concat([data1, data2], axis=1)
        result.set_index('Ladder', inplace=True)
        result.fillna(0, inplace=True)
        #********** End **********#
        return result
    ```

  - ## 第2关：合并与连接

    ```python
    import pandas as pd
    
    
    def task2(dataset1,dataset2,dataset3):
    
        # ********** Begin **********#
        #将字典转换为DataFrame类型
        d1,d2,d3=pd.DataFrame(dataset1),pd.DataFrame(dataset2),pd.DataFrame(dataset3)
    	#根据题意先将d1,d2连接合并，然后再与d3连接合并；参数均为id  
        result=pd.merge(d3,pd.merge(d1,d2,on='user_id',how='outer'),left_on='id',right_on='user_id',how='outer')
        result['user_id']=result['user_id'].fillna(result['id'])
        result['page_click_count_y']=result['page_click_count_y'].fillna(result['page_click_count_x'])
        result['city_x']=result['city_x'].fillna(result['city_y'])
        result=result.drop(['id','page_click_count_x','city_y'],axis=1)
        result.rename(columns={'city_x':'city','page_click_count_y':'page_click_count'},inplace=True)
        result=result.sort_values(by="user_id")
        result['user_id']= result['user_id'].values.astype(int)
    
        # ********** End **********#
        return result
    ```

  - ## 第3关：案例：美国各州的统计数据

    ```python
    import pandas as pd
    import numpy as np
    
    def task3():
        #********** Begin **********#
        #读取三个csv文件
        pop = pd.read_csv('./step3/state-population.csv')
        areas = pd.read_csv('./step3/state-areas.csv')
        abbrevs = pd.read_csv('./step3/state-abbrevs.csv')
        # 合并pop和abbrevs并删除重复列
        merged = pd.merge(pop, abbrevs, how='outer',
        left_on='state/region', right_on='abbreviation')
        merged = merged.drop('abbreviation', 1)
        # 填充对应的全称
        merged.loc[merged['state/region'] == 'PR', 'state'] = 'Puerto Rico'
        merged.loc[merged['state/region'] == 'USA', 'state'] = 'United States'
        # 合并面积数据
        final = pd.merge(merged, areas, on='state', how='left')
        # 删掉这些缺失值
        final.dropna(inplace=True)
        # 取year为2010年的两种人口的数据
        data2010_1= final.query("year == 2010 & ages == 'total'")
        data2010_2=final.query("year == 2010 & ages == 'under18'")
        # 二者人口相加
        p=np.array(data2010_1.loc[:,'population'])+np.array(data2010_2.loc[:,'population'])
        data2010=data2010_1.copy()
        data2010.loc[:,'population']=p
        #设置州为索引
        data2010.set_index('state', inplace=True)
        #计算人口密度
        density = data2010['population'] / data2010['area (sq. mi)']
        # 对值进行排序
        density.sort_values(ascending=False, inplace=True)
        # 输出人口密度前5名和倒数5名
        print('前5名：')
        print(density[:5])
        print('后5名：')
        print(density[-5:])
        #********** End **********#
    ```