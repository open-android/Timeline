* 欢迎关注微信公众号、长期为您推荐优秀博文、开源项目、视频

* 微信公众号名称：Android干货程序员

* ![](http://upload-images.jianshu.io/upload_images/4037105-8f737b5104dd0b5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Timeline-View 

Android Timeline View Library (使用 RecyclerView) is simple implementation used to display view like Tracking of shipment/order, steppers etc.


![showcase](https://github.com/vipulasri/Timeline-View/blob/master/art/showcase.png)



### 使用步骤

#### 1. 在project的build.gradle添加如下代码(如下图)
```
	allprojects {
	    repositories {
	        ...
	        maven { url "https://jitpack.io" }
	    }
	}
```
![](http://upload-images.jianshu.io/upload_images/4037105-e7d8b1d836d6cb46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	

	
#### 2. 在Module的build.gradle添加依赖
```
     compile 'com.github.open-android:Timeline:0.1.0'
```





#### 3. Usage
 * In XML Layout :

``` java
<com.github.vipulasri.timelineview.TimelineView
    android:id="@+id/time_marker"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    app:markerSize="20dp"
    app:lineSize="2dp"
    app:line="@color/colorPrimary"/>
```

##### Line Padding around marker

``` java
<com.github.vipulasri.timelineview.TimelineView
    android:id="@+id/time_marker"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    app:markerSize="20dp"
    app:lineSize="2dp"
    app:line="@color/colorPrimary"
    app:linePadding="5dp"/>
```
* Configure using xml attributes or setters in code:

    <table>
    <th>Attribute Name</th>
    <th>Default Value</th>
    <th>Description</th>
    <tr>
        <td>app:marker="@drawable/marker"</td>
        <td>Green Colored Oval Drawable</td>
        <td>sets marker drawable</td>
    </tr>
    <tr>
        <td>app:markerSize="25dp"</td>
        <td>25dp</td>
        <td>sets marker size</td>
    </tr>
    <tr>
        <td>app:markerInCenter="false"</td>
        <td>true</td>
        <td>sets the marker in center of line if `true`</td>
    </tr>
    <tr>
        <td>app:line="@color/primarColor"</td>
        <td>Dark Grey Line</td>
        <td>sets line color</td>
    </tr>
     <tr>
        <td>app:lineSize="2dp"</td>
        <td>2dp</td>
        <td>sets line width</td>
    </tr>
    <tr>
        <td>app:lineOrientation="horizontal"</td>
        <td>vertical</td>
        <td>sets orientation of line ie `horizontal` or `vertical`</td>
    </tr>
    <tr>
        <td>app:linePadding="5dp"</td>
        <td>0dp</td>
        <td>sets line padding around marker</td>
        </tr>
    </table>
 


#### 实现上面图片一的效果，拷贝如下XML到RecyclerView的Item布局,如下是完整的item布局
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginRight="10dp">

    <com.github.vipulasri.timelineview.TimelineView
        android:id="@+id/time_marker"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        app:markerSize="20dp"
        app:lineSize="3dp"
        app:line="@color/colorPrimary"
        app:linePadding="5dp"/>

    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="15dp"
        android:layout_marginBottom="15dp"
        android:layout_marginLeft="10dp"
        android:layout_gravity="center_vertical"
        app:cardElevation="1dp"
        app:contentPadding="15dp">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:gravity="center"
            android:orientation="vertical">

            <android.support.v7.widget.AppCompatTextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/text_timeline_date"
                android:textSize="12sp"
                tools:text="24 JAN"/>

            <android.support.v7.widget.AppCompatTextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="5dp"
                android:id="@+id/text_timeline_title"
                android:textColor="@android:color/black"
                tools:text="Order Successfully Completed"/>

        </LinearLayout>

    </android.support.v7.widget.CardView>

</LinearLayout>
```
#### RecyclerView的Adapter完整代码如下：
```
public class TimeLineAdapter extends RecyclerView.Adapter<TimeLineViewHolder> {

    private List<TimeLineModel> mFeedList;
    private Context mContext;
    private Orientation mOrientation;
    private boolean mWithLinePadding;
    private LayoutInflater mLayoutInflater;

    public TimeLineAdapter(List<TimeLineModel> feedList, Orientation orientation, boolean withLinePadding) {
        mFeedList = feedList;
        mOrientation = orientation;
        mWithLinePadding = withLinePadding;
    }

    @Override
    public int getItemViewType(int position) {
        return TimelineView.getTimeLineViewType(position,getItemCount());
    }

    @Override
    public TimeLineViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        mContext = parent.getContext();
        mLayoutInflater = LayoutInflater.from(mContext);
        View view;

        if(mOrientation == Orientation.HORIZONTAL) {
            view = mLayoutInflater.inflate(mWithLinePadding ? R.layout.item_timeline_horizontal_line_padding : R.layout.item_timeline_horizontal, parent, false);
        } else {
            view = mLayoutInflater.inflate(mWithLinePadding ? R.layout.item_timeline_line_padding : R.layout.item_timeline, parent, false);
        }

        return new TimeLineViewHolder(view, viewType);
    }

    @Override
    public void onBindViewHolder(TimeLineViewHolder holder, int position) {

        TimeLineModel timeLineModel = mFeedList.get(position);

        if(timeLineModel.getStatus() == OrderStatus.INACTIVE) {
            holder.mTimelineView.setMarker(VectorDrawableUtils.getDrawable(mContext, R.drawable.ic_marker_inactive, android.R.color.darker_gray));
        } else if(timeLineModel.getStatus() == OrderStatus.ACTIVE) {
            holder.mTimelineView.setMarker(VectorDrawableUtils.getDrawable(mContext, R.drawable.ic_marker_active, R.color.colorPrimary));
        } else {
            holder.mTimelineView.setMarker(ContextCompat.getDrawable(mContext, R.drawable.ic_marker), ContextCompat.getColor(mContext, R.color.colorPrimary));
        }

        if(!timeLineModel.getDate().isEmpty()) {
            holder.mDate.setVisibility(View.VISIBLE);
            holder.mDate.setText(DateTimeUtils.parseDateTime(timeLineModel.getDate(), "yyyy-MM-dd HH:mm", "hh:mm a, dd-MMM-yyyy"));
        }
        else
            holder.mDate.setVisibility(View.GONE);

        holder.mMessage.setText(timeLineModel.getMessage());
    }

    @Override
    public int getItemCount() {
        return (mFeedList!=null? mFeedList.size():0);
    }

}
```
#### 上面RecyclerView注意左边时间轴图片，一共是三种状态，空心圆圈，实心圆圈，圆圈里面有一个点，三种状态如下：
```
public enum OrderStatus {

    COMPLETED,
    ACTIVE,
    INACTIVE;

}
```
### 下面的代码都属于核心代码，如果看完上面的完整代码，下面的代码可以忽略

* RecyclerView Holder : 
   Your `RecyclerViewHolder` should have an extra paramenter in constructor i.e viewType from `onCreateViewHolder`. You would also have to call the method `initLine(viewType)` in constructor definition.
 
``` java

    public class TimeLineViewHolder extends RecyclerView.ViewHolder {
        public  TimelineView mTimelineView;

        public TimeLineViewHolder(View itemView, int viewType) {
            super(itemView);
            mTimelineView = (TimelineView) itemView.findViewById(R.id.time_marker);
            mTimelineView.initLine(viewType);
        }
    }

```

* RecyclerView Adapter : 
   override `getItemViewType` method in Adapter
 
``` java

    @Override
    public int getItemViewType(int position) {
        return TimelineView.getTimeLineViewType(position,getItemCount());
    }

```
   And pass the `viewType` from `onCreateViewHolder` to its Holder.
   
``` java

    @Override
    public TimeLineViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = View.inflate(parent.getContext(), R.layout.item_timeline, null);
        return new TimeLineViewHolder(view, viewType);
    }

```
