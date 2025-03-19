1. 下载一个可以显示中文的字体:
```linux
sudo apt-get install ttf-wqy-zenhei
```

2. 确认ubuntu系统环境下拥有的中文字体文件及其路径：
```linux
fc-list :lang=zh
```

3. 查看matplotlib库的字体存储位置
```python
import matplotlib
print(matplotlib.matplotlib_fname())
```

4. 找到font下的文件夹，将所下载的字体复制到该文件夹中
清理字体缓存：
```linux
cd ~/.cache/matplotlib
rm * -rf
```

5. 需要注意的是ubuntu中的文件显示往往带有缩写，需要打开属性查看全名称
同时若后缀为.ttc,应改为.ttf
如显示wqy-zenhei，无法直接使用，打开属性后发现全名为WenQuanYi Zen Hei

6. 最后测试一下
```python
import matplotlib.pyplot as plt
```
&emsp;&emsp;正常显示中文字体与负号
```python
plt.rcParams['font.family'] = 'WenQuanYi Zen Hei'
plt.rcParams['axes.unicode_minus'] = False
```