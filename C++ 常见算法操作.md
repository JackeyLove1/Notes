# C++ 常见算法操作

## Vectorc

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool isOdd(int i) { return ((i % 2) == 1); }

template<typename T>
void print(vector<T> &vec) {
    for (T t : vec)
        cout << t << " ";
    cout << endl;
}

void printn(int i) { cout << i << " "; }

bool cmp(int a, int b){
    return a > b;
}

int main() {
    // vector
    int myints[] = {1, 2, 3, 4, 5, 6, 7};
vector<int> myvec;
// 容器赋值1
myvec.assign(10,1);
cout<<"after assigning is ";
print(myvec);
myvec.clear();
// 容器赋值2
for (int i = 0; i < 7; i++) {
    myvec.push_back(myints[i]);
}
print(myvec);

// 容器计数count_if
int mycount;
mycount = (int) count_if(myvec.begin(), myvec.end(), isOdd);
cout << "the number of odd in vec is " << mycount << endl;

// fill
fill(myvec.begin(), myvec.end(), 1);
print(myvec);

// find
// 未找到返回尾指针
// 找到返回该位指针的位置
auto it = find(myvec.begin(), myvec.end(), 10);
if (it == myvec.end()) {
    cout << "10 not find in myvec";
}

// find_if 根据条件查找元素
// find_if( begin(), end(), Pred)
it = find_if(myvec.begin(), myvec.end(), isOdd);
if (it != myvec.end()) {
    cout << "the first odd is " << *it << endl;
}

// for_each 对于每个元素都执行的操作
// for_each( iterator first, iterator last, function fn)
cout << "printn execute" << endl;
for_each(myvec.begin(), myvec.end(), printn);
cout << endl;

// 清空容器
myvec.clear();
if (myvec.empty()) {
    cout << "myvec is empty!" << endl;
}

vector<int> temp1 = {1, 2, 3, 4, 5};
auto it1 = temp1.begin();
for (; it1 != temp1.end();) {
    it1 = temp1.erase(it1);
}
if (temp1.empty()) {
    cout << "temp1 is empty " << endl;
}

// make_heap 创建堆
for (int i = 0; i < 7; i++) {
    myvec.push_back(i);
}
make_heap(myvec.begin(), myvec.end());
cout << "Init the heap : " << myvec.front() << endl;

// 弹出heap首元素
pop_heap(myvec.begin(), myvec.end());
myvec.pop_back();
cout << "max heap after pop " << myvec.front() << endl;

// 添加元素
myvec.push_back(100);
push_heap(myvec.begin(), myvec.end());
cout << "max heap after push " << myvec.front() << endl;

// 获取容器中的最大的元素
cout << "max number in myvec is " << *max_element(myvec.begin(), myvec.end()) << endl;
// 获取几个数字之中的最大数字
cout << "max of 1 and 2 is " << max(1, 2) << endl;

// 给容器排序
sort(myvec.begin(), myvec.end());
cout<<"after sorting is ";
print(myvec);

// 自定义排序函数cmp
// sort(first, last, cmp)
sort(myvec.begin(), myvec.end(), cmp);
cout<<"after cmp sorting ";
print(myvec);、
}
```



## map

```c++
#include <iostream>
#include <algorithm>
#include <map>

using namespace std;

int main() {
    // 定义map
    map<int, string> person;

    // 插入元素 insert
    person.insert(pair<int, string>(0, "student0"));

    person.insert(map<int, string>::value_type(1, "student1"));

    person[3] = "student3";

    // 查找元素 find
    auto iter = person.find(1);
    if (iter != person.end()) {
        cout << "find 1 is " << person[1] << endl;
    }

    // 迭代器删除
    person.erase(iter);
    if (person.find(1) == person.end()) {
        cout << "erase success !" << endl;
    }

    // 清空map, erase or clear
    // person.clear()
    person.erase(person.begin(), person.end());
    if (person.empty()) {
        cout << "person is deleted !" << endl;
    }

    // map大小
    int n = person.size();
    cout << "size of person is " << n << endl;
}
```

