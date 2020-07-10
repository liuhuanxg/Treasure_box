## matplotlib使用

matplotlib是一个可视化库，用于在python中创建静态，动画和交互式可视化。绘图流程主要包含以下三步：

- 创建画布
- 绘制图形
- 图形展示

1. #### 简单图形示例

    ```python
    import matplotlib.pyplot as plt
    import numpy as np
    
    # 绘图三部曲
    # (1)创建画布
    # 参数： figsize ---画布大小
    # 参数： dpi ---像素
    # 返回值： 返回画布对象
    plt.figure()
    # (2)绘制图形
    # 绘制下一周天气走势---折线图
    # 折线图---要素：点 ---坐标轴：x,y坐标 ---(x,y)
    # 绘制折线图的时候---准备 (x1,y1) (x2,y2) (x3,y3) ....
    # 绘制折线图的时候，不需要配对xy---只需要准备好一个x数组，一个y的数组
    # 横轴---周一、周二、 ...、周日
    # 纵轴---不同的温度
    # 如果横轴是中文，一般先用序号代替，后续再进行将序号替换为中文
    x = np.arange(1, 8)
    y = np.array([15, 20, 22, 23, 20, 18, 16])
    print('x:\n', x)
    print('y:\n', y)
    
    # 绘制
    plt.plot(x,y)
    
    # (3)图形展示
    plt.show()
    ```

2. #### 图形修饰

    ```python
    import matplotlib.pyplot as plt
    import numpy as np
    
    # 绘图三部曲
    # (1)创建画布
    # 参数： figsize ---画布大小
    # 参数： dpi ---像素
    # 返回值： 返回画布对象
    plt.figure()
    # 默认不支持中文
    # 修改参数，让其支持中文
    plt.rcParams['font.sans-serif'] = 'SimHei'
    # 修改参数让其重新支持负号
    plt.rcParams['axes.unicode_minus'] = False
    # (2)绘制图形
    # 绘制下一周天气走势---折线图
    # 折线图---要素：点 ---坐标轴：x,y坐标 ---(x,y)
    # 绘制折线图的时候---准备 (x1,y1) (x2,y2) (x3,y3) ....
    # 绘制折线图的时候，不需要配对xy---只需要准备好一个x数组，一个y的数组
    # 横轴---周一、周二、 ...、周日
    # 纵轴---不同的温度
    # 如果横轴是中文，一般先用序号代替，后续再进行将序号替换为中文
    x = np.arange(1, 8)
    y_bj = np.array([15, 20, 22, 23, 20, 18, 16])
    y_heb = np.array([-10, -8, -12, -10, -8, -6, 1])
    
    print('x:\n', x)
    print('y_bj:\n', y_bj)
    print('y_heb:\n', y_heb)
    
    # color --线的颜色
    # linestyle --线的样式
    # linewidth --线的宽度
    # marker --点的样式
    # markersize --点的大小
    # markerfacecolor --点的填充颜色
    # markeredgecolor --点的边缘颜色
    # 绘制
    plt.plot(x, y_bj, color='r', linestyle=':', linewidth=1.2, marker="*", markersize=7, markerfacecolor='b',
             markeredgecolor='g')
    
    plt.plot(x, y_heb, color='#2F4F4F', linestyle='-.', linewidth=1.2, marker="d", markersize=7, markerfacecolor='r',
             markeredgecolor='r')
    
    # 增加标题
    plt.title('北京、哈尔滨下一周天气温度走势图')
    # 修改横轴名称
    plt.xlabel('日期')
    
    # 修改纵轴名称
    plt.ylabel('温度/℃')
    
    # 将横轴的序号 替换为中文 --修改横轴刻度
    # 注意：如果修改的时候，是将序号替换为中
    # 参数1 ：需要被替换的序号
    # 参数2 ：替换之后的中文
    # 构建中文日期列表
    xticks = ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
    #  rotation=45  旋转角度
    plt.xticks(x, xticks, rotation=45)
    
    # 修改纵轴刻度
    # 重新设置新的显示的刻度范围
    # 注意：只需要将新的刻度范围传递进去
    # 并不改变真实点的纵坐标，只是修改其显示位置
    yticks = np.arange(-15, 31, 3)
    plt.yticks(yticks)
    
    # 增加图例
    # loc 图例的位置
    plt.legend(['北京', '哈尔滨'], loc=4)
    
    # 标注
    # plt.text ---一次标注一个点
    # 循环标注
    for i, j in zip(x, y_bj):  # zip打包函数
        # i 点的横坐标
        # j 点的纵坐标
        # 参数1 标注的横坐标
        # 参数2 标注的纵坐标
        # 参数3 标注的内容---str
        plt.text(i, j + 1, '%d℃' % j,
                 horizontalalignment='center',  # 水平居中
                 # verticalalignment='bottom'  # 点的底部
                 )
    
    for i, j in zip(x, y_heb):  # zip打包函数
        # i 点的横坐标
        # j 点的纵坐标
        # 参数1 标注的横坐标
        # 参数2 标注的纵坐标
        # 参数3 标注的内容---str
        plt.text(i, j + 1, '%d℃' % j,
                 horizontalalignment='center',  # 水平居中
                 # verticalalignment='bottom'  # 点的底部
                 )
    
    # 将图片进行保存
    plt.savefig('./北京、哈尔滨下一周天气温度走势图.png')
    
    # (3)图形展示
    plt.show()
    ```

3. #### 柱状图

    ```python
    import matplotlib.pyplot as plt
    
    
    labels = ['G1', 'G2', 'G3', 'G4', 'G5']
    men_means = [20, 35, 30, 35, 27]
    women_means = [25, 32, 34, 20, 25]
    men_std = [2, 3, 4, 1, 2]
    women_std = [3, 5, 2, 3, 3]
    width = 0.35       
    
    fig, ax = plt.subplots()
    
    ax.bar(labels, men_means, width, yerr=men_std, label='Men')
    ax.bar(labels, women_means, width, yerr=women_std, bottom=men_means,
           label='Women')
    
    ax.set_ylabel('Scores')
    ax.set_title('Scores by group and gender')
    ax.legend()
    
    plt.show()
    ```

更多示例参见官网案例：[https://matplotlib.org/gallery/index.html](https://matplotlib.org/gallery/index.html)