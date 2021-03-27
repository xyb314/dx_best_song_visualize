# 舞萌DX Best25 + Best15 可视化程序

*哎呀就瞎JB写写啦，代码写成一坨人家也没有办法啦~*



## 先介绍一下要准备哪些文件

mai文件夹里有三个文件夹：

- cover：放封面的。文件命名格式是`f'{str(int(idNum))}.png'`，也就是前面不要补0。图片大小都是400*400，因为我缩放是按倍数缩放的。如果图片大小不一致，要按像素缩放的话，麻烦自己改写代码啦~
- pic：放了其他图片素材。
- wmbest：放生成的图片的。



## 然后介绍一下`best_song_visualize.py`里有啥

我主要是通过`PIL`库来作图的（所以好烦呜呜）。代码里有三个类和一个计算单曲定数函数`computeRa`。

- `ChartInfo`类：用来存放“某首歌某难度谱面成绩”的各种信息。做成类完全是因为方便用单曲rating定义大小来进行排序。

  构造函数中参数解释：

参数名|类型|解释
:-:|:-:|:-:
idNum|str|歌曲编号。要用此编号索cover文件夹的图的。
diff|int|难度的索引。0~4分别代表绿黄红紫白谱。
tp|str|类别，填'SD'或'DX'。（但写到后面发现这个没用上ww）
achievement|float|达成率。
ra|int|单曲rating。用`computeRa`计算。
comboId|int|连击信息的索引。0~4分别代表无、FC、FC+、AP、AP+
scoreId|int|评价的索引。0~13分别代表D、C、B、BB、BBB、A、AA、AAA、S、S+、SS、SS+、SSS、SSS+
title|str|歌名
ds|float|谱面定数
lv|str|谱面难度（其实后面也没用上）

  此类利用`ra`重写了`__eq__`和`__lt__`方法，来方便地利用定数排序~

- `BestList`类：用来存放若干个`ChartInfo`对象的容器。构造函数传入`size`定义其容量。

  可以用`push(elem)`方法来向此容器添加`ChartInfo`对象。当加入的个数大于`size`时，将会舍弃掉一些按单曲rating排序最低的歌曲，保证元素个数不会大于`size`。

  在下面的示例代码中，我构造了两个此容器：`sdBest = BestList(25)`和`dxBest = BestList(15)`来代表SD谱和DX谱的前25首和前15首单曲rating最高的歌曲。

  *注意：因为存在单曲rating相同的情况，所以最终留在这类容器中的歌曲与机台显示的不一定相同，排序结果仅供参考。*

- `DrawBest`类：用来制图的类。

  构造函数中参数解释：

参数名|类型|解释
:-:|:-:|:-:
sdBest|BestList|表示SD前25首单曲rating最高的歌曲的`BestList`类容器
dxBest|BestList|表示DX前15首单曲rating最高的歌曲的`BestList`类容器
userName|str|玩家名
playerRating|int|玩家Rating
musicRating|int|底分

在调用构造函数时，程序便开始绘图并保存在内存中。调用`getDir()`方法保存图片并返回图片路径。顺便一提，该方法构造图片名的方法是对内存中的`self`对象进行哈希作为文件名。



## main函数示例代码

```python
if __name__ == '__main__':
    sdBest = BestList(25)
    dxBest = BestList(15)
    ShuaiCongGe = ChartInfo('749', 3, 'SD', 100.4999, 114514, 4, 0, '水魚玩葱', 0.0, 0)
    sdBest.push(ShuaiCongGe)
    pic = DrawBest(sdBest, dxBest, 'Chiyuki', 10302, 8202).getDir()
    Image.open(pic).show()
```

结果：

<img src=".\mai\wmbest\002A6B19B485.png" alt="002A6B19B485" style="zoom:100%;" />