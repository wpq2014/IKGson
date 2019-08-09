### 该项目gson module是Java工程，改完代码打新的jar就可以引用了。我是用Sublime直接编辑了几个类，都改完切到项目根目录执行`mvn clean package`会在
### gson文件夹下生成target文件夹，里面有新的jar。

### 1. 说明
> 改造后的Gson库，在非Debug模式下对常用的数据类型进行容错处理（某一个字段由于数据类型不匹配，采用默认值处理）

- 集成方式
> 直接下载使用项目工程中IKGson.jar文件即可

- 开启容错模式
```
 if (BuildConfig.DEBUG)
 {
    Gson.StrictMode = true;
 }
```

### 2. 容错模式下常用类型采用的默认值

类型 | 默认值
:---:|:---:
byte | 0
short | 0
int |0
long |0
float | 0.0
double | 0.0
chat | '0'
String | ""
Boolean |false
Array(数组) | {} 空数据，非null
List(集合) | []  空集合，非null

---

### 3. 测试的Json数据：

```
  "gifts":
  {
    "name": "xxx",
    "gold":20,
    "cl":[1,2,3],
    "gid":154,
    "c":100,
    "mshort":45,
    "mfloat":15,
    "mdouble":1,
    "boolean":true,
    "mchar":'c'
  }
```

### 4. JavaBean对象类

```
/**
 * Created by xxx on 2017/8/21.
 */
public class Gift {

    public String name;
    public int gold;
    public int[] cl;
    public long gid;
    public byte c;
    public short mshort;
    public float mfloat;
    public double mdouble;
    public boolean mboolean;
    public char mchar;
}
```


---

### 5. 各类型异常数据测试：


#### > 1. Byte类型的测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c=10, c=0
双引号数据 | "10", "asd"| c=10, c= 0
裸露数据 | asd, [], {} |  c = 0
空类型数据 | null， "null", 'null'| c = 0
大数数据 | 1000000000000000 |c = 0

#### > 2. Int类型的测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c=10, c=0
双引号数据 | "10", "asd"| c=10, c= 0
裸露数据 | asd, [], {} |  c = 0
空类型数据 | null， "null", 'null'| c = 0
大数数据 | 1000000000000000 |c = -1530494976

#### > 3. Short类型的测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c=10, c=0
双引号数据 | "10", "asd"| c=10, c= 0
裸露数据 | asd, [], {} |  c = 0
空类型数据 | null， "null", 'null'| c = 0
大数数据 | 1000000000000000 |c = -32768

#### > 4. Long类型的测试：

测试类型 | 测试用例 | 测试结果
---|---|---
单引号数据 | '10'，'asd'| c=10, c=0
双引号数据 | "10", "asd"| c=10, c= 0
裸露数据 | asd, [], {} |  c = 0
空类型数据 | null， "null", 'null'| c = 0
大数数据 | 1000000000000000 |c = 1000000000000000

#### > 5. Float类型的测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c=10.0, c=0.0
双引号数据 | "10", "asd"| c=10.0, c= 0.0
裸露数据 | asd, [], {} |  c = 0.0
空类型数据 | null， "null", 'null'| c = 0.0
大数数据 | 1000000000000000 |c = 9.9999999E14

#### > 6. Double类型的数据测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c=10.0, c=0.0
双引号数据 | "10", "asd"| c=10.0, c= 0.0
裸露数据 | asd, [], {} |  c = 0.0
空类型数据 | null， "null", 'null'| c = 0.0
大数数据 | 1000000000000000 |c = 1.0E15

#### > 7. Char类型的数据测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c='1', c='a'
双引号数据 | "10", "asd"| c= '1', c= 'a'
裸露数据 | asd, [], {} |  c = 'a', c = '0',c = '0'
空类型数据 | null， "null", 'null'| c = , c= 'n', c = 'n'
大数数据 | 1000000000000000 |c = 1

#### > 8. String类型的数据测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c='10', c='asd'
双引号数据 | "10", "asd"| c= '10', c= 'asd'
裸露数据 | asd, [], {} |  c = 'asd', c = '',c = ''
空类型数据 | null， "null", 'null'| c = null, c= 'null', c = 'null'
大数数据 | 1000000000000000 |c = '1000000000000000'

#### > 9. Boolean类型的数据测试：

测试类型 | 测试用例| 测试结果
---|---|---
单引号数据 | '10'，'asd'| c= false
双引号数据 | "10", "asd"| c= false
裸露数据 | asd, [], {} |  c = false
空类型数据 | null， "null", 'null'| c = false
大数数据 | 1000000000000000 |c = false

#### > 10. 数组类型的数据测试：


测试类型 | 测试用例 | 测试结果
---|---|---
 单引号数据| "asd" | c =[]
双引号数 | '10' | c = []
裸露数据| 1 | c= []
空数据| null | c= null

#### > 11. 集合类型的数据测试：

测试类型 | 测试用例 | 测试结果
---|---|---
 单引号数据| "asd" | c = []
双引号数 | '10' | c = []
裸露数据| 1 | c= []
空数据| null | c= null

















