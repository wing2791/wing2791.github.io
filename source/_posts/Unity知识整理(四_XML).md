---
title: Unity知识整理(四_XML)
math: true
---
# XML基本语法
- XML格式是一种树形结构
![Pasted image 20241203201539.png](https://s2.loli.net/2024/12/05/PwWTGlbLso1axRt.png)

- 基本规则
1. 每个元素都必须有关闭标签
2. 元素命名规则基本遵照C#中变量名命名规则
3. XML标签对大小写敏感，name和Name不一样
4. XML文档必须有根元素
5. 特殊的符号应该用实体引用
	&lt        < 小于
	&gt       >大于
	&amp   &和号
	&apos  '单引号
	&quot  "双引号
```xml
<!-- 注释 -->
<!-- 固定内容代表xml的版本，使用的编码-->
<!-- 所谓的编码格式就是读取文件时,解析字符串使用的编码是什么编码格式,不同的字符在内存中的二进制是不一样的,每一个字符对应一个数字,不同的编码格式字符对应的二进制是不一样的 -->
<!-- 乱码出现的情况就是因为用了不一样的编码格式去解析文本内容由于字符和对应的二进制不匹配就会出现乱码 -->
<?xml version="1.0" encoding="UTF-8"?>
<!-- 一定要有一个根节点 -->
<!-- <节点名>可以填写数据，或者在其中包裹其他的节点</节点名> -->
<PlayerInfo>
    <name>wing2791</name>
    <atk>10</atk>
    <ItemList>
        <Item>
            <id>1</id>
            <num>1</num>
        </Item>
        <Item>
            <id>2</id>
            <num>10</num>
        </Item>
        <Item>
            <id>3</id>
            <num>20</num>
        </Item>
    </ItemList>
</PlayerInfo>
```

# XML属性知识点
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 一定要有一个根节点 -->
<!-- <节点名>可以填写数据，或者在其中包裹其他的节点</节点名> -->
<test>
    <!-- 属性就是在元素标签后面空格 添加的内容 -->
    <!-- 注意：属性必须用引号包裹单引号双引号都可以 -->
    <Friend name="小明">我的朋友</Friend>
    <!-- 如果使用属性记录信息，不想使用元素记录，可以下面这样写 -->
    <!-- 属性和元素节点只是写法上的区别而已 -->
    <Father name="父亲" age="50"/>
</test>
```

- 网址检查语法错误
<https://www.runoob.com/xml/xml-validator.html>


# Csharp读取存储XML
## XML文件存放位置
- TestXML.xml
```csharp
<?xml version="1.0" encoding="UTF-8"?>
<Root>
	<name>唐老狮</name>
	<age>18</age>
	<Item id="1" num="10"/>
	<Friend>
		<name>小明</name>
		<age>8</age>
	</Friend>
	<Friend>
		<name>小红</name>
		<age>10</age>
	</Friend>
</Root>
```

- 只读不写的XML文件
	- 可以放在Resources或者StreamingAssets文件夹下
		- Resources.Load\<TextAsset\>("文件名");
			- 加载的是.txt文件
		- Application.streamingAssetsPath
- 动态存储的XML文件
	- 放在Application.persistentDataPath路径下
![Pasted image 20241203231939.png](https://s2.loli.net/2024/12/05/LIxafOAU3KSXy1k.png)

## Csharp读取XML文件知识点

- 加载的文件是TestXML.xml
```csharp
<?xml version="1.0" encoding="UTF-8"?>
<Root>
	<name>唐老狮</name>
	<age>18</age>
	<Item id="1" num="10"/>
	<Friend>
		<name>小明</name>
		<age>8</age>
	</Friend>
	<Friend>
		<name>小红</name>
		<age>10</age>
	</Friend>
</Root>
```

- LoadXml.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using System.Xml;
using UnityEngine;

public class LoadXml : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        //C#读取XML的方法有几种
        //1.XmlDocument    (把数据加载到内存中，方便读取)
        //2.XmlTextReader  (以流形式加载，内存占用更少，但是是单向只读，使用不是特别方便，除非有特殊需求，否则不会使用)
        //3.Linq           (以后专门讲Linq的时候讲)

        //使用XmlDocument类读取是较方便最容易理解和操作的方法
        #region 知识点一 读取xml文件信息
        // using System.Xml;
        XmlDocument xml = new XmlDocument();
        //通过XmlDocument读取xml文件 有两个API
        //1.直接根据xml字符串内容 来加载xml文件
        //存放在Resorces文件夹下的xml文件加载处理
        TextAsset asset = Resources.Load<TextAsset>("TestXml");
        print(asset.text);
        //通过这个方法 就能够翻译字符串为xml对象
        xml.LoadXml(asset.text);

        //2.是通过xml文件的路径去进行加载
        xml.Load(Application.streamingAssetsPath + "/TestXml.xml");
        #endregion

        #region 知识点二 读取元素和属性信息
        //节点信息类
        //XmlNode 单个节点信息类
        //节点列表信息
        //XmlNodeList 多个节点信息类

        //获取xml当中的根节点
        XmlNode root = xml.SelectSingleNode("Root");
        //再通过根节点 去获取下面的子节点
        XmlNode nodeName = root.SelectSingleNode("name");
        //如果想要获取节点包裹的元素信息 直接 .InnerText
        print(nodeName.InnerText);

        // 如果有age同名节点，只会获取到第一个节点
        XmlNode nodeAge = root.SelectSingleNode("age");
        print(nodeAge.InnerText);

        XmlNode nodeItem = root.SelectSingleNode("Item");
        //第一种方式 直接 中括号获取信息
        print(nodeItem.Attributes["id"].Value);
        print(nodeItem.Attributes["num"].Value);
        //第二种方式
        print(nodeItem.Attributes.GetNamedItem("id").Value);
        print(nodeItem.Attributes.GetNamedItem("num").Value);

        //这里是获取 一个节点下的同名节点的方法
        XmlNodeList friendList = root.SelectNodes("Friend");

        //遍历方式一：迭代器遍历
        //foreach (XmlNode item in friendList)
        //{
        //    print(item.SelectSingleNode("name").InnerText);
        //    print(item.SelectSingleNode("age").InnerText);
        //}
        //遍历方式二：通过for循环遍历
        //通过XmlNodeList中的 成员变量 Count可以得到 节点数量
        for (int i = 0; i < friendList.Count; i++)
        {
            print(friendList[i].SelectSingleNode("name").InnerText);
            print(friendList[i].SelectSingleNode("age").InnerText);
        }
        #endregion

        #region 总结
        //1.读取XML文件
        //XmlDocument xml = new XmlDocument();
        //读取文本方式1-xml.LoadXml(传入xml文本字符串)
        //读取文本方式2-xml.Load(传入路径)

        //2.读取元素和属性
        //获取单个节点 : XmlNode node = xml.SelectSingleNode(节点名)
        //获取多个节点 : XmlNodeList nodeList = xml.SelectNodes(节点名)

        //获取节点元素内容：node.InnerText
        //获取节点元素属性：
        //1.item.Attributes["属性名"].Value
        //2.item.Attributes.GetNamedItem("属性名").Value

        //通过迭代器遍历或者循环遍历XmlNodeList对象 可以获取到各单个元素节点

        #endregion
    }
}
```

## Csharp存储XML文件知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml;
using UnityEngine;

public class SaveXml : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 决定存储在哪个文件夹下
        //注意：存储xml文件 在Unity中一定是使用各平台都可读可写可找到的路径
        // 1.Resources 可读 不可写 打包后找不到  ×
        // 2.Application.streamingAssetsPath 可读，PC端可写,ios和安卓端不可写，找得到  ×
        // 3.Application.dataPath 打包后找不到，只在测试的时候使用  ×
        // 4.Application.persistentDataPath 可读可写找得到   √

        string path = Application.persistentDataPath + "/PlayerInfo2.xml";
        print(Application.persistentDataPath);
        #endregion

        #region 知识点二 存储xml文件
        //关键类 XmlDocument 用于创建节点 存储文件
        //关键类 XmlDeclaration 用于添加版本信息
        //关键类 XmlElement 节点类

        //存储有5步
        //1.创建文本对象
        XmlDocument xml = new XmlDocument();

        //2.添加固定版本信息
        //这一句代码 相当于就是创建<?xml version="1.0" encoding="UTF-8"?>这句内容
        XmlDeclaration xmlDec = xml.CreateXmlDeclaration("1.0", "UTF-8", "");
        //创建完成过后 要添加进入 文本对象中
        xml.AppendChild(xmlDec);

        //3.添加根节点
        XmlElement root = xml.CreateElement("Root");
        xml.AppendChild(root);

        //4.为根节点添加子节点
        //加了一个 name子节点
        XmlElement name = xml.CreateElement("name");
        name.InnerText = "唐老狮";
        root.AppendChild(name);

        XmlElement atk = xml.CreateElement("atk");
        // 无论是int还是float，都需要转成string
        atk.InnerText = "10";
        root.AppendChild(atk);

        XmlElement listInt = xml.CreateElement("listInt");
        for (int i = 1; i <= 3; i++)
        {
            XmlElement childNode = xml.CreateElement("int");
            childNode.InnerText = i.ToString();
            listInt.AppendChild(childNode);
        }
        root.AppendChild(listInt);

        XmlElement itemList = xml.CreateElement("itemList");
        for (int i = 1; i <= 3; i++)
        {
            XmlElement childNode = xml.CreateElement("Item");
            //添加属性
            childNode.SetAttribute("id", i.ToString());
            childNode.SetAttribute("num", (i * 10).ToString());
            itemList.AppendChild(childNode);
        }
        root.AppendChild(itemList);

        //5.保存
        xml.Save(path);
        #endregion

        #region 知识点三 修改xml文件
        //1.先判断是否存在文件
        // File需要引入这个命名空间
        // using System.IO;
        if (File.Exists(path))
        {
            //2.加载后 直接添加节点 移除节点即可
            XmlDocument newXml = new XmlDocument();
            newXml.Load(path);

            //修改就是在原有文件基础上 去移除 或者添加
            //移除
            XmlNode node; // = newXml.SelectSingleNode("Root").SelectSingleNode("atk");
            //这种是一种简便写法 通过/来区分父子关系
            node = newXml.SelectSingleNode("Root/atk");
            //得到自己的父节点
            XmlNode root2 = newXml.SelectSingleNode("Root");
            //移除子节点方法
            root2.RemoveChild(node);

            //添加节点
            XmlElement speed = newXml.CreateElement("moveSpeed");
            speed.InnerText = "20";
            root2.AppendChild(speed);

            //改了记得存
            newXml.Save(path);
        }

        #endregion

        #region 总结
        //1.路径选取
        //在运行过程中存储 只能往可写且能找到的文件夹存储
        //故 选择了Application.persistentDataPath

        //2.存储xml关键类
        //XmlDocument  文件
        //   创建节点 CreateElement
        //   创建固定内容方法 CreateXmlDeclaration
        //   添加节点 AppendChild
        //   保存 Save
        //XmlDeclaration 版本
        //XmlElement 元素节点
        //   设置属性方法SetAttribute

        //3.修改
        //RemoveChild移除节点
        //可以通过 /的形式 来表示 子节点的子节点
        #endregion
    }
}
```

![Pasted image 20241203235048.png](https://s2.loli.net/2024/12/05/LTPDNz4f1M7GhxY.png)

# XML序列化知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml.Serialization;
using UnityEngine;

public class Lesson1Test
{
    [XmlElement("testPublic123123")]
    public int testPublic; //<testPublic123123>0</testPublic123123>

    private int testPrivate;
    protected int testProtected;
    internal int testInternal;

    public string testPUblicStr;

    public int testPro { get; set; }

    public Lesson1Test2 testClass = new Lesson1Test2();

    public int[] arrayInt;

    [XmlArray("IntList")]
    [XmlArrayItem("Int32")]
    public List<int> listInt; // <IntList> <Int32></Int32> </IntList>
    public List<Lesson1Test2> listItem;

    //不支持字典
    //public Dictionary<int, string> testDic = new Dictionary<int, string>() { { 1, "123" } };
}

public class Lesson1Test2
{
    //<Lesson1Test2 Test1="1" test2="1.1" test3="true" />
    [XmlAttribute("Test1")]
    public int test1 = 1;

    [XmlAttribute()]
    public float test2 = 1.1f;

    [XmlAttribute()]
    public bool test3 = true;
}

public class Lesson1 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 什么是序列化和反序列化
        //序列化：把对象转化为可传输的字节序列过程称为序列化
        //反序列化：把字节序列还原为对象的过程称为反序列化

        //说人话：
        //序列化就是把想要存储的内容转换为字节序列用于存储或传递
        //反序列化就是把存储或收到的字节序列信息解析读取出来使用
        #endregion

        #region 知识点二 xml序列化
        //1.第一步准备一个数据结构类
        Lesson1Test lt = new Lesson1Test();
        //2.进行序列化
        //  关键知识点
        //  XmlSerializer 用于序列化对象为xml的关键类
        //  StreamWriter 用于存储文件
        //  using 用于方便流对象释放和销毁

        //第一步：确定存储路径
        string path = Application.persistentDataPath + "/Lesson1Test.xml";
        print(Application.persistentDataPath);
        //第二步：结合 using知识点 和 StreamWriter这个流对象 来写入文件
        // 括号内的代码：写入一个文件流 如果有该文件 直接打开并修改 如果没有该文件 直接新建一个文件
        // using 的新用法 括号当中包裹的声明的对象 会在 大括号语句块结束后 自动释放掉
        // 当语句块结束 会自动帮助我们调用 对象的 Dispose这个方法 让其进行销毁
        // using一般都是配合 内存占用比较大 或者 有读写操作时  进行使用的
        using (StreamWriter stream = new StreamWriter(path))
        {
            //第三步：进行xml文件序列化
            //Lesson1Test lt = new Lesson1Test();

            XmlSerializer s = new XmlSerializer(typeof(Lesson1Test));
            //XmlSerializer s = new XmlSerializer(lt.GetType());
            //这句代码的含义 就是通过序列化对象 对我们类对象进行翻译 将其翻译成我们的xml文件 写入到对应的文件中
            //第一个参数 ： 文件流对象
            //第二个参数: 想要备翻译 的对象
            //注意 ：翻译机器的类型s 一定要和传入的对象lt 是一致的 不然会报错
            s.Serialize(stream, lt);
        }
        #endregion

        #region 知识点三 自定义节点名 或 设置属性
        //可以通过特性 设置节点或者设置属性 并且修改名字
        #endregion

        #region 总结
        //序列化流程
        //1.有一个想要保存的类对象
        //2.使用XmlSerializer 序列化该对象
        //3.通过StreamWriter 配合 using将数据存储 写入文件
        //注意：
        //1.只能序列化公共成员
        //2.不支持字典序列化
        //3.可以通过特性修改节点信息 或者设置属性信息
        //4.Stream相关要配合using使用
        #endregion
    }
}
```

# XML反序列化

```csharp
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml.Serialization;
using UnityEngine;

public class Lesson2 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识回顾
        // 序列化 就是把类对象 转换为可存储和传输的数据
        // 反序列化 就是把存储或收到的数据 转换为 类对象

        // xml序列化关键知识
        // 1.using 和 StreamWriter
        // 2.XmlSerializer 的 Serialize序列化方法
        #endregion

        #region 知识点一 判断文件是否存在
        string path = Application.persistentDataPath + "/Lesson1Test.xml";
        if (File.Exists(path))
        {
            #region 知识点二 反序列化
            //关键知识
            // 1.using 和 StreamReader
            // 2.XmlSerializer 的 Deserialize反序列化方法

            //读取文件
            using (StreamReader reader = new StreamReader(path))
            {
                //产生了一个 序列化反序列化的翻译机器
                XmlSerializer s = new XmlSerializer(typeof(Lesson1Test));
                // 这里有个坑，如果是List，会根据Lesson1Test里面的具体情况进行初始化
                // 然后再根据xml文件来对List进行Add，建议Lesson1Test里面不要初始化
                Lesson1Test lt = s.Deserialize(reader) as Lesson1Test;
            }
            #endregion
        }
        #endregion

        #region 总结
        //1.判断文件是否存在 File.Exists
        //2.文件流获取 StreamReader reader = new StreamReader(path)
        //3.根据文件流 XmlSerializer通过Deserialize反序列化 出对象

        //注意：List对象 如果有默认值 反序列化时 不会清空 会往后面添加
        #endregion
    }
}
```

# IXmlSerializable接口知识点
```csharp
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml;
using System.Xml.Schema;
using System.Xml.Serialization;
using UnityEngine;

public class TestLesson3 : IXmlSerializable
{
    public int test1 = 10;
    public int test2 = 99;

    // 这个方法暂时不用管，返回null就可以了
    public XmlSchema GetSchema()
    {
        return null;
    }

    // 反序列化时会自动调用的方法
    public void ReadXml(XmlReader reader)
    {
        // 自定义反序列化规则
        //读属性
        // reader["Test1"] 意思是读取根节点的Test1的属性，读取出来是String类型
        //test1 = int.Parse(reader["Test1"]);
        //test2 = int.Parse(reader["Test2"]);

        //读节点
        //方式一
        /*
         * <TestLesson3>
         *		<test1>0</test1>
         *		<test2>123</test2>
         * </TestLesson3>
         * */
        //reader.Read();//这时读到的是节点<test1>
        //reader.Read();//这时读到的才是值0
        //test1 = int.Parse(reader.Value);//得到值内容，解析这个值
        //reader.Read();//得到节点尾部配对</test1>
        //reader.Read();//读到节点开头<test2>
        //reader.Read();//读到值123
        //test2 = int.Parse(reader.Value);//获取值内容
        //方式二
        //while (reader.Read())
        //{
        //    if(reader.NodeType == XmlNodeType.Element)
        //    {
        //        switch (reader.Name)
        //        {
        //            case "Test1":
        //                reader.Read();
        //                test1 = int.Parse(reader.Value) ;
        //                break;
        //            case "Test2":
        //                reader.Read();
        //                test2 = int.Parse(reader.Value);
        //                break;
        //        }
        //    }
        //}

        //读包裹点
        /*
         * <TestLesson3>
         *		<test1>0</test1>
         *		<test2>123</test2>
         * </TestLesson3>
         * */
        XmlSerializer s = new XmlSerializer(typeof(int));
        reader.Read();
        // 检查当前节点是否是 <Test1> 元素的起始标签，并将光标移到标签内容（即 0）的位置
        reader.ReadStartElement("Test1");
        // 使用 XmlSerializer 将当前节点的内容反序列化为 int 类型，并赋值给变量 test1
        // 此时，reader 光标移动到<Test1> 元素结束标签的位置
        test1 = (int)s.Deserialize(reader);
        // 检查并读取 <Test1> 元素的结束标签，确保匹配了对应的 <Test1> 开始标签
        reader.ReadEndElement();
        reader.ReadStartElement("Test2");
        test1 = (int)s.Deserialize(reader);
        reader.ReadEndElement();
    }

    // 序列化时，会自动调用的方法
    public void WriteXml(XmlWriter writer)
    {
        // 自定义序列化规则
        //写属性，可以改为this.test1.ToString()
        // 则会个test1就是这个TestLesson3的里面的属性
        //writer.WriteAttributeString("Test1", test1.ToString());
        //writer.WriteAttributeString("Test2", test2.ToString());

        //写节点
        //writer.WriteElementString("Test1", test1.ToString());
        //writer.WriteElementString("Test2", test2.ToString());

        //写包裹节点
        /*
         * <TestLesson3>
         *		<test1>
         *			<int>0</int>
         *		</test1>
         *		<test2>
         *			<string>123</string>
         *		</test2>
         *	</TestLesson3>
         */
        XmlSerializer s = new XmlSerializer(typeof(int));
        writer.WriteStartElement("Test1");
        s.Serialize(writer, test1);
        writer.WriteEndElement();

        writer.WriteStartElement("Test2");
        s.Serialize(writer, test2);
        writer.WriteEndElement();
    }
}

public class Lesson3 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 IXmlSerializable是什么
        //C# 的XmlSerializer 提供了可拓展内容
        //可以让一些不能被序列化和反序列化的特殊类能被处理
        //让特殊类继承 IXmlSerializable 接口 实现其中的方法即可
        #endregion

        #region 知识点二 自定义类实践
        TestLesson3 t = new TestLesson3();
        using (StreamWriter writer = new StreamWriter(Application.persistentDataPath + "/test.xml"))
        {
            // 序列化的“翻译机器”
            XmlSerializer s = new XmlSerializer(typeof(TestLesson3));
            // 在序列化时，如果对象中的引用成员为空，那么xml里面时看不到该字段的
            // 例如TestLesson3中，如果test2没有用等于号赋值，那么形成xml文件的时候，就不会有这个标签
            s.Serialize(writer, t);
        }

        // 反序列化
        using (StreamReader reader = new StreamReader(Application.persistentDataPath + "/test.xml"))
        {
            // 序列化的“翻译机器”
            XmlSerializer s = new XmlSerializer(typeof(TestLesson3));
            t = s.Deserialize(reader) as TestLesson3;
        }
        #endregion
    }
}
```

# 让Dictionary支持序列化，反序列化

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Xml;
using System.Xml.Schema;
using System.Xml.Serialization;
using UnityEngine;

public class SerizlizerDictionary<TKey, TValue> : Dictionary<TKey, TValue>, IXmlSerializable
{
    public XmlSchema GetSchema()
    {
        return null;
    }

    //自定义字典的 反序列化 规则
    public void ReadXml(XmlReader reader)
    {
        XmlSerializer keySer = new XmlSerializer(typeof(TKey));
        XmlSerializer valueSer = new XmlSerializer(typeof(TValue));

        //要跳过根节点
        reader.Read();
        //判断 当前不是元素节点 结束 就进行 反序列化
        while (reader.NodeType != XmlNodeType.EndElement)
        {
            //反序列化键
            //	<KeyValuePair>
            //		< Key > 1 </ Key >
            //		< Value > One </ Value >
            //	</ KeyValuePair >
            //	当光标位于<Key> 节点时
            //	XmlReader 的光标指向<Key>
            //	Deserialize 自动解析 < Key > 节点中的内容
            //	查找<Key> 标签中的值 1
            //	转换为与 TKey 类型匹配的值（int 或 string 等）
            //	key 的值被赋为 1
            TKey key = (TKey)keySer.Deserialize(reader);
            //反序列化值
            TValue value = (TValue)valueSer.Deserialize(reader);
            //存储到字典中
            this.Add(key, value);
        }
        reader.Read();
    }

    //自定义 字典的 序列化 规则
    public void WriteXml(XmlWriter writer)
    {
        XmlSerializer keySer = new XmlSerializer(typeof(TKey));
        XmlSerializer valueSer = new XmlSerializer(typeof(TValue));

        foreach (KeyValuePair<TKey, TValue> kv in this)
        {
            //键值对 的序列化
            keySer.Serialize(writer, kv.Key);
            valueSer.Serialize(writer, kv.Value);
        }
    }
}
```


# Xml存储和读取功能实现
```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml.Serialization;
using UnityEngine;

public class XmlDataMgr
{
    private static XmlDataMgr instance = new XmlDataMgr();

    public static XmlDataMgr Instance => instance;

    private XmlDataMgr() { }

    /// <summary>
    /// 保存数据到xml文件中
    /// </summary>
    /// <param name="data">数据对象</param>
    /// <param name="fileName">文件名</param>
    public void SaveData(object data, string fileName)
    {
        //1.得到存储路径
        string path = Application.persistentDataPath + "/" + fileName + ".xml";
        //2.存储文件
        using (StreamWriter writer = new StreamWriter(path))
        {
            //3.序列化
            XmlSerializer s = new XmlSerializer(data.GetType());
            s.Serialize(writer, data);
        }
    }

    /// <summary>
    /// 从xml文件中读取内容
    /// </summary>
    /// <param name="type">对象类型</param>
    /// <param name="fileName">文件名</param>
    /// <returns></returns>
    public object LoadData(Type type, string fileName)
    {
        //1。首先要判断文件是否存在
        string path = Application.persistentDataPath + "/" + fileName + ".xml";
        if (!File.Exists(path))
        {
            path = Application.streamingAssetsPath + "/" + fileName + ".xml";
            if (!File.Exists(path))
            {
                //如果根本不存在文件 两个路径都找过了
                //那么直接new 一个对象 返回给外部 无非 里面都是默认值
                return Activator.CreateInstance(type);
            }
        }
        //2.存在就读取
        using (StreamReader reader = new StreamReader(path))
        {
            //3.反序列化 取出数据
            XmlSerializer s = new XmlSerializer(type);
            return s.Deserialize(reader);
        }
    }
}
```

# 打包程序

- 先将想要打包的文件放在指定位置
![Pasted image 20241205233153.png](https://s2.loli.net/2024/12/05/HrY6qxFjaGTSWlE.png)

![Pasted image 20241205233311.png](https://s2.loli.net/2024/12/05/KXuc61nZzjYx3Ov.png)

- 只选中想要打包的内容
![Pasted image 20241205233342.png](https://s2.loli.net/2024/12/05/oVkTS26H51guyIC.png)
- 后续想要使用的时候，直接拖进Assets中就可以了
![Pasted image 20241205233408.png](https://s2.loli.net/2024/12/05/fVJwkyqTQjmUCcg.png)