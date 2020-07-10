## numpy

### 一、numpy使用

1. #### numpy介绍

    numpy是python的科学计算库，在存储数据时使用连续的内存空间存储，提高了查询效率。numpy内存图如下：

    <img src="image/numpy%E5%86%85%E5%AD%98%E5%9B%BE.png">

2. #### numpy基本属性

    ```python
    # 导入numpy
    import numpy as np
    
    """
    数组的基本属性：
        ndim:       数组的秩(纬度)，一维为1，二维为2
        shape:      数组的类型，为元组格式，示例:二行三列的数组为(2,3)
        size:       数组中元素的个数
        itemsize:   每个整数占用4
        dtype:      数组中数据的类型
        data:       占用的内存地址，也被称为缓冲区
    """
    
    # 创建数组
    arrl=np.array([1,2,3])
    print(type(arrl))   #<class 'numpy.ndarray'>
    
    #例1、一维数组
    print('arr1秩为：',arrl.ndim)            #一维数组秩为1，二维数组秩为2
    print('arr1轴为：',arrl.shape)           #(3,)  3列1行
    print('size为：',arrl.size)              # 3*1
    print('数组中元素的站位大小:',arrl.itemsize)  # 4
    print('元素类型：',arrl.dtype)           # int(32)
    print('每个元素大小：',arrl.itemsize)    # (4)
    print('缓冲区：',arrl.data)              # 内存地址
    print("-------------------------------------------------------")
    
    #例2：二维数组
    arr2=np.array([[1,2,3,4],[4,5,6,7],[7,8,9,10]])
    print(arr2)
    print('arr2秩为：',arr2.ndim)                  #2
    print('arr2轴为：',arr2.shape)                 # (3,4)
    print('size为：',arr2.size)                    # 3*4
    print('数组中元素的站位大小:',arr2.itemsize)   # 4
    print('元素类型：',arr2.dtype)                 # (int(32))
    print('每个元素大小：',arr2.itemsize)          # (4)
    print('缓冲区：',arr2.data)                    # 内存地址
    print("-------------------------------------------------------")
    
    #例3：三维数组
    arr3=np.array([
        [[1,2],[3,4],[5,6]],
        [[7,8],[9,10],[11,12]]
    ])
    print(arr3)
    print('arr3秩为：',arr3.ndim)              #3
    print('arr3轴为：',arr3.shape)             # (2, 3, 2)
    print('size为：',arr3.size)                # 12
    print('数组中元素的站位大小:',arr3.itemsize)  #4
    print('元素类型：',arr3.dtype)             # (int(32))
    print('每个元素大小：',arr3.itemsize)      # (4)
    print('缓冲区：',arr3.data)                # 内存地址
    ```

3. #### 数组的dtype属性

    ```python
    import numpy as np
    
    # numpy 数据类型---封装了Python中的数据类型---进行细致划分 --->变为numpy.数据类型
    # 可以创建数组的时候进行通过dtype参数指定
    arr = np.array([1, 2, 3, 4, 5, 6], dtype=np.float64)
    print(arr)
    print(arr.dtype)
    
    # 也可以进行强制转化
    # print(np.float(3))
    # print(np.int32(3.0))
    # 0代表False, 非0代表True
    # print(np.bool(1))
    # print(np.bool(0))
    
    # ndarray 用来存储单一数据类型的数据 ---运算的
    # 也可以用来存储 复合类型 --不推荐使用
    # 存储个人信息
    # df = np.dtype([('name', np.str, 40), ('height', np.float64), ('weight', np.float64)])
    # # 创建一个数组
    # arr = np.array([('zs', 178.0, 55.5), ('ls', 180.5, 65.0), ('ww', 190.0, 80.5)], dtype=df)
    # print(arr)
    
    # 创建bool数组---里面值全是bool类型的数组
    # bool_mask = np.array([0, 1, 1, 0], dtype=np.bool)
    # print(bool_mask)
    
    ```

4. #### 数组的创建

    ```python
    import numpy as np
    '''
    1、使用array函数来创建
    格式： np.array(object,dtype=None,copy=True,oreder='K',subok=False,ndmin=0)
    objece：接受array。表示想要创建的数据，无默认值
    dtype: 接受data-type.表示数组所需的数据类型。如果未给定，则选择保存对象所需的最小类型。默认为None
    ndmin:接收int。指定生成数据应该具有的最小维数，默认为None
    '''
    a1=np.array([1,2,3,4])
    print(a1,a1.dtype)     #[1 2 3 4] int32
    a2=np.array([1,2,3.14,4])
    print(a2,a2.dtype)    #[1.   2.   3.14 4.  ] float64
    a2=np.array([1,2,3.14,4],dtype=int)
    print(a2,a2.dtype)    #[1 2 3 4] int32
    
    a4=np.array([(1,2),(3,4)])
    print(a4,a4.ndim)
    '''
    [[1 2]
     [3 4]] 2
    '''
    a5=np.array([[1,2],[3,4]])
    print(a5,a5.ndim)
    '''
    [[1 2]
     [3 4]] 2
    '''
    
    #2、arange()
    #格式：arange(开始值，终止值，步长)  [开始值，终止值]
    arr6=np.arange(1,9,1)
    print(arr6)  # [1 2 3 4 5 6 7 8]
    
    arr7=np.arange(0,1,0.22)
    print(arr7)  #缺点是元素个数预估有难度
    # [0.   0.22 0.44 0.66 0.88]
    
    #3、linspace
    #格式：linspace(开始值，终止值，元素个数)
    arr8=np.linspace(0.1,1,7)   #float型
    print(arr8)    #[ 0.1   0.25  0.4   0.55  0.7   0.85  1.  ]
    
    
    # 4、使用logspace()函数
    # 生成10~1 到10~3之间的等比例数
    # base参数默认为10，可选
    arr9=np.logspace(1,3,3,base=10)    #float型
    print('arr9',arr9)  # [  10.  100. 1000.]
    
    # 其他函数
    a12=np.zeros((2,3))  #生成2行3列的0
    print(a12)
    """
    [[0. 0. 0.]
     [0. 0. 0.]]
    """
    
    #empty()函数
    a13=np.empty((2,3))
    print(a13)
    """
    [[0. 0. 0.]
     [0. 0. 0.]]
    [[0. 0. 0.]
    """
    
    #eye(N)函数
    #生成N阶矩阵，并且对角线元素为1
    a14=np.eye(3)
    print(a14)
    """
    [[1. 0. 0.]
     [0. 1. 0.]
     [0. 0. 1.]]
    """
    
    #使用diag()函数
    a15=np.diag([1,2,3,4])   #对角线为1,2,3,4.其他为0
    print(a15)
    """
    [[1 0 0 0]
     [0 2 0 0]
     [0 0 3 0]
     [0 0 0 4]]
    """
    ```

5. #### 数组索引

    ```python
    import numpy as np
    
    # # 创建一个一维数组
    # arr = np.arange(1, 13, 1)
    # print(arr)
    #
    # print('arr的维度：', arr.ndim)
    # print('arr的形状：', arr.shape)
    # print('*' * 100)
    #
    # # 使用下标--会降低维度
    # # 获取 3 ---下标
    # print('获取3元素:', arr[2])
    # # 获取11 --下标
    # print('获取11元素：', arr[-2])
    #
    # # 使用切片不降低维度
    # # 获取3 --切片 ---步长为1 可以省略
    # print('获取3元素：', arr[2:3:1])
    #
    # # 获取11 --切片
    # print('获取11元素：', arr[-2:-1:1])
    #
    # # 获取 3 和 11  -->下标列表
    # # ---先获取到具体的值，然后将获取多个值组合起来，就得到最终的结果
    # print('获取3和11元素：', arr[[2, -2]])
    #
    # # 获取3 和 11 ---按照规律
    # print('获取3 和11元素：', arr[2:-1:8])
    
    # 创建二维数组
    arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13, 14, 15, 16]])
    print(arr)
    print('arr的维度：', arr.ndim)
    print('arr的形状：', arr.shape)
    
    # # 获取 5元素 ---下标
    # print('获取5元素：', arr[1, 0])
    #
    # # 获取11 元素---下标
    # print('获取11元素：', arr[2, 2])
    #
    # # 获取 5元素 --切片
    # print('获取 5元素：', arr[1:2, 0:1])
    #
    # # 获取 11 元素---切片
    # print('获取11元素：', arr[2:3, 2:3])
    #
    # # 获取5 元素 --下标、切片配合
    # print('获取5 元素：', arr[1, 0:1])
    #
    # # 获取11元素 --下标、切片配合
    # print('获取11元素：', arr[2:3, 2])
    #
    # # 获取多个元素
    # # 获取 6  7  10  11  -下标列表 ---将多个具体的值组合起来
    # print('获取6 7 10 11：', arr[[1, 1, 2, 2], [1, 2, 1, 2]])
    #
    # # 获取6 7 10 11 --切片
    # #  [[ 6  7]
    # #  [10 11]]
    # print('获取6 7 10 11：\n', arr[1:3, 1:3])
    #
    # # 获取 6  8 13  16  ---下标列表
    # print('获取6 8 13 16：', arr[[1, 1, 3, 3], [1, 3, 0, 3]])
    
    # 存在arr ---3维 ---->arr[块索引,行索引,列索引]
    # 存在arr ---n维 ---->arr[n-1个逗号将各个维度分开] 在各个维度上单独索引
    
    
    # 索引方式---思维方式
    # 获取 7 元素
    # print('获取7元素：', arr[1][2])  # 高维度是由多个低维度组成
    
    # bool数组索引
    # 利用bool数组的特性来进行索引
    # 数组里面全部为 bool值的数组
    bool_mask = np.array([0, 1, 1, 0], dtype=np.bool)
    print(bool_mask)
    # 利用bool数组索引
    # 保留True 干掉False
    # print('bool数组索引：\n', arr[bool_mask, :])
    print('bool数组索引：\n', arr[:, bool_mask])
    
    
    # 参考都使用下标列表形式理解
    # arr[[F,T,T,F],[F,T,T,F]]
    # print('bool数组索引：\n', arr[bool_mask, bool_mask])
    ```

6. #### 数组shape属性

    ```python
    import numpy as np
    
    # 创建数组
    # arr = np.arange(16)
    # print(arr)
    # print('arr的维度：', arr.ndim)
    # print('arr的形状：', arr.shape)
    # print('*' * 100)
    
    # 元素个数不能变化
    # 形状更改 ---shape属性重新赋值
    # arr.shape = (4, 4)
    # print(arr)
    
    # reshape --更改形状 --返回新的数组
    # arr = arr.reshape((4,4))
    # print(arr)
    
    # 同时使用np.arange 和 reshape
    arr = np.arange(16).reshape((4, 4))
    print(arr)
    
    # 将高维度 展开为一维数组
    # new_arr = arr.flatten()  # 返回的是一个拷贝
    # 参数 C --行优先展开
    # F---列优先展开
    # new_arr = arr.ravel()  # 返回自身的视图
    # print(new_arr)
    
    # -1 只是占位作用
    # new_arr = arr.reshape((-1,))
    # new_arr = arr.reshape((-1, 1))
    # print(new_arr)
    ```

7. #### 组合数组

    ```python
    import numpy as np
    
    # 创建数组
    arr1 = np.arange(4).reshape((2, 2))
    arr2 = np.array([[0, 1], [0, 1]])
    print('arr1:\n', arr1)
    print('arr2:\n', arr2)
    print('*' * 100)
    
    # 合并
    # 向下拼接 ----垂直拼接
    # res = np.vstack((arr1,arr2))
    # print(res)
    
    # 向右拼接 ----水平拼接
    # res = np.hstack((arr1, arr2))
    # print(res)
    
    # # 行的方向进行拼接---垂直拼接
    # res = np.concatenate((arr1, arr2), axis=0)
    # print(res)
    #
    # # 列的方向进行拼接---水平拼接
    # res = np.concatenate((arr1, arr2), axis=1)
    # print(res)
    
    ```

8. #### 数组拆分

    ```python
    import numpy as np
    
    # 创建数组
    arr = np.arange(16).reshape((4, 4))
    print(arr)
    print('*' * 100)
    
    # 第二个必须是能够被整除的，均匀拆分
    # 水平拆分  ---将水平的行 打断
    # res = np.hsplit(arr, 2)
    # print(res)
    
    # 垂直拆分 ---将垂直的列 打断
    # res = np.vsplit(arr, 2)
    # print(res)
    
    # 把行的方向拆成 2部分 ---垂直拆分
    # res = np.split(arr, 2, axis=0)
    # print(res)
    
    # 把列的方向拆成2部分---水平拆分
    # res = np.split(arr, 2, axis=1)
    # print(res)
    
    
    # 可以不均匀拆分
    # 把行的方向进行拆分
    # ---这么拆：[:1] [1:3],[3:]
    # ---[:开始值]
    # ---[开始值：结束值]
    # ---[结束值：]
    # res = np.split(arr, [1, 3], axis=0)[0]
    # print(res)
    # print('*' * 100)
    # res = np.split(arr, [1, 3], axis=0)[1]
    # print(res)
    
    
    # 前n列为一部分，最后一列为一部分
    # 使用索引
    # part_1 = arr[:, :-1]
    # part_2 = arr[:, -1:]
    # print('part_1:\n',part_1)
    # print('part_2:\n',part_2)
    
    ```

9. #### 生成矩阵

    ```python
    import numpy as np
    
    # 生成矩阵
    # 可以使用np.mat  np.asmatrix来创建矩阵
    # np.mat = np.asmatrix 一样的
    # np.mat 将特殊字符串转化为矩阵
    # m1 = np.mat('1 2 3;4 5 6;7 8 9')
    # print(m1)
    # print(type(m1))  # <class 'numpy.matrixlib.defmatrix.matrix'>
    # print(m1.ndim)  # 2维 ---只能是2维
    # print(m1.shape)  # (3,3)
    # print(m1.itemsize)
    # print(m1.dtype)
    # print(m1.size)
    
    
    # # 也可以将列表嵌套转化为矩阵
    # m1 = np.asmatrix([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
    # print(m1)
    # print(type(m1))  # <class 'numpy.matrixlib.defmatrix.matrix'>
    #
    #
    # # 也可以将二维数组转化为矩阵
    #
    # # 创建一个二维数组
    # arr = np.arange(9).reshape((3, 3))
    #
    # m1 = np.mat(arr)
    #
    # print(m1)
    # print(type(m1))  # <class 'numpy.matrixlib.defmatrix.matrix'>
    
    
    # 可以使用np.bmat来组合矩阵
    arr1 = np.array([[1, 2], [1, 2]])
    arr2 = np.arange(4).reshape((2, 2))
    print('arr1:\n', arr1)
    print('arr2:\n', arr2)
    print('*' * 100)
    
    # 利用np.bmat将数组组合成矩阵
    # res = np.bmat('arr1 arr2;arr2 arr1')
    # res = np.bmat([[arr1, arr2], [arr2, arr1]])  # 列表对象
    # print(res)
    # print(type(res))  # <class 'numpy.matrixlib.defmatrix.matrix'>
    
    # 可以使用np.bmat 可以将 二维数组转化为矩阵
    res = np.bmat(arr1)
    # res = np.bmat('1 2 3;4 5 6;7 8 9')  # 错误的
    # res = np.bmat([[1, 2, 3], [4, 5, 6], [7, 8, 9]])  # 错误的
    print(res)
    print(type(res))  # <class 'numpy.matrixlib.defmatrix.matrix'>
    ```

    