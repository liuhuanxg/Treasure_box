## random模块的使用

random主要用于生成随机数使用：

| 函数                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| random.seed(a=None, version=2)                               | 初始化伪随机数生成器。如果未提供a或者a=None，则使用系统时间为种子。如果a是一个整数，则作为种子。 |
| random.getstate()                                            | 返回一个当前生成器的内部状态对象                             |
| random.setstate(state)                                       | 传入一个先前利用getstate方法获得的状态对象，使得生成器恢复到这个状态。 |
| random.getrandbits(k)                                        | 返回range(0,2\*\**k)之间的一个整数，相当于randrange(0,2\*\**k) |
| random.randrange(start, stop[, step])                        | 返回range[start,stop)之间的一个整数，可加step，跟range(0,10,2)类似 |
| random.randint(a, b)                                         | 返回range[a,b]之间的一个整数，等价于range(a,b+1)             |
| random.choice(seq)                                           | 非空序列seq中随机选取一个元素。如果seq为空则弹出 IndexError异常。 |
| random.choices(population, weights=None, *, cum_weights=None,k=1) | 3.6版本新增。从population集群中随机抽取K个元素（可重复），以列表形式存在。weights是相对权重列表，cum_weights是累计权重，两个参数不能同时存在。 |
| lst = [1,2,3]random.shuffle(lst)print(lst)                   | 随机打乱序列x内元素的排列顺序。只能针对可变的序列。          |
| random.sample(population,k)                                  | 从population样本或集合中随机抽取K个不重复的元素形成新的序列。常用于不重复的随机抽样。返回的是一个新的序列，不会破坏原有序列。要从一个整数区间随机抽取一定数量的整数，请使用sample(range(10000000), k=60)类似的方法，这非常有效和节省空间。如果k大于population的长度，则弹出ValueError异常。 |
| random.random()                                              | 返回一个介于左闭右开[0.0, 1.0)区间的浮点数                   |
| random.uniform(a, b)                                         | 返回一个介于a和b之间的浮点数。如果a>b，则是b到a之间的浮点数。这里的a和b都有可能出现在结果中。 |
| random.triangular(low, high, mode)                           | 返回一个low <= N <=high的三角形分布的随机数。参数mode指明众数出现位置。 |
| random.betavariate(alpha, beta)                              | β分布。返回的结果在0~1之间，alpha和beta都要大于0             |
| random.expovariate(lambd)                                    | 指数分布                                                     |
| random.gammavariate(alpha, beta)                             | 伽玛分布，alpha和beta都要大于0                               |
| random.gauss(mu, sigma)                                      | 高斯分布                                                     |
| random.lognormvariate(mu, sigma)                             | 对数正态分布                                                 |
| random.normalvariate(mu, sigma)                              | 正态分布                                                     |
| random.vonmisesvariate(mu, kappa)                            | 卡帕分布                                                     |
| random.paretovariate(alpha)                                  | 帕累托分布                                                   |
| random.weibullvariate(alpha, beta)                           | 威布尔分布                                                   |

```python
import random

"""
'betavariate', 'choice', 'expovariate', 'gammavariate', 'gauss', 'getrandbits', 
'getstate', 'lognormvariate', 'normalvariate', 'paretovariat
e', 'randint', 'random', 'randrange', 'sample', 'seed', 'setstate', 
'shuffle', 'triangular', 'uniform', 'vonmisesvariate', 'weibullvariate'
"""

# random.seed(a=None, version=2)  # 初始化伪随机数生成器。如果未提供a或者a=None，则使用系统时间为种子。如果a是一个整数，则作为种子。
# print(random.seed( version=2))

# random.getstate()  # 返回一个当前生成器的内部状态的对象
# state = random.getstate()
# print(state, type(state))
# random.setstate(state)  # 传入一个先前利用getstate方法获得的状态对象，使得生成器恢复到这个状态。
# state = random.getstate()
# print(state, type(state))

# random.getrandbits(k)  # 返回range(0,2**k)之间的一个整数，相当于randrange(0,2**k)
# print(random.getrandbits(3))
# print(random.randrange(0,2**3))
# random.randrange(stop)  # 返回range(0,stop)之间的一个整数

# random.randrange(start, stop[, step])  # 返回range[start,stop)之间的一个整数，可加step，跟range(0,10,2)类似
#
# random.randint(a, b)  # 返回range[a,b]之间的一个整数，等价于range(a,b+1)
#
# random.choice(seq)  # 从非空序列seq中随机选取一个元素。如果seq为空则弹出 IndexError异常。
#
# random.choices(population, weights=None, *, cum_weights=None,k=1)  # 3.6版本新增。从population集群中随机抽取K个元素（可重复）。weights是相对权重列表，cum_weights是累计权重，两个参数不能同时存在。
# print(random.choices((1,2,3,4), weights=None, cum_weights=None,k=2))


# random.shuffle(x[, random])  # 随机打乱序列x内元素的排列顺序。只能针对可变的序列，对于不可变序列，请使用下面的sample()方法。
lst = [1,2,3]
random.shuffle(lst)
print(lst)

# random.sample(population,k)  # 从population样本或集合中随机抽取K个不重复的元素形成新的序列。常用于不重复的随机抽样。返回的是一个新的序列，不会破坏原有序列。要从一个整数区间随机抽取一定数量的整数，请使用sample(range(10000000), k=60)类似的方法，这非常有效和节省空间。如果k大于population的长度，则弹出ValueError异常。
print(random.sample((1,2,3),2))
# random.random()  # 返回一个介于左闭右开[0.0, 1.0)区间的浮点数
#
# random.uniform(a, b)  # 返回一个介于a和b之间的浮点数。如果a>b，则是b到a之间的浮点数。这里的a和b都有可能出现在结果中。
#
# random.triangular(low, high, mode)  # 返回一个low <= N <=high的三角形分布的随机数。参数mode指明众数出现位置。
print(random.triangular(1, 5, 3))
# random.betavariate(alpha, beta)  # β分布。返回的结果在0~1之间
#
# random.expovariate(lambd)  # 指数分布
print(random.expovariate(2))
# random.gammavariate(alpha, beta)  # 伽玛分布
#
# random.gauss(mu, sigma)  # 高斯分布
#
# random.lognormvariate(mu, sigma)  # 对数正态分布
#
# random.normalvariate(mu, sigma)  # 正态分布
#
# random.vonmisesvariate(mu, kappa)  # 卡帕分布
#
# random.paretovariate(alpha)  # 帕累托分布
#
# random.weibullvariate(alpha, beta)  # 威布尔分布

```

