## pandas使用

### 一、pandas简介

pandas是一种建立在python基础上的快速，强大，灵活并且易于使用的开源数据分析和处理工具。是基于numpy实现的，具有存储表格数据、统计分析、数据清洗功能。

主要有两种数据结构：**DataFrame**和**Series**。

### 二、pandas使用

1. #### 两种数据结构的使用

    ```python
    import pandas as pd
    import numpy as np
    
    # 用来存储数据 ---两种结构
    
    # DataFrame ---具有行索引、列索引的表格数据
    #           ---可以存储不同的数据类型
    
    # 创建df --列表嵌套
    data = [['zs', 19, 1],
            ['ls', 18, 2],
            ['ww', 20, 1],
            ['zl', 19, 2]]
    print('data:\n', data)
    print('data的类型：\n', type(data))
    
    # 将data 转化df
    df = pd.DataFrame(data=data,  # 数据，
                      index=['stu0', 'stu1', 'stu2', 'stu3'],  # 行名称、行索引
                      columns=['name', 'age', 'group'],  # 列索引、列名称
                      )
    
    print('df:\n', df)
    print('df的类型：\n', type(df))  # <class 'pandas.core.frame.DataFrame'>
    
    # 创建df ---使用大字典
    df = pd.DataFrame(
        data={
            'name': ['zs', 'ls', 'ww', 'zl'],
            'age': [19, 18, 20, 19],
            'group': [1, 2, 1, 2]
        },
        index=['stu0', 'stu1', 'stu2', 'stu3']  # 行索引
    )
    print('df:\n', df)
    print('df的类型：\n', type(df))
    
    # 将ndarray数组转化为df
    # 先加载ndarray
    res = np.load('./国民经济核算季度数据.npz')
    # 获取数组
    columns = res['columns']
    values = res['values']
    print('columns:\n', columns)
    print('values:\n', values)
    # 构建行索引名称
    index = ['index_' + str(tmp) for tmp in range(values.shape[0])]
    # 将columns 与 values 结合起来
    df = pd.DataFrame(data=values,
                      columns=columns,
                      index=index)
    print('df:\n',df)
    print('df的类型：\n',type(df))
    
    # Series ----只有行索引、数据的表格结构
    #        ----数据的类型都是一致
    
    # 创建series --由简单列表、一维数组转化为series
    se = pd.Series(data=np.array(['zs', 'ls', 'ww', 'zl']),  # 数据
                   index=['stu0', 'stu1', 'stu2', 'stu3'])  # 行索引
    print('se:\n', se)
    print('se的类型：\n', type(se))  # <class 'pandas.core.series.Series'>
    
    
    # 创建series --通过映射关系来创建series，并按照index进行排序
    # 基础班 ---字典是无序(仅限于Python3.7版本之前)
    scores = {'zs': 90, 'ls': 80, 'ww': 60, 'zl': 48}
    index = ['zs', 'zl', 'ww', 'ls', 'kk']
    se = pd.Series(data=scores,
                   index=index  # 行名称，指定排序规则
                   )
    print('se:\n', se)
    print('se的类型：\n', type(se))
    
    # 从df中获取一列数据 ---series  ---特殊的df(一列的且无列名称的特殊的df)
    se = df['name']
    print('se:\n', se)
    print('se的类型：\n', type(se))
    ```

2. #### dataframe和series属性

    ```python
    import pandas as pd
    
    # 创建一个dataframe
    df = pd.DataFrame(
        data={
            'name': ['zs', 'ls', 'ww', 'zl'],
            'age': [18, 19, 20, 18],
            'group': [1, 2, 2, 1]
        },
        index=['stu0', 'stu1', 'stu2', 'stu3']
    )
    print('df:\n', df)
    
    print('df的类型：\n', type(df))
    print('*' * 100)
    
    """
        dataframe属性
        values index columns dtypes size ndim shape
        df中的数据元素
        可以将df 通过df.values转化为ndarray
        pd.DataFrame将ndarray转化为df
    """
    print('df的values:\n', df.values)
    print('df的values的类型：\n', type(df.values))  # <class 'numpy.ndarray'>
    print('df的index :\n', df.index)  # 行索引
    print('df的columns:\n', df.columns)  # 列索引
    
    # df可以存储不同类型的数据
    print('df的dtypes:\n', df.dtypes)  # df中每一列的数据类型
    """
    df的dtypes:
     age       int64
    group     int64
    name     object
    dtype: object
    """
    
    print('df的size：\n', df.size)  # 元素个数
    print('df的ndim:\n', df.ndim)  # 维数---->2维
    print('df的shape:\n', df.shape)  # 形状  (行，列)
    
    
    """series属性"""
    se = df['age']
    print('se：\n', se)
    print('se的类型：\n', type(se))
    print('*' * 100)
    
    """
        相比于dataframe --->没有columns属性
        values index dtypes/dtype ndim shape size itemsize
        se中的数据元素
        pd.Series可以将一维数组转化series
        也可以通过se.values将Series转化为ndarray
    """
    print('se的values:\n', se.values)
    print('se的values的类型:\n', type(se.values))  # <class 'numpy.ndarray'>
    
    print('se的index:\n', se.index)  # 行索引
    print('se的dtypes:\n', se.dtypes)  # 元素类型
    print('se的dtype:\n', se.dtype)  # 元素类型
    
    print('se的ndim:\n', se.ndim)  # 维度--->1维
    print('se的shape：\n', se.shape)  # 形状  #  (4,)  --->只有一个维度--->行维度
    print('se的size:\n', se.size)  # 元素个数
    
    print('se的itemsize:\n', se.itemsize)  # 每一个元素的占位大小
    
    ```

3. #### 读写文件

    ```python
    import pandas as pd
    
    
    """
        常用的文件excel文件、csv文件、html文件、json文件....
        1、pandas读取文件
            pandas里面读取文件格式：pd.read_xxx格式
            pd.read_csv()
            pd.read_table()
            csv文件 ---特殊的以逗号分隔的、文本的序列文件
    """
    # 读取csv文件
    info = pd.read_csv(filepath_or_buffer='./meal_order_info.csv',  # 文件路径+名称
                       encoding='ansi',  # 编码方式
                       # delimiter=',',  # 分隔符
                       # sep=','  # 分隔符
                       # header='infer',  # 读取的文件的时候，列索引自动识别，也可以显式指定
                       # header=0,  # 显式指定第n行为 列索引
                       # names=['列1', '列2'],  # 可以自定义列索引名称
                       # usecols=[0, 1]  # 可以指定读取的列下标
                       # index_col=0,  # 指定特定的列作为行索引名称
                       # nrows=10,  # 可以指定读取前n行
                       )
    # 参数参考read_csv理解,和read_csv的区别在于：没有默认的分隔符
    # info = pd.read_table('./meal_order_info.csv', sep=',', encoding='ansi')
    
    print('info:\n', info)
    print('info 的类型:\n', type(info))  # <class 'pandas.core.frame.DataFrame'>
    
    # excel文件 ---以.xlsx .xls为结尾的表格数据文件
    detail = pd.read_excel(io='./meal_order_detail.xlsx',  # 文件路径 + 名称
                           sheetname=None,  # 读取表格下标
                           # header=0,  # 以表格的第0行为列索引
                           # # index_col=0 ,# 可以指定特定列为行索引
                           # parse_cols=[0, 1],  # 读取指定的列
                           )
    print('detail:\n', detail)  # --->OrderedDict
    
    # 可以通过
    print(detail.keys())  # odict_keys(['meal_order_detail1', 'meal_order_detail2', 'meal_order_detail3'])
    print('*' * 100)
    # 获取不同的sheet
    detail_1 = detail['meal_order_detail1']
    detail_2 = detail['meal_order_detail2']
    detail_3 = detail['meal_order_detail3']
    print('detail_1:\n', detail_1)
    
    
    """
        2、pandas保存文件
            pandas保存文件的格式：df.to_xxx格式
            csv文件保存
    """
    info.to_csv(path_or_buf='./aaa.csv',  # 保存的路径+名称
                sep=',',  # 分隔符
                header=True,  # 需要保存列索引,如果不需要保存列索引--->header=False
                index=True,  # 需要保存行索引，如果不需要保存行索引---->index=False
                # columns=['info_id','emp_id']  # 指定需要保存的列
                mode='a',  # 没保存一次，之前的内容都会被覆盖掉，如果想要追加保存--->mode='a'
                )
    
    # excel文件保存
    # 一次只能保存一个sheet
    detail_1.to_excel(excel_writer='./bbb.xlsx',  # 具体的路径+ 名称 或者ExcelWriter对象
                      sheet_name='Sheet1',  # 默认保存的表格的名称
                      header=True,  # 保存列索引，如果不保存--header=False
                      index=True,  # 保存行索引，如果不保存，---index=False
                      # startrow=20,  # 保存的文件里面跳过指定行继续保存
                      # startcol=5, # 保存的文件里面跳过指定列继续保存
                      )
    
    detail_2.to_excel(excel_writer='./bbb.xlsx',  # 具体的路径+ 名称  或者ExcelWriter对象
                      sheet_name='Sheet2',  # 默认保存的表格的名称
                      header=True,  # 保存列索引，如果不保存--header=False
                      index=True,  # 保存行索引，如果不保存，---index=False
                      # startrow=20,  # 保存的文件里面跳过指定行继续保存
                      # startcol=5, # 保存的文件里面跳过指定列继续保存
                      )
    
    # 如果按照上面的方式进行保存---->每保存一次，覆盖一次
    
    # 将多个df分别保存到相同文件的不同sheet中去
    
    # 可以借助 ExcelWriter 来进行保存
    # 创建ExcelWriter对象
    writer = pd.ExcelWriter('./ccc.xlsx')
    
    """
        将不同df 保存到不同sheet中
        写入数据
    """
    detail_1.to_excel(excel_writer=writer, sheet_name='sheet1')
    detail_2.to_excel(excel_writer=writer, sheet_name='sheet2')
    detail_3.to_excel(excel_writer=writer, sheet_name='sheet3')
    
    # # 保存修改
    writer.save()
    # # 关闭ExcelWriter对象
    writer.close()
    ```

4. #### dataframe的查询操作

    查询操作主要根据列索引和切片获取数据。

    ```python
    import pandas as pd
    
    # 加载detail数据
    # 默认加载第0个sheet
    detail = pd.read_excel('./meal_order_detail.xlsx')
    
    # 修改一下行索引
    index = ['index_' + str(tmp) for tmp in detail.index]
    detail.index = index
    
    print('detail:\n', detail)
    print('detail的列索引：\n', detail.columns)
    print('*' * 100)
    
    # df索引方式--->先列后行(直接索引方式)
    # ndarray索引---arr[行,列] ---同时索引
    
    # 获取 dishes_name 这一列数据 --列名 --Series
    print('获取单列数据：\n', detail['dishes_name'])
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe
    print('获取多列数据：\n', detail[['dishes_name', 'dishes_id', 'amounts', 'counts']])
    
    """
    获取单列数据指定行---->行下标列表、行名称列表、行下标切片、行名称切片
                    ---->head(获取前n行)
                    ---->tail(获取后n行)
    """
    
    # 获取 dishes_name 这一列 数据 的前n行  ---行下标切片
    print('获取单列数据：\n', detail['dishes_name'][:5])
    
    # # 获取 dishes_name 这一列 数据 的前n行  ---行名称切片 ---包含结束位置
    print('获取单列数据：\n', detail['dishes_name'][:'index_4'])
    
    # 获取 dishes_name 这一列 数据 的前n行  ---行下标列表
    print('获取单列数据：\n', detail['dishes_name'][[0, 1, 2, 3, 4]])
    
    # 获取 dishes_name 这一列 数据 的前n行  ---行名称列表
    print('获取单列数据：\n', detail['dishes_name'][['index_0', 'index_1', 'index_2', 'index_3', 'index_4']])
    
    
    # # 获取 dishes_name 这一列 数据 的前n行  ---head()
    print('获取单列数据：\n', detail['dishes_name'].head(10))
    
    # 获取 dishes_name 这一列 数据 的后n行  ---tail()
    print('获取单列数据：\n', detail['dishes_name'].tail(10))
    
    """
    获取多列数据的前n行---->行下标切片、行名称切片
                     ---->head(获取前n行)
                     ---->tail(获取后n行)
    """
    
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe --->前n行数据 --不能使用行下标列表
    # print('获取多列数据：\n', detail[['dishes_name', 'dishes_id', 'amounts', 'counts']][[0, 1, 2, 3, 4, 5]])  # 此时是错误的，不能使用行下标列表
    
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe --->前n行数据 --不能使用行名称列表
    # print('获取多列数据：\n',
    #       detail[['dishes_name', 'dishes_id', 'amounts', 'counts']][['index_0', 'index_1', 'index_2']])  # 此时是错误的，不能使用行名称列表
    
    
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe --->前n行数据 --行下标切片 --可行
    print('获取多列数据：\n',detail[['dishes_name', 'dishes_id', 'amounts', 'counts']][0:5])
    
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe --->前n行数据 --行名称切片 --可行
    print('获取多列数据：\n',detail[['dishes_name', 'dishes_id', 'amounts', 'counts']]['index_0':'index_5'])
    
    
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe --->前n行数据 --head --可行
    print('获取多列数据：\n', detail[['dishes_name', 'dishes_id', 'amounts', 'counts']].head(10))
    
    # 获取  dishes_name dishes_id amounts counts 这四列数据 --列名列表 --dataframe --->后n行数据 --tail--可行
    print('获取多列数据：\n', detail[['dishes_name', 'dishes_id', 'amounts', 'counts']].tail(10))
    
    
    ```

    