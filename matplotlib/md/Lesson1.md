
### change label font & plot line thickness


```python
import matplotlib.pyplot as plt 
%matplotlib inline
```


```python
squares = [1,4,9,16,25]
plt.plot(squares, linewidth =5)  # plt.plot(x,y) 画出x,y轴的值
plt.title('Square Numbers',fontsize=24)
plt.xlabel('Value',fontsize=14)
plt.ylabel('Square of Value',fontsize=14)
plt.tick_params(axis='both',labelsize=14)  #设置刻度标记的大小
plt.show()
```


![png](output_2_0.png)



```python
# 画出x轴，y轴
x_values = [1,2,3,4,5]
y_values = [1,4,9,16,25]
plt.scatter(x_values,y_values,s=100)
plt.axis([0,8,0,30])
```




    [0, 8, 0, 30]




![png](output_3_1.png)



```python
plt.savefig('squares_plot.png',bbox_inches='tight')
```

### RandomWalk( ) 类 


```python
from random import choice

class RandomWalk():
    
    def __init__(self, num_points=5000):
        self.num_points = num_points
        
        self.x_values=[0]
        self.y_values=[0]
```
