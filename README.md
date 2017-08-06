# LoadingDetail
   ### 1、由于这里为两个ScrollView设置了OnTouchListener，所以在其他地方不能再设置了，否则就白搭了。

   ### 2、两个ScrollView的layout参数统一由mMoveLen决定。

   ### 3、变量mEvents有两个作用：
    一是防止手动滑到底部或顶部时继续滑动而改变布局，必须再次按下才能继续滑动；
    二是在新的pointer down或up时把mEvents设置成-1可以舍弃将要到来的第一个move事件，防止mMoveLen出现剧变。
   ### 为什么会出现剧变呢？
    因为假设一开始只有一只手指在滑动，记录的坐标值是这个pointer的事件坐标点，这时候另一只手指按下了导致事件又多了一个pointer，
    这时候到来的move事件的坐标可能就变成了新的pointer的坐标，这时计算与上一次坐标的差值就会出现剧变，变化的距离就是两个pointer间的距离。
    所以要把这个move事件舍弃掉，让mLastY值记录这个pointer的坐标再开始计算mMoveLen。pointer up的时候也一样。
