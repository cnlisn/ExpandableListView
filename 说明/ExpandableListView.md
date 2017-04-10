 ExpandableListView组件是android中一个比较常用的组件，当点击一个父item的时候可以将它的子item显示出 来，像手机QQ中的好友列表就是实现的类型效果。使用ExpandableListView组件的关键就是设置它的adapter，这个adapter必 须继承BaseExpandbaleListAdapter类，所以实现运用ExpandableListView的核心就是学会继承这个 BaseExpanableListAdapter类。

 下面是一个小demo：

activvity_main.xml:


<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity" >
 
    <ExpandableListView
        android:id="@+id/main_expandablelistview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
 
</RelativeLayout>


layout_parent.xml:(父item运用的样式)


<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
 
    <TextView
        android:id="@+id/parent_textview"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:layout_gravity="center"
        android:gravity="center" />
 
</LinearLayout>


   layout_children.xml:(子item运用的样式)


<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
 
    <TextView
        android:id="@+id/second_textview"
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:layout_gravity="center"
        android:gravity="center" />
 
</LinearLayout>
MainActivity.java:

package com.example.android_expandablelistview;
 
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
 
import android.app.Activity;
import android.content.Context;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseExpandableListAdapter;
import android.widget.ExpandableListView;
import android.widget.TextView;
 
public class MainActivity extends Activity {
 
    ExpandableListView mainlistview = null;
    List<String> parent = null;
    Map<String, List<String>> map = null;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mainlistview = (ExpandableListView) this
                .findViewById(R.id.main_expandablelistview);
 
        initData();
        mainlistview.setAdapter(new MyAdapter());
    }
 
    // 初始化数据
    public void initData() {
        parent = new ArrayList<String>();
        parent.add("parent1");
        parent.add("parent2");
        parent.add("parent3");
 
        map = new HashMap<String, List<String>>();
 
        List<String> list1 = new ArrayList<String>();
        list1.add("child1-1");
        list1.add("child1-2");
        list1.add("child1-3");
        map.put("parent1", list1);
 
        List<String> list2 = new ArrayList<String>();
        list2.add("child2-1");
        list2.add("child2-2");
        list2.add("child2-3");
        map.put("parent2", list2);
 
        List<String> list3 = new ArrayList<String>();
        list3.add("child3-1");
        list3.add("child3-2");
        list3.add("child3-3");
        map.put("parent3", list3);
 
    }
 
    class MyAdapter extends BaseExpandableListAdapter {
       
        //得到子item需要关联的数据
        @Override
        public Object getChild(int groupPosition, int childPosition) {
            String key = parent.get(groupPosition);
            return (map.get(key).get(childPosition));
        }
 
        //得到子item的ID
        @Override
        public long getChildId(int groupPosition, int childPosition) {
            return childPosition;
        }
 
        //设置子item的组件
        @Override
        public View getChildView(int groupPosition, int childPosition,
                boolean isLastChild, View convertView, ViewGroup parent) {
            String key = MainActivity.this.parent.get(groupPosition);
            String info = map.get(key).get(childPosition);
            if (convertView == null) {
                LayoutInflater inflater = (LayoutInflater) MainActivity.this
                        .getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                convertView = inflater.inflate(R.layout.layout_children, null);
            }
            TextView tv = (TextView) convertView
                    .findViewById(R.id.second_textview);
            tv.setText(info);
            return tv;
        }
 
        //获取当前父item下的子item的个数
        @Override
        public int getChildrenCount(int groupPosition) {
            String key = parent.get(groupPosition);
            int size=map.get(key).size();
            return size;
        }
      //获取当前父item的数据
        @Override
        public Object getGroup(int groupPosition) {
            return parent.get(groupPosition);
        }
 
        @Override
        public int getGroupCount() {
            return parent.size();
        }
 
        @Override
        public long getGroupId(int groupPosition) {
            return groupPosition;
        }
       //设置父item组件
        @Override
        public View getGroupView(int groupPosition, boolean isExpanded,
                View convertView, ViewGroup parent) {
            if (convertView == null) {
                LayoutInflater inflater = (LayoutInflater) MainActivity.this
                        .getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                convertView = inflater.inflate(R.layout.layout_parent, null);
            }
            TextView tv = (TextView) convertView
                    .findViewById(R.id.parent_textview);
            tv.setText(MainActivity.this.parent.get(groupPosition));
            return tv;
        }
 
        @Override
        public boolean hasStableIds() {
            return true;
        }
 
        @Override
        public boolean isChildSelectable(int groupPosition, int childPosition) {
            return true;
        }
 
    }
}


最后的实现结果：



