RecyclerView简单使用
##前言
RecyclerView出现了这么多年了，每一次使用都要考虑怎样用。这真的不能容忍了。
##任务
自己动手实战一把，注意下面的某些句子是摘抄的。。。
##介绍
RecyclerView是support-v7包中的新组件，是一个强大的滑动组件，与经典的ListView相比，同样拥有item回收复用的功能，这一点从它的名字Recyclerview即回收view也可以看出。
优点：
- RecyclerView封装了viewholder的回收复用，也就是说RecyclerView标准化了ViewHolder，编写Adapter面向的是ViewHolder而不再是View了，复用的逻辑被封装了，写起来更加简单。
- 提供了一种插拔式的体验，高度的解耦，异常的灵活，针对一个Item的显示RecyclerView专门抽取出了相应的类，来控制Item的显示，使其的扩展性非常强。
- 可以控制Item增删的动画，可以通过ItemAnimator这个类进行控制，当然针对增删的动画，RecyclerView有其自己默认的实现。
- Adapter：使用RecyclerView之前，你需要一个继承自RecyclerView.Adapter的适配器，作用是将数据与每一个item的界面进行绑定。
- LayoutManager：用来确定每一个item如何进行排列摆放，何时展示和隐藏。回收或重用一个View的时候，LayoutManager会向适配器请求新的数据来替换旧的数据，这种机制避免了创建过多的View和频繁的调用findViewById方法（与ListView原理类似）。

##简单的RecyclerView使用方法
####1.添加依赖
在AS的build.gradle中添加依赖，然后同步一下就可以引入依赖包：
```groovy
dependencies {
	...
    compile 'com.android.support:recyclerview-v7:26.+'
}
```
####2.布局文件
添加完依赖之后，就开始写代码了，与ListView用法类似，也是先在xml布局文件中创建一个RecyclerView的布局：
```groovy
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".baseUse.RecyclerActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

    </android.support.v7.widget.RecyclerView>

</RelativeLayout>
```
####2.创建完布局之后在MainActivity中获取这个RecyclerView，并声明LayoutManager与Adapter，代码如下：
```groovy
        mRecyclerView = findViewById(R.id.recycler_view);
        //创建默认的线性LayoutManager
        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this);
        linearLayoutManager.setOrientation(LinearLayoutManager.VERTICAL);
        mRecyclerView.setLayoutManager(linearLayoutManager);
        //创建并设置Adapter
        mRecyclerView.setAdapter(new MyAdapter(myDatas));
```
####3.Adapter的创建问题，是要继承RecyclerView.Adapter这个类的。代码如下：
```groovy

    public class MyAdapter extends RecyclerView.Adapter {
        public String[] datas = null;

        public MyAdapter(String[] datas) {
            this.datas = datas;
        }

        @Override
        public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_recycler, parent, false);
            ViewHolder vh = new ViewHolder(view);
            return vh;
        }

        @Override
        public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
            if (holder instanceof ViewHolder) {
                ((ViewHolder) holder).mTextView.setText(datas[position]);
            }
        }

        @Override
        public int getItemCount() {
            return datas.length;
        }


        //自定义的ViewHolder，持有每个Item的的所有界面元素
        class ViewHolder extends RecyclerView.ViewHolder {
            public TextView mTextView;

            public ViewHolder(View view) {
                super(view);
                mTextView = view.findViewById(R.id.tv_recycler);
            }
        }

    }
```
主要的就是3个方法：
**a) onCreateViewHolder()**
这个方法主要生成为每个Item inflater出一个View，但是该方法返回的是一个ViewHolder。该方法把View直接封装在ViewHolder中，然后我们面向的是ViewHolder这个实例，当然这个ViewHolder需要我们自己去编写。直接省去了当初的convertView.setTag(holder)和convertView.getTag()这些繁琐的步骤。

**b) onBindViewHolder()**
这个方法主要用于适配渲染数据到View中。方法提供给你了一viewHolder而不是原来的convertView。

**c) getItemCount()**
这个方法就类似于BaseAdapter的getCount方法了，即总共有多少个条目。
####4.运行
写完这些代码这个例子既可以跑起来了
##总结
本节介绍的是一个最最简单的RecyclerView的使用方法，后面将介绍一些更高级的用法。小伙伴们都会用了吧！