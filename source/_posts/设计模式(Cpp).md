---
title: 设计模式(Cpp)
math: true
---
![设计模式书籍.jpg](https://s2.loli.net/2025/02/14/nAdCNZsGIq4amrk.jpg)

# 引言
第一章的引论暂时不看，这个讲的似乎还是比较高级的

# SOLID设计原则
## 单一职责原则(Single Responsibility Principle,SRP)
> 每个类只有一个职责

```cpp
#define _CRT_SECURE_NO_WARNINGS

#include <iostream> // 包含输入输出流头文件
#include <fstream>
#include <vector>
#include <string>
#include <boost/lexical_cast.hpp>
using namespace std;

struct Journal {
	string title;
	vector<string> entries;

	// explicit：这个关键字用于修饰构造函数，表示该构造函数不能通过隐式类型转换来调用。
	// 例如，不能将一个 const string& 类型的值隐式转换为 Journal 类型。
	// 如果没有 explicit，则可以通过类似 Journal j = "My Journal"; 
	// 的方式构造 Journal 对象（"My Journal" 会被自动转换为 string 类型）。
	// :title{title} 表示，花括号里面的是参数title，外面的是成员变量
	explicit Journal(const string& title) :title{ title } {}

	void add(const string& entry);
};

void Journal::add(const string& entry) {
	//定义了一个静态局部变量 count，初始值为 1。
	//	static 关键字表示 count 的生命周期贯穿整个程序运行期间，而不是每次函数调用时重新初始化。
	//	每次调用 add 函数时，count 的值会保留上一次调用结束时的值。
	static int count = 1;
	entries.push_back(boost::lexical_cast<string>(count++) + ": " + entry);
}

// 这里想要添加一个持久化的功能
// 放在Journal中是不合适的，如果保存日记从保存到本地到保存到云端，需要改动Journal类中的方法
// 如果有多个一起修改，那么都需要改，不方便
// 这里其实暂时没这么麻烦，这个单一职责就只是说完成某个单一的功能，书中例子不一定完美
struct PersistenceManager {
	static void save(const Journal& j, const string& filename) {
		ofstream ofs(filename);
		for (auto& s : j.entries) {
			ofs << s << endl;
		}
		ofs.close();

	}
};

// 主函数
int main() {

}
```

## 开闭原则(Open-Closed Principle,OCP)

> 不必返回已经编写和测试的代码来进行修改
> 对拓展开放，对修改关闭

```cpp
#define _CRT_SECURE_NO_WARNINGS

#include <iostream> // 包含输入输出流头文件
#include <fstream>
#include <vector>
#include <string>
using namespace std;

enum class Color { Red, Green, Blue };
enum class Size { Small, Medium, Large };

struct Product {
	string name;
	Color color;
	Size size;
};

struct ProductFilter {
	typedef vector<Product*> Items;
	Items by_color(Items items, Color color);
	Items by_size(Items items, Size sizes);
	Items by_color_and_size(Items items, Color color, Size size);
};

ProductFilter::Items ProductFilter::by_color(Items items, Color color) {
	Items result;
	for (auto& i : items) {
		if (i->color == color) {
			result.push_back(i);
		}
	}
	return result;
}

ProductFilter::Items ProductFilter::by_size(Items items, Size size) {
	Items result;
	for (auto& i : items) {
		if (i->size == size) {
			result.push_back(i);
		}
	}
	return result;
}

ProductFilter::Items ProductFilter::by_color_and_size(Items items, Color color, Size size) {
	Items result;
	for (auto& i : items) {
		if (i->color == color && i->size == size) {
			result.push_back(i);
		}
	}
	return result;
}

template <typename T>
struct Specification {
	virtual bool is_satisfied(T* item) const = 0;
	//AndSpecification<T> operator &&(Specification& other) {
	//	return AndSpecification(*this, other);
	//}
};



template <typename T>
struct Filter {
	virtual vector<T*> filter(vector<T*> items, const Specification<T>& spec) const = 0;
};

struct BetterFilter :Filter<Product> {
	vector<Product*> filter(vector<Product*> items, const Specification<Product>& spec) const override {
		vector<Product*> result;
		for (auto& p : items) {
			if (spec.is_satisfied(p)) {
				result.push_back(p);
			}
		}
		return result;
	}
};

struct ColorSpecification :Specification<Product>
{
	Color color;
	explicit ColorSpecification(const Color color) : color{ color } {}

	bool is_satisfied(Product* item) const override {
		return item->color == color;
	}
};

struct SizeSpecification :Specification<Product>
{
	Size size;
	explicit SizeSpecification(const Size size) : size{ size } {}

	bool is_satisfied(Product* item) const override {
		return item->size == size;
	}
};


template <typename T> struct AndSpecification :Specification<T>
{
	const Specification<T>& first;
	const Specification<T>& second;

	// 传递单独的Color或Size的条件，两个进行合并组成的AndSpecification
	AndSpecification(const Specification<T>& first, const Specification<T>& second) :first{ first }, second{ second } {}

	bool is_satisfied(T* item) const override {
		return first.is_satisfied(item) && second.is_satisfied(item);
	}
};

// 或者
template <typename T>
AndSpecification<T> operator&& (const Specification<T>& first, const Specification<T>& second) {
	return AndSpecification{ first, second };

	// return { first,second };
}


// 主函数
int main() {
	Product apple{ "Apple",Color::Green,Size::Small };
	Product tree{ "Tree",Color::Green,Size::Large };
	Product house{ "House",Color::Blue,Size::Large };

	vector<Product*> all{ &apple,&tree,&house };

	BetterFilter bf;
	//ColorSpecification green(Color::Green);

	//auto green_things = bf.filter(all, green);
	//for (auto& x : green_things) {
	//	cout << x->name << " is green\n";
	//}

	SizeSpecification large(Size::Large);
	ColorSpecification green(Color::Green);
	// auto test = green && large;
	// AndSpecification<Product> green_and_large{ green, large };
	//auto green_large_things = bf.filter(all, green_and_large);
	// 这里green && large产生的是临时变量，不是左值，不能传递进入filter中
	// 这里利用const的特性，能将左值变成右值
	auto green_large_things = bf.filter(all, green && large);
	auto green_large_things_2 = bf.filter(all, ColorSpecification(Color::Green) && SizeSpecification(Size::Large));

	for (auto& x : green_large_things) {
		cout << x->name << " is green and large\n";
	}

	for (auto& x : green_large_things_2) {
		cout << x->name << " is green and large\n";
	}

}

```

## 里氏替换原则(Liskov Substitution Principle,LSP)
> 如果接口以基类Parent类型的对象为参数，那么它应该同等地接受子类Child类对象作为参数
> 下面展示的是失效的情况，因为正方形不能完全适应Rectangle，需要特殊处理

```cpp
#define _CRT_SECURE_NO_WARNINGS

#include <iostream> // 包含输入输出流头文件
#include <fstream>
#include <vector>
#include <string>
using namespace std;


class Rectangle {
protected:
	int width, height;
public:
	Rectangle(const int width, const int height) :width{ width }, height{ height } {}
	int get_width() const { return width; }
	virtual void set_width(const int width) { this->width = width; }

	int get_height() const { return height; }
	virtual void set_height(const int height) { this->height = height; }

	int area() const { return width * height; }

	bool is_square() const;
};

bool Rectangle::is_square() const {
	return width == height;
}

class Square : public Rectangle {
public:
	Square(int size) : Rectangle(size, size) {}
	void set_width(const int width) override {
		this->width = height = width;
	}

	void set_height(const int height) override {
		this->height = width = height;
	}


};

void process(Rectangle& r) {
	int w = r.get_width();
	r.set_height(10);
	cout << "expected area = " << (w * 10) << ", got " << r.area() << endl;
}

struct RectangleFactory {
	static Rectangle create_rectangle(int w, int h);
	static Rectangle crete_square(int size);
};

// 主函数
int main() {

	Square s{ 5 };
	process(s);
}
```



## 接口隔离原则(Interface Segregation Principle,ISP)
> 将所有接口查分开，让实现者根据自身需求挑选接口并实现

```cpp
#define _CRT_SECURE_NO_WARNINGS

#include <iostream> // 包含输入输出流头文件
#include <fstream>
#include <vector>
#include <string>
using namespace std;

struct Document {
	// 随便定义的一个文档类
};

struct IMachine {
	virtual void print(vector<Document*> docs) = 0;
	virtual void fax(vector<Document*> docs) = 0;
	virtual void scan(vector<Document*> docs) = 0;
};

struct MyFavouritePrinter : IMachine {
	void print(vector<Document*> docs) override;
	void fax(vector<Document*> docs) override;
	void scan(vector<Document*> docs) override;
};

struct IPrinter {
	virtual void print(vector<Document*> docs) = 0;
};

struct IScanner {
	virtual void scan(vector<Document*> docs) = 0;
};

struct IFax {
	virtual void fax(vector<Document*> docs) = 0;
};

struct Printer :IPrinter {
	void print(vector<Document*> docs) override;
};

struct Scanner : IScanner {
	void scan(vector<Document*> docs) override;
};

struct Fax : IFax {
	void fax(vector<Document*> docs) override;
};

struct Imachine : IPrinter, IScanner, IFax {

};

struct Machine :IMachine {
	IPrinter& printer;
	IScanner& scanner;

	Machine(IPrinter& pinter, IScanner& scanner) :printer{ printer }, scanner{ scanner } {}

	void print(vector<Document*> docs) override {
		printer.print(docs);
	}

	void scan(vector<Document*> docs) override {
		scanner.scan(docs);
	}

};

// 主函数
int main() {

}
```



## 依赖倒转原则(Dependency Inversion Principle,DIP)

> 高层模块不依赖低层模块，应该依赖抽象接口
> 抽象接口不应该依赖细节，细节应该依赖抽象接口
> 

```cpp
#define _CRT_SECURE_NO_WARNINGS

#include <iostream> // 因此，要使用 ostream，你只需要包含 <iostream>
// #include <ostream> // 有ostream 类，但是 iostream 头文件包含了输入输出流所需的所有类（包括 istream 和 ostream）
#include <fstream>
#include <vector>
#include <string>
#include <boost/di.hpp>
//
//namespace di = boost::di;
using namespace std;


struct ILogger {
	virtual ~ILogger() {}
	virtual void Log(const string& s) = 0;

};

class Reporting {
	const ILogger& logger;
public:
	Reporting(const ILogger& logger) :logger{ logger } {}
	void prepare_report() {
		// ...
	}
};

struct Engine {
	float volume = 5;
	int horse_power = 400;

	// 这里的ostream是输出流
	friend ostream& operator<<(ostream& os, const Engine& obj) {
		return os << "volume: " << obj.volume << " horse_power: " << obj.horse_power;
	}
};

struct ConsoleLogger :ILogger {
	ConsoleLogger() {}

	void Log(const string& s) override {
		// c_str()返回值类型const char*，是C风格的字符串
		cout << "LOG: " << s.c_str() << endl;
	}
};

struct Car {
	unique_ptr<Engine> engine;
	shared_ptr<ILogger> logger;
	// unique_ptr 是 独占式的智能指针，它不允许两个 unique_ptr 拥有相同的资源。当你把一个 unique_ptr 从一个地方传递到另一个地方时，你需要显式地表明你“转移”了所有权
	Car(unique_ptr<Engine> engine, const shared_ptr<ILogger>& logger) : engine{ move(engine) }, logger{ logger } {
		logger->Log("making a car");
	}

	friend ostream& operator<<(ostream& os, const Car& obj) {
		return os << "car with engine: " << *obj.engine;
	}
};

// 主函数
int main() {
	//auto injector = di::make_injector(di::bind<ILogger>().to<ConsoleLogger>());
	//auto car = injector.create<shared_ptr<Car>>();

}
```

# 第二章节 构造器模式
> 构造器模式的目的是简化复杂对象或一系列对象的构建过程，从而单独定义构成该复杂对象的各个组件的构建方法
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <functional>
using namespace std;

struct HtmlElement{
friend struct HtmlBuilder;
public:
	string name, text;
	// emplace_back无法使用HtmlElement的构造函数
	vector<HtmlElement> elements;
	const size_t indent_size = 2;
	static unique_ptr<HtmlBuilder> create(const string& root_name) {
		return make_unique<HtmlBuilder>(root_name);
	}

protected:
	HtmlElement() {}
	HtmlElement(const string& name, const string& text) :name(name), text(text) {}
public:
	string str(int indent = 0) const {
		// pretty-print the contents
		// (implementation omitted)
		string total_text;
		for (auto element : elements) {
			total_text += "<" + element.name + ">" + element.text + "</" + element.name + ">\n";
		}
		return total_text;
	}
};

struct HtmlBuilder {
public:
	HtmlElement root;
	// 作用：将 HtmlBuilder 对象隐式转换为 HtmlElement 对象
	operator HtmlElement() const {
		return root;
	}

	HtmlElement build() const {
		return root;
	}
	// other operations omitted
	HtmlBuilder(string root_name) : root(root_name, "") {}
	// 这是流式接口，可以再Builder上实现链式调用
	HtmlBuilder& add_child(string child_name, string child_text) {
		// elements会自动调用HtmlElement的string,string的构造函数
		// 但是是protected，不可以使用，所以只能显式构造
		// root.elements.emplace_back(child_name, child_text); //报错
		root.elements.emplace_back(HtmlElement(child_name, child_text));
		return *this;
	}
	string str() { return root.str(); }
};



//struct HtmlBuilder {
//	HtmlElement root;
//
//	HtmlBuilder(string root_name) { root.name = root_name; }
//
//	//void add_child(string  child_name, string child_text) {
//	//	root.elements.emplace_back(child_name, child_text);
//	//}
//
//	// 这是流式接口，可以再Builder上实现链式调用
//	HtmlBuilder& add_child(string child_name, string child_text) {
//		root.elements.emplace_back(child_name, child_text);
//		return *this;
//	}
//
//	string str() { return root.str(); }
//};


struct Tag {
	string name;
	string text;
	vector<Tag> children;
	vector<pair<string, string>> attributes;
	friend ostream& operator<<(ostream& os, const Tag& tag) {
		// implementation omitted
		os << tag.name << endl;
		return os;
	}
protected:
	Tag(const string& name, const string& text) :name(name), text(text) {}
	Tag(const string& name, const vector<Tag>& children) : name(name), children(children) {}

};

struct P :public Tag {
	explicit P(const string& text) :Tag("p", text) {}
	P(initializer_list<Tag> children) :Tag("p", children) {}
};

struct IMG : Tag {
	explicit IMG(const string& url) : Tag{ "img","" } {
		attributes.emplace_back(make_pair("src",url));
	}
};

//class PersonBuilderBase {
//protected:
//	Person& person;
//	explicit PersonBuilderBase(Person& person) :person{ person } {}
//public:
//	operator Person() { return move(person); }
//	// builder facets
//	PersonAddressBuilder lives() const;
//	PersonJobBuilder works() const;
//};
//
//class Person {
//public:
//	// address
//	string street_address, post_code, city;
//	//employment
//	string company_name, position;
//	int annual_income = 0;
//public:
//	Person() {}
//	static unique_ptr<PersonBuilderBase> create() {
//		return make_unique<PersonBuilderBase>();
//	}
//};
//
//class PersonAddressBuilder :public PersonBuilderBase
//{
//	typedef PersonAddressBuilder self;
//public:
//	explicit PersonAddressBuilder(Person& person) :PersonBuilderBase{ person } {}
//	self& at(string street_address) {
//		person.street_address = street_address;
//		return *this;
//	}
//	self& with_postcode(string post_code) { cout << "with_postcode" << endl; }
//	self& in(string city) { cout << "in" << endl; }
//};
//
//class PersonJobBuilder :public PersonBuilderBase
//{
//	typedef PersonJobBuilder self;
//public:
//	explicit PersonJobBuilder(Person& person) :PersonBuilderBase{ person } {}
//	self& at(string street_address) {
//		person.street_address = street_address;
//		return *this;
//	}
//	self& with_postcode(string post_code) { cout << "with_postcode" << endl; }
//	self& in(string city) { cout << "in" << endl; }
//};
//
//PersonAddressBuilder PersonBuilderBase::lives() const {
//	Person person;
//	PersonAddressBuilder temp(person);
//	return temp;
//}
//
//PersonJobBuilder PersonBuilderBase::works() const {
//	Person person;
//	PersonJobBuilder temp(person);
//	return temp;
//}
//
//
//
//
//
//class PersonBuilder : public PersonBuilderBase
//{
//	Person p; // object being built
//public:
//	PersonBuilder() : PersonBuilderBase{ p } {}
//};

//参数化构造器




class MailService {
	class Email {
	public:
		string from, to, subject, body;
		// possibly other members here
	}; // keep it private
public:
	class EmailBuilder {
		Email& email;
	public:
		explicit EmailBuilder(Email& email) :email(email) {}
		EmailBuilder& from(string from) {
			email.from = from;
			return *this;
		}
		// other fluent member here
	};
	void send_email(function<void(EmailBuilder&)> builder) {
		Email email;
		EmailBuilder b(email);
		builder(b);
		send_email_impl(email);

	}
private:
	void send_email_impl(const Email& email) {
		// actually send the email
	}
};

// 构造器模式的继承性
class Person {
public:
	string name, position, date_of_birth;

	friend ostream& operator<<(ostream& os, const Person& obj) {
		return os << "name: " << obj.name << " position: " << obj.position << " date_of_birth: " << obj.date_of_birth << endl;
	}
};

class PersonBuilder {
protected:
	Person person;
public:
	[[_nodiscard]] Person build() const{
		return person;
	}
};

//class PersonInfoBuilder : public PersonBuilder {
//public:
//	PersonInfoBuilder& called(const string& name) {
//		person.name = name;
//		return *this;
//	}
//};

template <typename TSelf>
class PersonInfoBuilder :public PersonBuilder {
public:
	TSelf& called(const string& name) {
		person.name = name;
		return static_cast<TSelf&>(*this);
		// alternatively &static_cast<Tself*>(this);
	}
};

//class PersonJobBuilder :public PersonInfoBuilder {
//public:
//	PersonJobBuilder& work_as(const string& position) {
//		person.position = position;
//		return *this;
//	}
//};

template <typename TSelf>
class PersonJobBuilder :public PersonInfoBuilder<PersonJobBuilder<TSelf>>
{
public:
	TSelf& works_as(const string& position) {
		this->person.position = position;
		return static_cast<TSelf&>(*this);
	}
};

template <typename TSelf>
class PersonBirthDateBuilder :public PersonJobBuilder<PersonBirthDateBuilder<TSelf>> {
public:
	TSelf& born_on(const string& date_of_birth) {
		this->person.date_of_birth = date_of_birth;
		return static_cast<TSelf&>(*this);
	}
};

class MyBuilder :public PersonBirthDateBuilder<MyBuilder> {};


int main() {

	//string words[] = { "hello","world" };
	//HtmlElement list{ "ul","" };

	//for (auto w : words) {
	//	list.elements.emplace_back("li", w);
	//}
	//printf(list.str().c_str());

	//HtmlBuilder builder("ul");
	//builder.add_child("li", "hello");
	//builder.add_child("li", "world");
	//builder.add_child("li", "hello").add_child("li", "world");
	//cout << builder.str() << endl;

	//HtmlElement e = HtmlElement::create("ul")->add_child("li", "hello").add_child("li", "world");
	//cout << e.str() << endl;
	//return 0;

	//cout<< 
	//	P{ 
	//	IMG{"http://pokemon.com/pikachu.png"}
	//}
	//<< endl;

	//Person p = Person::create()->lives().at("123 London Road").with_postcode("SW1 1GB").in("London").works().at("PragmaSoft");
	//MailService ms;
	//ms.send_email([&](auto& eb) {
	//	//eb.from("foo@bar.com").to("bar@baz.com").subject("hello").body("Hello, how are you?");
	//	});

	MyBuilder mb;
	auto me = mb.called("Dmitri").works_as("Programmer").born_on("01/01/1999").build();
	cout << me;
	// name: Dmitri position: Programmer date_of_birth: 01/01/1999

}
```

# 第三章 工厂方法和抽象工厂模式
### 3.1 预想方案

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <functional>
using namespace std;

class Point2D {
	int x, y;
};

class Wall {
	Point2D start, end;
	int elevation, height;
public:
	Wall(Point2D start, Point2D end, int elevation, int height) :start{ start }, end{ end }, elevation{ elevation }, height{ height } {}

};

enum class Material {
	brick,
	aerated_concrete,
	dry_wall
};

class SolidWall :public Wall {
	int width;
	Material material;
public:
	SolidWall(Point2D start,Point2D end, int elevation, int height, int width,Material material):Wall{start,end,elevation,height},width{width},material{material}{	
	}
	SolidWall(const Point2D start, const Point2D end,
		const int elevation,
		const int height, const int width,
		const Material material) :Wall{ start,end,elevation,height } {
		if (elevation < 0 && material == Material::aerated_concrete) {
			throw invalid_argument("elevation");
		}
		if (width < 120 && material == Material::brick)
			throw invalid_argument("width");
	}
};

int main() {

}
```


### 工厂方法

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <functional>
using namespace std;

class Point2D {
public:
	int x, y;
	Point2D() {

	}
	Point2D(int x, int y) :x{ x }, y{y} {}
};

class Wall {
	Point2D start, end;
	int elevation, height;
public:
	Wall(Point2D start, Point2D end, int elevation, int height) :start{ start }, end{ end }, elevation{ elevation }, height{ height } {}

};

enum class Material {
	brick,
	aerated_concrete,
	dry_wall
};

class SolidWall :public Wall {
	int width;
	Material material;
protected:
	SolidWall(const Point2D start, const Point2D end,
		const int elevation,
		const int height, const int width, const Material material);
public:
	static SolidWall create_main(Point2D start, Point2D end,
		int elevation, int height) {
		return SolidWall{ start,end,elevation,height,375,Material::aerated_concrete };

	}

	friend ostream& operator <<(ostream& os, const SolidWall& obj) {
		return os << "start: "<<endl;
	}

	static unique_ptr<SolidWall> create_partition(Point2D start, Point2D end,
		int elevation, int height) {
		unique_ptr<SolidWall> p0(new SolidWall(start, end, elevation,
			height, 120, Material::brick));
		return p0;
	}
	
};

SolidWall::SolidWall(const Point2D start, const Point2D end,
	const int elevation,
	const int height, const int width, const Material material)
	: Wall(start, end, elevation, height), width(width), material(material) {
	// 构造函数的具体实现
}

int main() {
	const auto main_wall = SolidWall::create_main({ 0,0 }, { 0,3000 },2700,3000);
	cout << main_wall << "\n";

}
```

### 工厂
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <functional>
using namespace std;

class Point2D {
public:
	int x, y;
	Point2D() {

	}
	Point2D(int x, int y) :x{ x }, y{y} {}
};

class Wall {
	Point2D start, end;
	int elevation, height;
public:
	Wall(Point2D start, Point2D end, int elevation, int height) :start{ start }, end{ end }, elevation{ elevation }, height{ height } {}

};

enum class Material {
	brick,
	aerated_concrete,
	dry_wall
};

class SolidWall :public Wall {
public:
	friend class WallFactory;
	int width;
	Material material;
protected:
	SolidWall(const Point2D start, const Point2D end,
		const int elevation,
		const int height, const int width, const Material material);
public:
	static shared_ptr<SolidWall> create_main(Point2D start,
		Point2D end, int elevation, int height) {
		if (elevation < 0) return {};
		unique_ptr<SolidWall> p0(new SolidWall(start, end, elevation, height, 375, Material::aerated_concrete));
		return p0;
	}

	friend ostream& operator <<(ostream& os, const SolidWall& obj) {
		return os << "start: "<<endl;
	}

	static unique_ptr<SolidWall> create_partition(Point2D start, Point2D end,
		int elevation, int height) {
		unique_ptr<SolidWall> p0(new SolidWall(start, end, elevation,
			height, 120, Material::brick));
		return p0;
	}
	
};

SolidWall::SolidWall(const Point2D start, const Point2D end,
	const int elevation,
	const int height, const int width, const Material material)
	: Wall(start, end, elevation, height), width(width), material(material) {
	// 构造函数的具体实现
}

class WallFactory {
	static vector<weak_ptr<Wall>> walls;
public:
	static shared_ptr<SolidWall> create_main(Point2D start,
		Point2D end, int elevation, int height) {
		// as before
	}

	static shared_ptr<SolidWall> create_partition(Point2D start,
		Point2D end,
		int elevation, int height) {
		const auto this_wall = new SolidWall{ start,end,elevation,height,120,Material::brick };
		// ensure we don't intersect other walls
		for (const auto wall : walls) {
			if (auto p = wall.lock()) {
				if (true) {
					delete this_wall;
					return {};
				}
			}
		}

		shared_ptr<SolidWall> ptr(this_wall);
		walls.push_back(ptr);
		return ptr;
	}
};

int main() {
	const auto partition = SolidWall::create_main({ 0,0 }, { 2000,4000 }, 0, 2700);
	cout << (*partition) << endl;
}
```

### 工厂方法和多态
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <functional>
using namespace std;

class Point2D {
public:
	int x, y;
	Point2D() {

	}
	Point2D(int x, int y) :x{ x }, y{y} {}
};

enum class WallType {
	basic,
	main,
	partition
};


class Wall {
	Point2D start, end;
	int elevation, height;
public:
	Wall(Point2D start, Point2D end, int elevation, int height) :start{ start }, end{ end }, elevation{ elevation }, height{ height } {}

};

enum class Material {
	brick,
	aerated_concrete,
	dry_wall
};

class SolidWall :public Wall {
public:
	friend class WallFactory;
	int width;
	Material material;
protected:
	SolidWall(const Point2D start, const Point2D end,
		const int elevation,
		const int height, const int width, const Material material);
public:
	static shared_ptr<SolidWall> create_main(Point2D start,
		Point2D end, int elevation, int height) {
		if (elevation < 0) return {};
		unique_ptr<SolidWall> p0(new SolidWall(start, end, elevation, height, 375, Material::aerated_concrete));
		return p0;
	}

	friend ostream& operator <<(ostream& os, const SolidWall& obj) {
		return os << "start: "<<endl;
	}

	static unique_ptr<SolidWall> create_partition(Point2D start, Point2D end,
		int elevation, int height) {
		unique_ptr<SolidWall> p0(new SolidWall(start, end, elevation,
			height, 120, Material::brick));
		return p0;
	}
	
};

SolidWall::SolidWall(const Point2D start, const Point2D end,
	const int elevation,
	const int height, const int width, const Material material)
	: Wall(start, end, elevation, height), width(width), material(material) {
	// 构造函数的具体实现
}

class WallFactory {
	static vector<weak_ptr<Wall>> walls;
public:
	static shared_ptr<SolidWall> create_main(Point2D start,
		Point2D end, int elevation, int height) {
		// as before
	}

	static shared_ptr<SolidWall> create_partition(Point2D start,
		Point2D end,
		int elevation, int height) {
		const auto this_wall = new SolidWall{ start,end,elevation,height,120,Material::brick };
		// ensure we don't intersect other walls
		for (const auto wall : walls) {
			if (auto p = wall.lock()) {
				if (true) {
					delete this_wall;
					return {};
				}
			}
		}

		shared_ptr<SolidWall> ptr(this_wall);
		walls.push_back(ptr);
		return ptr;
	}

	static shared_ptr<Wall> create_wall(WallType type, Point2D start, Point2D end, int elevation, int height) {
		switch (type) {
		case WallType::main:
			return {};
		}
	}
};



int main() {
	const auto also_partition = WallFactory::create_wall(WallType::partition,{0,0},{5000,0},0,4200);
	if (also_partition) {
		cout << "\n";
	}
}
```


### 嵌套工厂
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <functional>
using namespace std;

class Point2D {
public:
	int x, y;
	Point2D() {

	}
	Point2D(int x, int y) :x{ x }, y{y} {}
};

class Wall {
	Point2D start, end;
	int elevation, height;
	Wall(Point2D start, Point2D end, int elevation, int height) :start{ start }, end{ end }, elevation{ elevation }, height{ height } {}
private:
	class BasicWallFactory {
		BasicWallFactory() = default;
		
	public:
		shared_ptr<Wall> create(const Point2D start,
			const Point2D end,
			const int elevation, const int height) {
			Wall* wall = new Wall(start, end, elevation, height);
			return shared_ptr<Wall>(wall);
		}
		// 让 Wall 访问工厂
		friend class Wall;
	};
public:
	static BasicWallFactory factory;
};
// **定义私有静态成员变量**
Wall::BasicWallFactory Wall::factory;

int main() {
	auto basic = Wall::factory.create({0,0},{5000,0},0,3000);
}
```

### 抽象工厂
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

struct HotDrink {
	virtual void prepare(int volume) = 0;
};


struct Tea :HotDrink {
	void prepare(int volume) override {
		cout << "Take tea bag,boil water,pour " << volume << "m1, adde some lemon" << endl;
	}
};

struct Coffee :HotDrink {
	void prepare(int volume) override {
		cout << "Take coffee bag,boil water,pour " << volume << "m1, adde some lemon" << endl;
	}
};



unique_ptr<HotDrink> make_drink(string type) {
	unique_ptr<HotDrink> drink;
	if (type == "tea") {
		drink = make_unique<Tea>();
		drink->prepare(200);
	}
	else {
		drink = make_unique<Coffee>();
		drink->prepare(50);
	}
	return drink;
}

class HotDrinkFactory {
public:
	virtual unique_ptr<HotDrink> make() const = 0;
};

class TeaFactory : public HotDrinkFactory {
public:
	unique_ptr<HotDrink> make() const override {
		return make_unique < Tea>();
	}
};

class CoffeeFactory : public HotDrinkFactory {
public:
	unique_ptr<HotDrink> make() const override {
		return make_unique < Coffee>();
	}
};

class DrinkFactory {
	map<string, unique_ptr<HotDrinkFactory>> hot_factories;
public:
	DrinkFactory() {
		hot_factories["coffee"] = make_unique<CoffeeFactory>();
		hot_factories["tea"] = make_unique<TeaFactory>();
	}

	unique_ptr<HotDrink> make_drink(const string& name) {
		auto drink = hot_factories[name]->make();
		drink->prepare(200); // oops!
		return drink;
	}
};

int main() {

}
```

### 函数式工厂
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

struct HotDrink {
	virtual void prepare(int volume) = 0;
};


struct Tea :HotDrink {
	void prepare(int volume) override {
		cout << "Take tea bag,boil water,pour " << volume << "m1, adde some lemon" << endl;
	}
};

struct Coffee :HotDrink {
	void prepare(int volume) override {
		cout << "Take coffee bag,boil water,pour " << volume << "m1, adde some lemon" << endl;
	}
};



unique_ptr<HotDrink> make_drink(string type) {
	unique_ptr<HotDrink> drink;
	if (type == "tea") {
		drink = make_unique<Tea>();
		drink->prepare(200);
	}
	else {
		drink = make_unique<Coffee>();
		drink->prepare(50);
	}
	return drink;
}

class HotDrinkFactory {
public:
	virtual unique_ptr<HotDrink> make() const = 0;
};

class TeaFactory : public HotDrinkFactory {
public:
	unique_ptr<HotDrink> make() const override {
		return make_unique < Tea>();
	}
};

class CoffeeFactory : public HotDrinkFactory {
public:
	unique_ptr<HotDrink> make() const override {
		return make_unique < Coffee>();
	}
};

class DrinkFactory {
	map<string, unique_ptr<HotDrinkFactory>> hot_factories;
public:
	DrinkFactory() {
		hot_factories["coffee"] = make_unique<CoffeeFactory>();
		hot_factories["tea"] = make_unique<TeaFactory>();
	}

	unique_ptr<HotDrink> make_drink(const string& name) {
		auto drink = hot_factories[name]->make();
		drink->prepare(200); // oops!
		return drink;
	}
};

template<typename T>
void construct(function<T()> f) {
	T t = f();
	// use t somehow
}

class DrinkWithVolumeFactory {
	map < string, function<unique_ptr<HotDrink>()>> factories;
public:
	DrinkWithVolumeFactory() {
		factories["tea"] = [] {
			auto tea = make_unique<Tea>();
			tea->prepare(200);
			return tea;
			};
	}

	inline unique_ptr<HotDrink> make_drink(const string& name) {
		return factories[name]();
	}
};

int main() {

}
```

### 对象追踪


# 第四章原型模式
### 对象构建

### 普通拷贝
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

class Address {
public:
	string street, city;
	int suite;
};

class Contact {
public:
	string name;
	Address address;
};

// 因为代码Contact jane = john会拷贝地址指针，所以john和jane以及其他
// 每一个原型拷贝都会共享一个地址
//class Contact {
//public:
//	string name;
//	Address* address; // pointer{reference,shared_ptr,etc..}
//	~Contact() { delete address; }
//};

int main() {
	// here is prototype:
	Contact worker{ "",Address{"123 East Dr","London",0} };
	// make a cpoy of prototype and customize it
	Contact john = worker;
	john.name = "John Doe";
	john.address.suite = 10; 
}
```

### 通过拷贝构造函数进行拷贝
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

class Address {
public:
	string street, city;
	int suite;
	// Address(string street, string city, int suite) :street(street), city(city), suite(suite) {}
	Address(const string& street, const string& city, const int suite) :street{ street }, city{ city }, suite{ suite } { cout << "Address(const string& street, const string& city, const int suite)" << endl; }
};

class Contact {
public:
	string name;
	Address* address;
	//Contact(const Contact& other) :name{ other.name }//, address{ new Address{*other.address} }
	//{
	//	address = new Address(
	//		other.address->street,
	//		other.address->city,
	//		other.address->suite
	//	);
	//}
	Contact() {}


	Contact(string name,Address* address) :name{ name } ,address{ address}{}

	Contact(const Contact& other) :name{ other.name }, address{ new Address{*other.address} }{}
	Contact& operator=(const Contact& other) {
		if (this == &other) {
			return *this;
		}
		name = other.name;
		address = other.address;
		return *this;
	}
};


int main() {
	Contact worker{ "",new Address{"123 East Dr","London",0}};
	Contact john{ worker }; // or: Contact john = worker;
	john.name = "John";
	john.address->suite = 10;
}
```

### “虚”构造函数
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

class Address {
public:
	string street, city;
	int suite;
	// Address(string street, string city, int suite) :street(street), city(city), suite(suite) {}
	Address(const string& street, const string& city, const int suite) :street{ street }, city{ city }, suite{ suite } { cout << "Address(const string& street, const string& city, const int suite)" << endl; }
	//virtual Address clone() {
	//	return Address{ street,city,suite };
	//}

	virtual Address* clone() {
		return new Address{ street,city,suite };
	}
};

class ExtendedAddress :public Address {
public:
	string country, postcode;
	ExtendedAddress(const string& street, const string& city,
		const int suite, const string& country,
		const string& postcode) :
		Address{ street, city, suite }, country{ country }, postcode{postcode} {}
	ExtendedAddress* clone() override {
		return new ExtendedAddress(street, city, suite, country, postcode);
	}
	//ExtendedAddress* clone() override {
	//	return new ExtendedAddress(*this);
	//}
};

int main() {
	ExtendedAddress ea{ "123 West Dr","London",123,"UK","SW101EG" };
	Address& a = ea;// upcast
	auto cloned = a.clone();
}
```

### 序列化
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

class Address {
public:
	string street;
	string city;
	int suite;
private:
	friend class boost::serialization::access;
	template<class Ar> void serialize(
		Ar& ar,
		const unsigned int version)
	{
		ar& street;
		ar& city;
		ar& suite;
	}
};

class Contact {
public:
	string name;
	Address* address = nullptr;
private:
	friend class boost::serialization::access;
	template<class Ar> void serialize(Ar& ar,
		const unsigned int version) {
		ar& name;
		ar& address; // note,no * here
	}
public:
	template <typename T> T clone(T obj) {
		// 1. Serialize the object
		ostringstream oss;
		boost::archive::text_oarchive oa(sss);
		oa << obj;
		string s = oss.str();
		
		// 2. Deserialize it
		istringstream iss(oss.str());
		boost::archive::text_iarchive ia(iss);
		T result;
		ia >> result;
		return result;
	}
};



int main() {
	Contact john{ "",new Address{"123 East Dr","London",0} };
	Contact jane = clone(john);
	jane.name = "Jane";
}
```

### 原型工厂
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

class Address {
public:
	string street, city;
	int suite;
	// Address(string street, string city, int suite) :street(street), city(city), suite(suite) {}
	Address(const string& street, const string& city, const int suite) :street{ street }, city{ city }, suite{ suite } { cout << "Address(const string& street, const string& city, const int suite)" << endl; }
};

class Contact {
public:
	string name;
	Address* address;
	//Contact(const Contact& other) :name{ other.name }//, address{ new Address{*other.address} }
	//{
	//	address = new Address(
	//		other.address->street,
	//		other.address->city,
	//		other.address->suite
	//	);
	//}
	Contact() {}


	Contact(string name, Address* address) :name{ name }, address{ address } {}

	Contact(const Contact& other) :name{ other.name }, address{ new Address{*other.address} } {}
	Contact& operator=(const Contact& other) {
		if (this == &other) {
			return *this;
		}
		name = other.name;
		address = other.address;
		return *this;
	}
};

class EmployeeFactory {
public:
	static Contact main;
	static Contact aux;

	static unique_ptr<Contact> NewEmployee(string name, int suite, Contact& proto)
	{
		auto result = make_unique<Contact>(proto);
		result->name = name;
		result->address->suite = suite;
		return result;
	}
public:
	static unique_ptr<Contact> NewMainOfficeEmployee(
		string name, int suite) {
		return NewEmployee(name, suite, main);
	}
	static unique_ptr<Contact> NewAuxOfficeEmployee(string name, int suite) {
		return NewEmployee(name, suite, aux);
	}
};

Contact EmployeeFactory::main;
Contact EmployeeFactory::aux;

int main() {
	auto john = EmployeeFactory::NewAuxOfficeEmployee("John Doe", 123);
	auto jane = EmployeeFactory::NewMainOfficeEmployee("Jane Doe", 125);
}
```

# 第五章单例模式
### 单例模式的经典实现
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
using namespace std;

//struct Database {
//	// \brief Please do not create more than one instance.
//	Database() {}
//	// C++11之后，静态对象在初始化中不可以被多个线程访问
//	Database& get_database() {
//		static Database database;
//		return database;
//	}
//};


//struct Database {
//	Database() {
//		static int instance_count{ 0 };
//		if (++instance_count > 1) {
//			throw exception("Cannot make > 1 database!");
//		}
//	}
//};

//struct Database {
//protected:
//	Database(){/*do what you need to do */ }
//public:
//	//static Database& get() {
//	//	// thread-salfe since C++11
//	//	static Database database;
//	//	return database;
//	//}
//
//	static Database& get() {
//		// 只有指针而非整个对象是静态的
//		static Database* database = new Database();
//		return *database;
//	}
//
//	Database(Database const&) = delete;
//	Database(Database&&) = delete;
//	Database& operator=(Database const&) = delete;
//	Database& operator=(Database&&) = delete;
//};

// 双重校验锁保证线程安全
struct Database {
	// same members as before , but hen...
	static Database& instance();
private:
	static boost::atomic<Database*> instance;
	static boost::mutex mtx;
};

Database& Database::instance() {
	Database* db = instance.load(boost::memory_order_consume);
	if (!db) {
		boost::mutex::scoped_lock lock(mtx);
		db = instance.load(boost::memory_order_consume);
		if (!db) {
			db = new Database();
			instance.store(db.boost::memory_order_release);
		}
	}
}
int main() {


	return 0;
}
```


### 单例模式存在的问题
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

class Database {
public:
	virtual int get_population(const string& name) = 0;
};

class SingletonDatabase : public Database {
	SingletonDatabase() {/*read data from database*/ }
	map<string, int> capitals;
public:
	SingletonDatabase(SingletonDatabase const&) = delete;
	void operator=(SingletonDatabase const&) = delete;

	static SingletonDatabase& get() {
		static SingletonDatabase db;
		return db;
	}

	int get_population(const string& name) override {
		{
			return capitals[name];
		} }
};

struct SingletonRecordFinder {
	int total_population(vector<string> names) {
		int result = 0;
		for (auto& name : names) {
			result += SingletonDatabase::get().get_population(name);
		}
		return result;
	}
};

struct ConfigurableRecordFinder {
	explicit ConfigurableRecordFinder(Database& db):db{db}{}
	int total_population(vector<string> names) {
		int result = 0;
		for (auto& name : names) {
			result += db.get_population(name);

		}
		return result;
	}
	Database& db;
};

class DummyDatabase : public Database {
	map<string, int> capitals;
public:
	DummyDatabase() {
		capitals["alpha"] = 1;
		capitals["beta"] = 2;
		capitals["gamma"] = 3;
	}

	int get_population(const string& name) override {
		return capitals[name];
	}
};

class PerThreadSingleton {
	PerThreadSingleton() {
		id = this_thread::get_id();
	}
public:
	thread::id id;
	static PerThreadSingleton& get() {
		thread_local PerThreadSingleton instance;
		return instance;
	}
};

static class BuildingContext final {
public:
	int height{ 0 };
	BuildingContext() = default;
	static stack<BuildingContext> st;
	// later initialized with
	stack<BuildingContext> st;
	static BuildingContext current() {
		return st.top();
	}
	static Token with_height(int height,function<void()> action) {
		auto copy = current();
		copy.height = height;
		st.push(copy);
		action();
		return Token{};
	}

};

//class Token {
//public:
//	~Token() {
//		if (st.size() > 1) st.pop();
//	}
//};

// 单态模式
class Printer {
	static int id;
public:
	int get_id() const { return id; }
	void set_id(int value) { id = value; }
};

int main() {
	//thread t1([]() {
	//	cout << "t1: " << PerThreadSingleton::get().id << "\n";
	//	}

	//);

	//thread t2([]() {
	//	cout << "t2: " << PerThreadSingleton::get().id << "\n";
	//	cout << "t2 again: " << PerThreadSingleton::get().id << "\n";
	//	}

	//);

	//t1.join();
	//t2.join();

	// auto injector = di::make_injector(di::bind<IFoo>.to<Foo>.in(di::singleton));

	return 0;
}
```


# 第六章适配器模式

### 预想方案
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

struct Point {
	int x, y;

};

struct Line {
	Point start, end;
};

struct VectorObject {
	virtual vector<Line>::iterator begin() = 0;
	virtual vector<Line>::iterator end() = 0;
};

struct VectorRectangle :VectorObject {
	VectorRectangle(int x, int y,int width, int height) {
		lines.emplace_back(Line{ Point{x,y},Point{x + width,y} });
		lines.emplace_back(Line{ Point{x + width,y},Point{x + width,y + height} });
		lines.emplace_back(Line{ Point{x,y},Point{x,y + height} });
		lines.emplace_back(Line{ Point{x,y+height},Point{x+width,y + height} });
	}
	vector<Line>::iterator begin() override {
		return lines.begin();
	}
	vector<Line>::iterator end() override {
		return lines.end();
	}
	//void DrawPoints(CPaintDC& dc, vector<Point>::iterator start, vector<Point>::iterator end) {
	//	for (auto i = start, i != end; i++) {
	//		dc.SetPixel(i->x, i->y, 0);
	//	}
	//}
private:
	vector<Line> lines;
};

int main() {

}
```

### 适配器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

struct Point {
	int x, y;

};

struct Line {
	Point start, end;
};

struct VectorObject {
	virtual vector<Line>::iterator begin() = 0;
	virtual vector<Line>::iterator end() = 0;
};

struct VectorRectangle :VectorObject {
	VectorRectangle(int x, int y,int width, int height) {
		lines.emplace_back(Line{ Point{x,y},Point{x + width,y} });
		lines.emplace_back(Line{ Point{x + width,y},Point{x + width,y + height} });
		lines.emplace_back(Line{ Point{x,y},Point{x,y + height} });
		lines.emplace_back(Line{ Point{x,y+height},Point{x+width,y + height} });
	}
	vector<Line>::iterator begin() override {
		return lines.begin();
	}
	vector<Line>::iterator end() override {
		return lines.end();
	}
	//void DrawPoints(CPaintDC& dc, vector<Point>::iterator start, vector<Point>::iterator end) {
	//	for (auto i = start, i != end; i++) {
	//		dc.SetPixel(i->x, i->y, 0);
	//	}
	//}
private:
	vector<Line> lines;
};

struct LineToPointAdapter {
	typedef vector<Point> Points;
	LineToPointAdapter(Line& line) {
		int left = min(line.start.x, line.end.x);
		int right = max(line.start.x, line.end.x);
		int top = min(line.start.y, line.end.y);
		int bottom = max(line.start.y, line.end.y);
		int dx = right - left;
		int dy = line.end.y - line.start.y;
		// we only support vertical or horizontal lines
		if (dx == 0) {
			//vertical
			for (int y = top; y <= bottom; ++y) {
				points.emplace_back(Point{ left,y });
			}
		}
		else if (dy == 0) {
			// horizontal
			for (int x = left; x <= right; ++x) {
				points.emplace_back(Point{ x,top });
			}
		}
	};
	virtual Points::iterator begin() { return points.begin(); }
	virtual Points::iterator end() { return points.end(); }
private:
	Points points;
};



int main() {
	vector<shared_ptr<VectorObject>> vectorObjects{
		make_shared<VectorRectangle>(10,10,100,100),
		make_shared<VectorRectangle>(30,30,60,60)
	};
	//for (const auto& obj : vectorObjects) {
	//	for (const auto& line : *obj) {
	//		LineToPointAdapter lpo{ line };
	//		DrawPoints(dc, lpo.begin(), lpo.end());
	//	}
	//}
}
```

### 临时适配器对象
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

struct Point {
	int x, y;

	friend size_t hash_value(const Point& obj) {
		size_t seed = 0x725C686F;
		//boost::hash_combine(seed, obj.x);
		//boost::hash_combine(seed, obj.y);
		return seed;
	}
};

struct Line {
	Point start, end;

	friend size_t hash_value(const Line& obj) {
		size_t seed = 0x719E6B16;
		//boost;:hash_combine(seed, obj.start);
		//boost::hash_combine(seed, obj.end);
		return seed;
	}
};

struct VectorObject {
	virtual vector<Line>::iterator begin() = 0;
	virtual vector<Line>::iterator end() = 0;
};

struct VectorRectangle :VectorObject {
	VectorRectangle(int x, int y,int width, int height) {
		lines.emplace_back(Line{ Point{x,y},Point{x + width,y} });
		lines.emplace_back(Line{ Point{x + width,y},Point{x + width,y + height} });
		lines.emplace_back(Line{ Point{x,y},Point{x,y + height} });
		lines.emplace_back(Line{ Point{x,y+height},Point{x+width,y + height} });
	}
	vector<Line>::iterator begin() override {
		return lines.begin();
	}
	vector<Line>::iterator end() override {
		return lines.end();
	}
	//void DrawPoints(CPaintDC& dc, vector<Point>::iterator start, vector<Point>::iterator end) {
	//	for (auto i = start, i != end; i++) {
	//		dc.SetPixel(i->x, i->y, 0);
	//	}
	//}
private:
	vector<Line> lines;
};

struct LineToPointAdapter {
	typedef vector<Point> Points;
	LineToPointAdapter(Line& line) {
		int left = min(line.start.x, line.end.x);
		int right = max(line.start.x, line.end.x);
		int top = min(line.start.y, line.end.y);
		int bottom = max(line.start.y, line.end.y);
		int dx = right - left;
		int dy = line.end.y - line.start.y;
		// we only support vertical or horizontal lines
		if (dx == 0) {
			//vertical
			for (int y = top; y <= bottom; ++y) {
				points.emplace_back(Point{ left,y });
			}
		}
		else if (dy == 0) {
			// horizontal
			for (int x = left; x <= right; ++x) {
				points.emplace_back(Point{ x,top });
			}
		}
	};
	virtual Points::iterator begin() { return points.begin(); }
	virtual Points::iterator end() { return points.end(); }
private:
	Points points;
};

struct LineToPointCachingAdapter {
	typedef vector<Point> Points;
	LineToPointCachingAdapter(Line& line) {
		//static boost::hash<Line> hash;
		//line_hash = hash(line);//note: line_hash is a field!
		//if (cache.find(line_hash) != cache.end()) {
		//	return; // we already have it;
		//}
		Points points;


		//cache[line_hash] = points;
	}
	static map<size_t, Points> cache;
	//virtual Points::iterator begin() { return cache[line_hash].begin(); }
	//virtual Points::iterator end() { return cache[line_hash].end();; }
};

int main() {
	vector<Point> points;
	vector<shared_ptr<VectorObject>> vectorObjects{
		make_shared<VectorRectangle>(10,10,100,100),
		make_shared<VectorRectangle>(30,30,60,60)
	};
	for (auto& o : vectorObjects) {
		for (auto& l : *o) {
			LineToPointAdapter lpo{ l };
			for (auto& p : lpo) {
				points.push_back(p);
			}
		}
	}
	//DrawPoints(dc, points.begin(), points.end());
}
```

### 双向转换器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

template <typename TFrom, typename TTo> class Converter {
public:
	virtual TTo Convert(const TFrom& from) = 0;
	virtual TFrom ConvertBack(const TTo& to) = 0;
};

class IntToStringConverter : Converter<int, string> {
public:
	string Convert(const int& from) override {
		return to_string(from);
	}
	int ConvertBack(const string& to) override {
		int result;
		try {
			result = stoi(to);
		}
		catch (...) {
			return numeric_limits<int>::min();
		}
	}
};

int main() {
	IntToStringConverter converter;
	cout << converter.Convert(123) << "\n";//123
	cout << converter.ConvertBack("456") << "\n";//456
	cout << converter.ConvertBack("xyz") << "\n";//-2147483648
}
```

# 第七章桥接模式
### Pimpl模式
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

struct Person {
	string name;
	void greet();

	Person();
	~Person();
	// 前向声明（只是告诉编译器有这个类，但没有定义）
	class PersonImpl;
	PersonImpl* impl; // good place for gsl::owner<T>
};

struct Person::PersonImpl {
	void greet(Person* p);
};

Person::Person():impl(new PersonImpl){}
Person::~Person() { delete impl; }

void Person::greet() {
	impl->greet(this);
}

void Person::PersonImpl::greet(Person* p) {
	printf("hello %s", p->name.c_str());
}

int main() {

}
```

### 桥接模式介绍
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <stack>
using namespace std;

struct Renderer {
	virtual void render_circle(float x, float y, float radius) = 0;
};

struct VectorRenderer :Renderer {
	void render_circle(float x, float y, float radius) override {
		cout << "Rasterizing circle of radius " << radius << endl;
	}
};

struct RasterRenderer : Renderer {
	void render_circle(float x, float y, float radius) override {
		cout << "Drawing a vector circle of radius " << radius << endl;
	}
};

struct Shape {
protected:
	Renderer& renderer;
	Shape(Renderer& renderer) : renderer{renderer}{}
public:
	virtual void draw() = 0;
	virtual void resize(float factor) = 0;
};

struct Circle :Shape {
	float x, y, radius;
	Circle(Renderer& renderer, float x, float y, float radius) :Shape{ renderer }, x{ x }, y{y},radius{radius}{}

	void draw() override {
		renderer.render_circle(x, y, radius);
	}

	void resize(float factor) override {
		radius *= factor;
	}
};

int main() {
	RasterRenderer rr;
	Circle raster_circle{ rr,5,5,5 };
	raster_circle.draw();
	raster_circle.resize(2);
	raster_circle.draw();
}
```


# 第八章组合模式

### 支持数组形式的属性
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
using namespace std;

//class Value {
//public:
//	generator<int> operator()() {
//		co_yield 1;
//		co_yield 2;
//		co_yield 3;
//	}
//};

//template <typename T> class Contains {
//	virtual generator<T> operator()() = 0;
//};

//class Creature {
//	int strength, agility, intelligence;
//public:
//	int get_strength() const {
//		return strength;
//	}
//
//	void set_strength(int strength) {
//		Creature::strength = strength;
//	}
//	// other geeters and setters here
//
//	int sum() const {
//		return strength + agility + intelligence;
//	}
//
//	double average() const {
//		return sum() / 3.0;
//	}
//
//	int max() const {
//		return ::max({ strength,agility,intelligence });
//	}
//};

class Creature {
	enum Abilities{str,agl,intl,count};

	array<int, count> abilities;
	
	int get_strength() const {
		return abilities[str];
	}

	void set_strength(int value) {
		abilities[str] = value;
	}

	// same for other properties

	int sum() const {
		return (abilities.begin(), abilities.end(), 0);
	}

	double average() const {
		return sum() / (double)count;
	}

	int max() const {
		return *max_element(abilities.begin(), abilities.end());
	}
};



int main() {

}
```


### 组合图形对象
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
using namespace std;

struct GraphicObject {
	virtual void draw() = 0;
};

struct Circle :GraphicObject {
	// 通过 GraphicObject 继承
	void draw() override
	{
		cout << "Circle" << endl;
	}
};

struct Group : GraphicObject {
	string name;
	explicit Group(const string& name):name{name}{}
	

	// 通过 GraphicObject 继承
	void draw() override
	{
		cout << "Group " << name.c_str() << " constains: " << endl;
		for (auto&& o : objects) {
			o->draw();
		}
	}
	vector<GraphicObject*> objects;

};


int main() {
	Group root("root");
	Circle c1, c2;
	root.objects.push_back(&c1);

	Group subgroup("sub");
	subgroup.objects.push_back(&c2);

	root.objects.push_back(&subgroup);
	root.draw();
}
```

### 神经网络
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
using namespace std;

//struct Neuron {
//	vector<Neuron*> in, out;
//	unsigned int id;
//	Neuron() {
//		static int id = 1;
//		this->id = id++;
//	}
//
//	void connect_to(Neuron& other) {
//		out.push_back(&other);
//		other.in.push_back(this);
//	}
//};
//
//struct NeuronLayer :vector<Neuron> {
//	NeuronLayer(){}
//
//	NeuronLayer(int count) {
//		while (count-- > 0) {
//			emplace_back(Neuron{});
//		}
//	}
//
//	//Neuron* begin() {
//	//	return this;
//	//}
//
//	//Neuron* end() {
//	//	return this + 1;
//	//}
//};

template <typename Self>
struct SomeNeurons {
	template <typename T> void connect_to(T& other) {
		for (Neuron& from : *static_cast<Self*>(this)){
			for (Neuron& to : other) {
				from.out.push_back(&to);
				to.in.push_back(&from);
			}
		}
	}

	virtual begin() = 0;
	virtual end() = 0;
};


template <typename T> class Scalar : public SomeNeurons<T>
{
public:
	T* begin() { return reinterpret_cast<T*> (this); }
	T* end() { return reinterpret_cast<T*>(this); }
};

class Neuron : public Scalar<Neuron> {
	//as before
};

template <typename T> concept Iterable = {
	requires(T& t) {
	t.begin();
	t.end();
} || requires(T & t) {
	begin(t);
	end(t);
}
}

template <Iterable Self> // <-- a nightmare
struct SomeNeurons {
	template <Iterable T> // <-- okay
	void connect_to(T& other) {
		//as before
	}
};

template <Iterable T> class Scalar : public SomeNeurons<T> {
	// as before
};

struct Neuron : Scalar<Neuron>{};

struct Neuron : Scalar<Neuron>,SomeNeurons<Neuron>{};


template <Iterable T> ConnectionProxy<T> operator--(T&& item, int) {
	return ConnectionProxy<T>{item};
}

template <Iterable T> class ConnectionProxy {
	T& item;
public:
	explicit ConnectionProxy(T& item) : item{item}{}
	template <Iterable U> void operator>(U& other) {
		for (Neuron& from : item) {
			for (Neuron& to : other) {
				from.out.push_back(&to);
				to.in.push_back(&from);
			}
		}
	}
};

int main() {
	//Neuron neuron, neuron2;
	//NeuronLayer layer, layer2;
	//neuron.connect_to(neuron2);
	//neuron.connect_to(layer);
	//layer.connect_to(neuron);
	//layer.connect_to(layer2);

	Neuron n1, n2;
	n1--> n2;
}
```

### 组合模式的规范
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
using namespace std;

template <typename T> struct CompositeSpecification : Specification<T>
{
protected:
	vector<unique_ptr<Specification<T>>> specs;
	template<typename... Specs> CompositeSpecification(Specs... specs) {
		this->specs.reserve(Sizeof...(Specs));
		(this->specs.push_back(make_unique<Specs>(move(specs))),...);
	}

public:
	virtual bool is_satisfied(T& item) const {};
};

template <typename T> struct AndSpecification :CompositeSpecification<T> {
	template<typename.. Specs> AndSpecification(Specs... specs):CompositeSpecification<T>{specs...}{}

	bool is_satisfied(T& item) const override {
		return all_of(this->specs.begin(), this->specs.end(), [=](const auto& s) {
			return s->is_satisfied(item);
			})
	}
};


int main() {
	//auto spec = AndSpecification<Prodect>{ green,large,cheap };
}
```

# 第九章装饰器模式
### 预想方案
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
#include <sstream>
using namespace std;

struct Shape {
	virtual string str() const = 0;
};

struct Circle :Shape {
	float radius;
	explicit Circle(const float radius) :radius{ radius } {}
	void resize(float factor) { factor *= factor; }

	// 通过 Shape 继承
	string str() const override
	{
		ostringstream oss;
		oss << "A circle of radius " << radius;
		return oss.str();
	}

}; //Square implementation omitted

int main() {

}
```

### 动态装饰器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
#include <sstream>
using namespace std;

struct Shape {
	virtual string str() const = 0;
};

struct Circle :Shape {
	float radius;
	explicit Circle(const float radius) :radius{ radius } {}
	void resize(float factor) { factor *= factor; }

	// 通过 Shape 继承
	string str() const override
	{
		ostringstream oss;
		oss << "A circle of radius " << radius;
		return oss.str();
	}

}; //Square implementation omitted

struct ColoredShape : Shape {
	Shape& shape;
	string color;

	ColoredShape(Shape& shape, const string& color) :shape{ shape }, color{ color } {}
	string str() const override {
		ostringstream oss;
		oss << shape.str() << " has the color " << color<<"\n";
		return oss.str();
	}

	void make_dark() {
		if (constexpr auto dark = "dark "; !color.starts_with(dark)) {
			color.insert(0, dark);
		}
	}
};

struct TransparentShape : Shape {
	Shape& shape;
	uint8_t transparency;

	TransparentShape(Shape& shape, const uint8_t transparency) :shape{ shape }, transparency{ transparency } {}
	string str() const override {
		ostringstream oss;
		oss << shape.str() << " has " << static_cast<float>(transparency) / 255.f * 100.f << "% transparency";
		return oss.str();
	}
};

int main() {
	//Circle circle{ 0.5f };
	//ColoredShape redCircle{ circle,"red" };
	//cout << redCircle.str();
	////A circle of  radius 0.5 has the color red
	//redCircle.make_dark();
	//cout << redCircle.str();
	//// A circle of radius 0.5 has the color dark red

	Circle c{ 23 };
	ColoredShape cs{ c,"green" };
	TransparentShape myCircle{ cs,64 };
	cout << myCircle.str();
	// A circle of radius 23 has the color green has 25.098%
	// transparency

}
```

### 静态装饰器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
#include <sstream>
using namespace std;

struct Shape {
	virtual string str() const = 0;
};

struct Circle :Shape {
	float radius;
	explicit Circle(const float radius) :radius{ radius } {}
	void resize(float factor) { factor *= factor; }

	// 通过 Shape 继承
	string str() const override
	{
		ostringstream oss;
		oss << "A circle of radius " << radius;
		return oss.str();
	}

}; //Square implementation omitted

template <typename T> struct ColoredShape :T {
	string color;
	string str() const override {
		ostringstream oss;
		oss << T::str() << " has the color " << color;
		return oss.str();
	}
}; // implementation of TransparentSahpe<T> omitted

// 确保模板参数T继承自Shape
template <typename T> struct ColoredShape2 :T {
	static_assert(is_base_of<Shape, T>, "Template argument must be a Shape");
	// as before
};

template <typename T> struct TransparentShape :T {
	uint8_t transparency;
	template<typename...Args>
	TransparentShape(const uint8_t transparency,Args... args):T(std::forward<Args>(args)...), transparency{ transparency }{}

}; 

int main() {
	//ColoredShape<TransparentShape<Circle>> circle{ "blue" };
	//ColoredShape<TransparentShape<Square>> sq{ "red",51,5 };
	//cout << sq.str();
	//// A square with side 5 has 20% transparency has the color red
}
```

### 函数装饰器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <numeric>
#include <sstream>
using namespace std;

struct Shape {
	virtual string str() const = 0;
};

template <typename T>
struct Logger3;  // 只声明，作为占位

struct Circle :Shape {
	float radius;
	explicit Circle(const float radius) :radius{ radius } {}
	void resize(float factor) { factor *= factor; }

	// 通过 Shape 继承
	string str() const override
	{
		ostringstream oss;
		oss << "A circle of radius " << radius;
		return oss.str();
	}

}; //Square implementation omitted

template <typename T> struct ColoredShape :T {
	string color;
	string str() const override {
		ostringstream oss;
		oss << T::str() << " has the color " << color;
		return oss.str();
	}
}; // implementation of TransparentSahpe<T> omitted

// 确保模板参数T继承自Shape
template <typename T> struct ColoredShape2 :T {
	static_assert(is_base_of<Shape, T>, "Template argument must be a Shape");
	// as before
};

template <typename T> struct TransparentShape :T {
	uint8_t transparency;
	template<typename...Args>
	TransparentShape(const uint8_t transparency,Args... args):T(std::forward<Args>(args)...), transparency{ transparency }{}

}; 

struct Logger {
	function<void()> func;
	string name;
	Logger(const function<void()>& fun, const string& name):func{func},name{name}{}

	void operator()() const {
		cout << "Entering " << name << "\n";
		func();
		cout << "Exiting " << name << "\n";
	}


};

double add(double a, double b) {
	cout << a << "+" << b << "=" << (a + b) << endl;
	return a + b;
}

template <typename Func>
struct Logger2 {
	Func func;
	string name;

	Logger2(const Func& func,const string& name):func{func},name{name}{}

	void operator()() const {
		cout << "Entering " << name << endl;
		func();
		cout << "Exiting " << name << endl;
	}
};

template <typename Func> auto make_logger2(Func func, const string& name) {
	return Logger2<Func>{func, name};
}



template <typename R, typename... Args>
struct Logger3 <R(Args...)> {
	Logger3(function<R(Args...)> func, const string& name) :func{ func }, name{ name } {}

	R operator() (Args ...args) {
		cout << "Entering " << name << endl;
		R result = func(args...);
		cout << "Exiting " << name << endl;
		return result;
	}
	function<R(Args ...)> func;
	string name;
};

template <typename R,typename... Args>
auto make_logger3(R(*func)(Args...), const string& name) {
	return Logger3<R(Args...)>(function<R(Args...)>(func), name);
}


int main() {
	//ColoredShape<TransparentShape<Circle>> circle{ "blue" };
	//ColoredShape<TransparentShape<Square>> sq{ "red",51,5 };
	//cout << sq.str();
	//// A square with side 5 has 20% transparency has the color red

	//Logger([]() { cout << "Hello\n"; },"HelloFunction")();
	//// Entering HelloFunction
	//// Hello 
	//// Exiting HelloFunction

	//auto call = make_logger2([]() {cout << "Hello!" << endl; },"HelloFunction");

	auto logged_add = make_logger3(add, "Add");
	auto result = logged_add(2, 3);
}
```

# 第十章外观模式
### 幻方生成器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct Generator {
	virtual vector<int> generate(const int count) const {
		vector<int> result(count);
		//generate(result.begin(), result.end(), [&]({
		//		return 1+rand() % 9;
		//	});
		return result;
	}
};

struct Splitter {
	vector<vector<int>> split(vector<vector<int>> array) const {
		// implementation omitted
	}
};

struct Verifier {
	bool verify(vector<vector<int>> array) const {
		if (array.empty()) return false;
		auto expected = accumulate(array[0].begin(), array[0].end(), 0);
		return all_of(array.begin(), array.end(), [=](auto& inner) {
			return accumulate(inner.begin(), inner.end(), 0) == expected;
			});
	}
};

//struct MagicSquareGenerator {
//	vector<vector<int>> generate(int size) {
//		Generator g;
//		Splitter s;
//		Verifier v;
//
//		vector<vector<int>> square;
//
//		do {
//			square.clear();
//			for (int i = 0; i < size; i++) {
//				square.emplace_back(g.generate(size));
//			}
//		} while (!v.verify(s.split(square)));
//		return square;
//	}
//	
//};

template <typename G=Generator,typename S= Splitter,typename V= Verifier>
struct MagicSquareGenerator {
	vector<vector<int>> generate(int size) {
		G g;
		S s;
		V v;
		// rest of code as before
	}
};

struct UniqueGenerator :Generator {
	vector<int> generate(const int count) const override {
		vector<int> result;
		do {
			result = Generator::generate(count);
		} while (set<int>(result.begin(), result.end()).size() != result.size());
		return result;
	}
};




int main() {
	MagicSquareGenerator<UniqueGenerator> gen;
	auto square = gen.generate(3);
}
```


### 构建贸易终端
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct Generator {
	virtual vector<int> generate(const int count) const {
		vector<int> result(count);
		//generate(result.begin(), result.end(), [&]({
		//		return 1+rand() % 9;
		//	});
		return result;
	}
};

struct Splitter {
	vector<vector<int>> split(vector<vector<int>> array) const {
		// implementation omitted
	}
};

struct Verifier {
	bool verify(vector<vector<int>> array) const {
		if (array.empty()) return false;
		auto expected = accumulate(array[0].begin(), array[0].end(), 0);
		return all_of(array.begin(), array.end(), [=](auto& inner) {
			return accumulate(inner.begin(), inner.end(), 0) == expected;
			});
	}
};

//struct MagicSquareGenerator {
//	vector<vector<int>> generate(int size) {
//		Generator g;
//		Splitter s;
//		Verifier v;
//
//		vector<vector<int>> square;
//
//		do {
//			square.clear();
//			for (int i = 0; i < size; i++) {
//				square.emplace_back(g.generate(size));
//			}
//		} while (!v.verify(s.split(square)));
//		return square;
//	}
//	
//};

template <typename G=Generator,typename S= Splitter,typename V= Verifier>
struct MagicSquareGenerator {
	vector<vector<int>> generate(int size) {
		G g;
		S s;
		V v;
		// rest of code as before
	}
};

struct UniqueGenerator :Generator {
	vector<int> generate(const int count) const override {
		vector<int> result;
		do {
			result = Generator::generate(count);
		} while (set<int>(result.begin(), result.end()).size() != result.size());
		return result;
	}
};


struct TableBuffer : Buffer {
	TableBuffer(vector<TableColumnSpec> spec, int totalHeight) {
	//
	}
	struct TableColumnSpec {
		string header;
		int width;
		enum class TableColumnAlignment {
			Left,
			Center,
			Right
		}alignment;
	};
};

struct Console {
	vector<Viewport*> viewports;
	Size charSize, gridSize;
	//
	Console(bool fullscreen, int char_width, int char_height, int width, int height, optional<Size> client_size) {
		// signle buffer and viewport created here
		// linked together and added to appropriate collections
		// image textures generated
		// grid size calculated depending on wherther ew want fullscreen mode 
	}

	Console(const ConsoleCreationParameters& ccp){/**/ }


};

struct ConsoleCreationParameters {
	optional<Size> client;
	int character_width{ 10 };
	int width{ 20 };
	int height{ 30 };
	bool fullscreen{ false };
	bool create_default_view_and_vuffer{ true };
};

int main() {
	MagicSquareGenerator<UniqueGenerator> gen;
	auto square = gen.generate(3);
}
```

# 第十一章享元模式

### 用户名问题
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

typedef uint16_t key;

struct User {
	User(const string& first_name, const string& last_name) :first_name{ add(first_name) }, last_name{ add(last_name) } {}

protected:
	key first_name, last_name;
	static boost::bimap<key, string> names;
	static key seed;
	static key add(const string& s) {}

	const string& get_first_name() const {
		return names.left.find(last_name)->second;
	}

	const string& get_last_name() const {
		return names.left.find(last_name)->second;
	}

	friend ostream& operator<<(ostream& os, const User& obj) {
		return os << "first_name: " << obj.get_first_name() << " last_name: " << obj.get_last_name();
	}
};

key User::add(const string& s) {
	auto it = names.right.find(s);
	if (it == names.right.end()) {
		// add it
		names.insert({ ++seed,s });
		return seed;
	}

	return it->second;
}

int main() {

}
```


### Boost.Flyweight
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

typedef uint16_t key;



struct User {
	User(const string& first_name, const string& last_name) :first_name{ add(first_name) }, last_name{ add(last_name) } {}

protected:
	key first_name, last_name;
	static boost::bimap<key, string> names;
	static key seed;
	static key add(const string& s);

	const string& get_first_name() const {
		return names.left.find(last_name)->second;
	}

	const string& get_last_name() const {
		return names.left.find(last_name)->second;
	}

	friend ostream& operator<<(ostream& os, const User& obj) {
		return os << "first_name: " << obj.get_first_name() << " last_name: " << obj.get_last_name();
	}
};

boost::bimap<key, string> User::names;
key User::seed = 0;

key User::add(const string& s) {
	auto it = names.right.find(s);
	if (it == names.right.end()) {
		// add it
		names.insert({ ++seed,s });
		return seed;
	}

	return it->second;
}

struct User2 {
	boost::flyweight<string> first_name, last_name;
	User2(const string& first_name,const string& last_name):first_name{first_name},last_name{last_name}{}
	
};



int main() {
	User2 john_doe{ "John","Doe" };
	User2 jane_doe{ "Jane","Doe" };
	// boolalpha 是 C++ 标准库中的一个 I / O 操作符，用于在 std::cout 或 std::cin 中格式化布尔值，让 true 和 false 以 字符串形式 而不是 1 和 0 进行输出或输入。
	cout << boolalpha << (&jane_doe.last_name.get() == &john_doe.last_name.get());
	//true
}
```

### 字符串的范围
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

typedef uint16_t key;



struct User {
	User(const string& first_name, const string& last_name) :first_name{ add(first_name) }, last_name{ add(last_name) } {}

protected:
	key first_name, last_name;
	static boost::bimap<key, string> names;
	static key seed;
	static key add(const string& s);

	const string& get_first_name() const {
		return names.left.find(last_name)->second;
	}

	const string& get_last_name() const {
		return names.left.find(last_name)->second;
	}

	friend ostream& operator<<(ostream& os, const User& obj) {
		return os << "first_name: " << obj.get_first_name() << " last_name: " << obj.get_last_name();
	}
};

boost::bimap<key, string> User::names;
key User::seed = 0;

key User::add(const string& s) {
	auto it = names.right.find(s);
	if (it == names.right.end()) {
		// add it
		names.insert({ ++seed,s });
		return seed;
	}

	return it->second;
}

struct User2 {
	boost::flyweight<string> first_name, last_name;
	User2(const string& first_name,const string& last_name):first_name{first_name},last_name{last_name}{}
	
};

class FormattedText {
	string plainText;
	bool* caps;
public:
	explicit FormattedText(const string& plainText) :plainText{ plainText } {
		caps = new bool[plainText.length()];
	}

	~FormattedText() {
		delete[] caps;
	}

	void capitalize(int start, int end) {
		for (int i = start; i <= end; i++) {
			caps[i] = true;
		}
	}

	friend ostream& operator<<(ostream& os, const FormattedText& obj) {
		string s;
		for (int i = 0; i < obj.plainText.length(); i++) {
			char c = obj.plainText[i];
			s += (obj.caps[i] ? toupper(c) : c);
		}
		return os << s;
	}
};

class BetterFormattedText {
public:
	struct TextRange {
		int start, end;
		bool capitalize{ false };
		// other options here,e.g. bold, italic, etc.
		// determine our range vovers a particular position
		bool covers(int position) const {
			return position >= start && position <= end;
		}
	};

	TextRange& get_range(int start, int end) {
		formatting.emplace_back(TextRange{ start,end });
		return *formatting.rbegin();
	}

	friend ostream& operator<<(ostream& os, const BetterFormattedText& obj) {
		string s;
		for (size_t i = 0; i < obj.plain_text.length(); i++) {
			auto c = obj.plain_text[i];
			for (const auto& rng : obj.formatting) {
				if (rng.covers(i) && rng.capitalize) {
					c = toupper(c);

				}
				s += c;
			}
		}
		return os << s;
	}

	BetterFormattedText(const string& plain_text):plain_text{plain_text}{}
private: 
	string plain_text;
	vector<TextRange> formatting;
};


int main() {
	BetterFormattedText bft("This is a brave new world");
	bft.get_range(10, 15).capitalize = true;
	cout << bft;// This is a BRAVE new world
}
```

# 第十二章代理模式
### 智能指针
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	void deposit(int amount) {

	}
};


int main() {
	BankAccount* ba = new BankAccount;
	ba->deposit(123);
	auto ba2 = make_shared<BankAccount>();
	ba2->deposit(123); // same API


}
```

### 属性代理
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	void deposit(int amount) {

	}
};

template<typename T> struct Property {
	T value;
	Property(const T initial_value) {
		*this = initial_value; //invokes operator =
	}

	operator T() {
		// perform some getter action
		return value;
	}

	T operator =(T new_value) {
		// perform some setter action
		return value = new_value;
	}
};

struct Creature {
	Property<int> strength{ 10 };
	Property<int> agility{ 5 };
};

int main() {
	Creature creatrue;
	creatrue.agility = 20; // calls Property<int>::operator=
	auto  x = creatrue.strength;// calls Property<int>::operator T

}
```

### 虚拟代理
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	void deposit(int amount) {

	}
};

template<typename T> struct Property {
	T value;
	Property(const T initial_value) {
		*this = initial_value; //invokes operator =
	}

	operator T() {
		// perform some getter action
		return value;
	}

	T operator =(T new_value) {
		// perform some setter action
		return value = new_value;
	}
};

struct Creature {
	Property<int> strength{ 10 };
	Property<int> agility{ 5 };
};

struct Image {
	virtual void draw() = 0;
};

struct Bitmap :Image {
	string filename;
	Bitmap(const string& filename) {
		cout << "Loading image from " << filename << endl;
		//image gets loaded here
	}
	void draw() override {
		cout << "Drawing image " << filename << endl;
	}
};

struct LazyBitmap :Image {
private:
	Bitmap* bmp{ nullptr };
	string filename;
public:
	LazyBitmap(const string& filename):filename(filename){}
	~LazyBitmap() { delete bmp; }
	void draw() override {
		if (!bmp) {
			bmp = new Bitmap(filename);

		}
		bmp->draw();
	}
};

void draw_image(Image& img) {
	cout << "About to draw the image" << endl;
	img.draw();
	cout << "Done drawing the image" << endl;
}

int main() {
	//Bitmap img{ "pokemon.png" };//Loading image from pokemon.png

	LazyBitmap img{ "pokemon.png" };
	draw_image(img); // image loaded here

	//About to draw the image
	//Loading image from pokemon.png
	//Drawing image
	//Done drawing the image
}
```

### 通信代理

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	void deposit(int amount) {

	}
};

template<typename T> struct Property {
	T value;
	Property(const T initial_value) {
		*this = initial_value; //invokes operator =
	}

	operator T() {
		// perform some getter action
		return value;
	}

	T operator =(T new_value) {
		// perform some setter action
		return value = new_value;
	}
};

struct Creature {
	Property<int> strength{ 10 };
	Property<int> agility{ 5 };
};

struct Image {
	virtual void draw() = 0;
};

struct Bitmap :Image {
	string filename;
	Bitmap(const string& filename) {
		cout << "Loading image from " << filename << endl;
		//image gets loaded here
	}
	void draw() override {
		cout << "Drawing image " << filename << endl;
	}
};

struct LazyBitmap :Image {
private:
	Bitmap* bmp{ nullptr };
	string filename;
public:
	LazyBitmap(const string& filename):filename(filename){}
	~LazyBitmap() { delete bmp; }
	void draw() override {
		if (!bmp) {
			bmp = new Bitmap(filename);

		}
		bmp->draw();
	}
};

void draw_image(Image& img) {
	cout << "About to draw the image" << endl;
	img.draw();
	cout << "Done drawing the image" << endl;
}

struct Pingable {
	virtual wstring ping(const wstring& message) = 0;

};

struct Pong : Pingable {
	wstring ping(const wstring& message) override {
		return message + L" pong";
	}
};

void tryit(Pingable& pp) {
	wcout << pp.ping(L"ping") << "\n";
}

//[Route("api/[controller]")]
//public class PingPongController : Controller {
//	[HttpGet("{msg}")]
//	public string Get(string msg) {
//		return msg + " pong";
//	}
//}; // achievement unlocked: use C# in a C++ book

struct RemotePong : Pingable {
	wstring result;
	http_client client(U("http://localhost:9149/"));
	uri_builder builder(U("/api/pingpong/"));
	builder.append(message);
	pplx::task<wstring> task = client.request(
		methods::Get, builder.to_string()
	).then([=](http_response r)) {
		return r.extract_string();
	};
	task.wait();
	return task.get();
};

int main() {
	Pong pp;
	for (int i = 0; i < 3; i++) {
		tryit(pp);
	}

	RemotePong pp2;
	for (int i = 0; i < 3; i++) {
		tryit(pp2);
	}
}
```

### 值代理
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	void deposit(int amount) {

	}
};

template<typename T> struct Property {
	T value;
	Property(const T initial_value) {
		*this = initial_value; //invokes operator =
	}

	operator T() {
		// perform some getter action
		return value;
	}

	T operator =(T new_value) {
		// perform some setter action
		return value = new_value;
	}
};

struct Creature {
	Property<int> strength{ 10 };
	Property<int> agility{ 5 };
};

struct Image {
	virtual void draw() = 0;
};

struct Bitmap :Image {
	string filename;
	Bitmap(const string& filename) {
		cout << "Loading image from " << filename << endl;
		//image gets loaded here
	}
	void draw() override {
		cout << "Drawing image " << filename << endl;
	}
};

struct LazyBitmap :Image {
private:
	Bitmap* bmp{ nullptr };
	string filename;
public:
	LazyBitmap(const string& filename):filename(filename){}
	~LazyBitmap() { delete bmp; }
	void draw() override {
		if (!bmp) {
			bmp = new Bitmap(filename);

		}
		bmp->draw();
	}
};

void draw_image(Image& img) {
	cout << "About to draw the image" << endl;
	img.draw();
	cout << "Done drawing the image" << endl;
}

struct Pingable {
	virtual wstring ping(const wstring& message) = 0;

};

struct Pong : Pingable {
	wstring ping(const wstring& message) override {
		return message + L" pong";
	}
};

void tryit(Pingable& pp) {
	wcout << pp.ping(L"ping") << "\n";
}

//[Route("api/[controller]")]
//public class PingPongController : Controller {
//	[HttpGet("{msg}")]
//	public string Get(string msg) {
//		return msg + " pong";
//	}
//}; // achievement unlocked: use C# in a C++ book

struct RemotePong : Pingable {
	wstring result;
	http_client client(U("http://localhost:9149/"));
	uri_builder builder(U("/api/pingpong/"));
	builder.append(message);
	pplx::task<wstring> task = client.request(
		methods::Get, builder.to_string()
	).then([=](http_response r)) {
		return r.extract_string();
	};
	task.wait();
	return task.get();
};

template <typename T> struct Value {
	virtual operator T() const = 0;
};

template <typename T> struct Const : Value<T> {
	const T v;
	Const() : v{} {}
	Const (T v): v{v}{}

	operator T() const override {
		return v;
	}
};

template <typename T> struct OneOf : Value<T> {
	vector<T> values;
	OneOf() : values{ {T{}} } {}
	OneOf(initializer_list<T> values):values{values}{}

	operator T() const override {
		return values[rand() % values.size()];
	}
};

void draw_ui(const Value<bool>& use_dark_theme) {
	if (use_dark_theme) {
		cout << "Using dark theme\n";
	}
	else {
		cout << "Using normal theme\n";
	}
}

void draw_ui(bool use_dark_theme) {
	if (use_dark_theme) {
		cout << "Using dark theme\n";
	}
	else {
		cout << "Using normal theme\n";
	}
}

int main() {
	const Const<int> life{ 42 };
	// 因为Const类的构造函数不是显示的，可以隐式转换为int
	cout << life / 2 << "\n";//21

	OneOf<int> stuff{ 1,3,5 };
	cout << stuff << "\n";// will pring 1,3 or 5

	OneOf<bool> dark{ true,false };
	draw_ui(dark);
}
```

# 第十三章责任链模式
### 预想方案
```cpp
struct Creature {
	string name;
	int attack, defense;
	// constructor and << here
};
```

### 指针链
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct Creature {
	string name;
	int attack, defense;
	// constructor and << here

	friend ostream& operator <<(ostream& os,const Creature& obj){
		return os << obj.name << " attack: " << to_string(obj.attack) << " defense:" << to_string(obj.defense) << endl;
	}
};

class CreatureModifier {
	CreatureModifier* next{ nullptr };
protected:
	Creature& creature; // alternative:pointer or shared_ptr
public:
	explicit CreatureModifier(Creature& creature):creature(creature){}
	void add(CreatureModifier* cm) {
		if (next) next->add(cm);
		else next = cm;
	}

	virtual void handle() {
		if (next) next->handle();// critical!
	}
};

class DoubleAttackModifier :public CreatureModifier {
public:
	explicit DoubleAttackModifier(Creature& creature):CreatureModifier(creature){}

	void handle() override{
		creature.attack += 2;
		CreatureModifier::handle();
	}
};

class IncreaseDefenseModifier : public CreatureModifier {
public:
	explicit IncreaseDefenseModifier(Creature& creature):CreatureModifier(creature){}
	void handle() override {
		if (creature.attack <= 2) creature.defense += 1;
		CreatureModifier::handle();
	}
};

class NoBonusersModifier : public CreatureModifier {
public:
	explicit NoBonusersModifier(Creature& creature):CreatureModifier(creature){}

	void handle() override {
		// nothing here
	}
};

int main() {
	Creature goblin{ "Goblin",1,1 };
	CreatureModifier root{ goblin };
	DoubleAttackModifier r1{ goblin };
	DoubleAttackModifier r1_2{ goblin };
	IncreaseDefenseModifier r2{ goblin };

	root.add(&r1);
	root.add(&r1_2);
	root.add(&r2);

	root.handle();
	cout << goblin << endl;
}
```


### 代理链
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct Creature {
	string name;
	int attack, defense;
	Game& game;
	// constructor and << here

	Creature(Game& game) :game{game}{}

	friend ostream& operator <<(ostream& os,const Creature& obj){
		return os << obj.name << " attack: " << to_string(obj.attack) << " defense:" << to_string(obj.defense) << endl;
	}

	int get_attack() const {
		Query q{ name,Query::Argument::attack,attack };
		//game.queries(q);
		return q.result;
	}
};

class CreatureModifier {
	CreatureModifier* next{ nullptr };
protected:
	Game& game;
	Creature& creature; // alternative:pointer or shared_ptr
public:
	CreatureModifier(Game& game,Creature& creature):game(game),creature{creature}{}
	// explicit CreatureModifier(Creature& creature):creature(creature){}
	void add(CreatureModifier* cm) {
		if (next) next->add(cm);
		else next = cm;
	}

	virtual void handle() {
		if (next) next->handle();// critical!
	}
};

class DoubleAttackModifier :public CreatureModifier {
public:
	//connection conn;
	// explicit DoubleAttackModifier(Creature& creature):CreatureModifier(creature){}

	DoubleAttackModifier(Game& game, Creature& creature) :CreatureModifier(game, creature) {
		//conn = game.queries.connect([&](Query& q) {
		//	if (q.creature_name == creature.name && q.argument == Query::argument::attack)
		//	{
		//		q.result *= 2;
		//	}
		//	})
	}
	//~DoubleAttackModifier() { conn.disconnect(); }
	void handle() override{
		creature.attack += 2;
		CreatureModifier::handle();
	}
};

class IncreaseDefenseModifier : public CreatureModifier {
public:
	explicit IncreaseDefenseModifier(Creature& creature):CreatureModifier(creature){}
	void handle() override {
		if (creature.attack <= 2) creature.defense += 1;
		CreatureModifier::handle();
	}
};

class NoBonusersModifier : public CreatureModifier {
public:
	explicit NoBonusersModifier(Creature& creature):CreatureModifier(creature){}

	void handle() override {
		// nothing here
	}
};

struct Query {
	string creature_name;
	enum Argument{attack,defense} argument;
	int result;
};

struct Game // mediator
{
	// boost::signals2<void(Query&)> queries;
};

int main() {
	//Creature goblin{ "Goblin",1,1 };
	//CreatureModifier root{ goblin };
	//DoubleAttackModifier r1{ goblin };
	//DoubleAttackModifier r1_2{ goblin };
	//IncreaseDefenseModifier r2{ goblin };

	//root.add(&r1);
	//root.add(&r1_2);
	//root.add(&r2);

	//root.handle();
	//cout << goblin << endl;

	Game game;
	Creature goblin{ game,"Strong Goblin",2,2 };
	cout << goblin;
	{
		DoubleAttackModifier dam{ game,goblin };
		cout << goblin;
	}
	cout << goblin;
}
```

# 第十四章命令模式
### 预想方案
```cpp
struct BankAccount {
	int balance = 0;
	int overdraft_limit = -500;

	void deposit(int amount) {
		balance += amount;
		cout << "deposited " << amount << ",balance is now " << balance << "\n";
	}

	void withdraw(int amount) {
		if (balance = amount >= overdraft_limit) {
			balance -= amount;
			cout << "withdraw " << amount << ",balance is now " << balance << "\n";
		}
	}
};
```


### 实现命令模式
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	int balance = 0;
	int overdraft_limit = -500;

	void deposit(int amount) {
		balance += amount;
		cout << "deposited " << amount << ",balance is now " << balance << "\n";
	}

	void withdraw(int amount) {
		if (balance = amount >= overdraft_limit) {
			balance -= amount;
			cout << "withdraw " << amount << ",balance is now " << balance << "\n";
		}
	}
};

struct Command {
	virtual void call() const = 0;
};

struct BankAccountCommand : Command {
	BankAccount& account;
	enum Action { deposit, withdraw } action;
	int amount;

	BankAccountCommand(BankAccount& account, const Action action, const int amount) :account(account), action(action), amount(amount) {}

	void call() const override {
		switch (action) {
		case deposit:
			account.deposit(amount);
			break;
		case withdraw:
			account.withdraw(amount);
			break;
		}
	}
};

int main() {
	BankAccount ba;
	BankAccountCommand  cmd{ ba,BankAccountCommand::deposit,100 };
	//Command* cmd = new BankAccountCommand(ba, BankAccountCommand::deposit, 100);
	cmd.call();

	return 1;
}
```


### 撤销操作
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	int balance = 0;
	int overdraft_limit = -500;

	void deposit(int amount) {
		balance += amount;
		cout << "deposited " << amount << ",balance is now " << balance << "\n";
	}

	bool withdraw(int amount) {
		if (balance - amount >= overdraft_limit) {
			balance -= amount;
			cout << "withdraw " << amount << ",balance is now " << balance << "\n";
			return true;
		}
		return false;
	}
};

struct Command {
	virtual void call() = 0;
	virtual void undo() = 0;
};

struct BankAccountCommand : Command {
	BankAccount& account;
	enum Action { deposit, withdraw } action;
	int amount;
	bool withdrawal_succeeded;

	BankAccountCommand(BankAccount& account, const Action action, const int amount) :account(account), action(action), amount(amount), withdrawal_succeeded{ false } {}

	void call() override {
		switch (action) {
		case deposit:
			account.deposit(amount);
			break;
		case withdraw:
			account.withdraw(amount);
			withdrawal_succeeded = account.withdraw(amount);
			break;
		}
	}

	void undo() override {
		switch (action) {
		case withdraw:
			if (withdrawal_succeeded) {
				account.deposit(amount);
			}
			break;
		case deposit:
			account.withdraw(amount);
			break;
		}
		
	}
};

int main() {

}
```

### 复合命令
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

struct BankAccount {
	int balance = 0;
	int overdraft_limit = -500;

	void deposit(int amount) {
		balance += amount;
		cout << "deposited " << amount << ",balance is now " << balance << "\n";
	}

	bool withdraw(int amount) {
		if (balance - amount >= overdraft_limit) {
			balance -= amount;
			cout << "withdraw " << amount << ",balance is now " << balance << "\n";
			return true;
		}
		return false;
	}
};

struct Command {
	virtual void call() = 0;
	virtual void undo() = 0;
};

struct BankAccountCommand : Command {
	BankAccount& account;
	enum Action { deposit, withdraw } action;
	int amount;
	bool withdrawal_succeeded;

	BankAccountCommand(BankAccount& account, const Action action, const int amount) :account(account), action(action), amount(amount), withdrawal_succeeded{ false } {}

	void call() override {
		switch (action) {
		case deposit:
			account.deposit(amount);
			break;
		case withdraw:
			account.withdraw(amount);
			withdrawal_succeeded = account.withdraw(amount);
			break;
		}
	}

	void undo() override {
		switch (action) {
		case withdraw:
			if (withdrawal_succeeded) {
				account.deposit(amount);
			}
			break;
		case deposit:
			account.withdraw(amount);
			break;
		}
		
	}
};

struct CompositeBankAccountCommand :vector<BankAccountCommand>, Command {
	CompositeBankAccountCommand(const initializer_list<value_type>& items):vector<BankAccountCommand>(items){}

	void call() override {
		for (auto& cmd : *this) {
			cmd.call();
		}
	}

	void undo() override {
		/*
		rbegin() 是 C++ 容器（如 vector、list、deque、set、map 等）的**反向迭代器（reverse iterator）**的起点。
		作用
		rbegin() 返回指向容器最后一个元素的反向迭代器。
		rend() 返回指向容器第一个元素之前位置的反向迭代器。
		for (auto it = rbegin(); it != rend(); ++it) 可以逆序遍历容器（从后往前）。
		*/
		for (auto it = rbegin(); it != rend(); it++) {
			it->undo();
		}
	}
};

struct MoneyTreansferCommand :CompositeBankAccountCommand {
	MoneyTreansferCommand(BankAccount& from, BankAccount& to, int amount) :CompositeBankAccountCommand{
		BankAccountCommand{from,BankAccountCommand::withdraw,amount},
		BankAccountCommand{to,BankAccountCommand::deposit,amount}
	}{}

	void call() override {
		bool ok = true;
		for (auto& cmd : *this) {
			if (ok) {
				cmd.call();
				ok = cmd.withdrawal_succeeded;

			}
			else {
				cmd.withdrawal_succeeded = false;
			}
		}
	}
};



int main() {

}
```

### 命令与查询分离
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

enum class CreatureAbility {
	strength,
	agility
};

struct CreatureCommand {
	enum Action { set, increaseBy, decreaseBy } action;
	CreatureAbility ability;
	int amount;
};

struct CreatureQuery {
	CreatureAbility ability;
};

class Creature {
	int strength, agility;
public:
	Creature(int strength,int agility):strength{strength},agility{agility}{}

	void process_command(const CreatureCommand& cc);
	int process_query(const CreatureQuery& q) const;

	void set_streangth(int value) {
		process_command(CreatureCommand{ CreatureCommand::set,CreatureAbility::strength,value });
	}

	int get_strength() const {
		return process_query(CreatureQuery{ CreatureAbility::strength });
	}
};



void Creature::process_command(const CreatureCommand& cc) {
	int* ability = nullptr;
	switch (cc.ability) {
	case CreatureAbility::strength:
		ability = &strength;
		break;
	case CreatureAbility::agility:
		ability = &agility;
		break;
	}

	switch (cc.action) {
	case CreatureCommand::set:
		*ability = cc.amount;
		break;
	case CreatureCommand::increaseBy:
		*ability += cc.amount;
		break;
	case CreatureCommand::decreaseBy:
		*ability = cc.amount;
		break;
	}
}

int Creature::process_query(const CreatureQuery& q)const {
	switch (q.ability) {
		case CreatureAbility::strength: 
			return strength;
		case CreatureAbility::agility:
			return agility;
	}
	return 0;
}


int main() {

}
```

# 第十五章解释器模式
### 解析整数
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;


int better_atoi(const char* str)
{
    int val{ 0 };
    while (*str) {
        // 先*str ,再-'0'，变成数字
        // 然后+val*10,本质就是将str->int
        val = val * 10 + (*str++ - '0');
    }
    return val;
}

int main() {
}

```

### 数值表达式求值
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;


int better_atoi(const char* str)
{
    int val{ 0 };
    while (*str) {
        // 先*str ,再-'0'，变成数字
        // 然后+val*10,本质就是将str->int
        val = val * 10 + (*str++ - '0');
    }
    return val;
}

struct Token {
    enum Type{integer,plus,minus,lparen,rparen} type;
    string text;
    explicit Token(Type type,const string& text):type{type},text{text}{}

    friend ostream& operator<<(ostream& os, const Token& obj) {
        return os << "`" << obj.text << "`";
    }
};

vector<Token> lex(const string& input) {
    vector<Token> result;
    for (int i = 0; i < input.size(); i++) {
        switch (input[i]) {
        case '+':
            result.emplace_back(Token::plus, "+");
            break;
        case '-':
            result.emplace_back(Token::minus, "-");
            break;
        case '(':
            result.emplace_back(Token::lparen, "(");
            break;
        case ')':
            result.emplace_back(Token::rparen, ")");
            break;
        default:
            cout << "default" << endl;
        }

        // map<BinaryOperation::Type,char>
        ostringstream buffer;
        buffer << input[i];
        for (int j = i + 1; j < input.size(); ++j) {
            if (isdigit(input[j])) {
                buffer << input[j];
                ++i;
            }
            else {
                result.emplace_back(Token::integer, buffer.str());
                buffer.str("");
                break;
            }
        }
        if (auto str = buffer.str(); str.length() > 0) {
            result.emplace_back(Token::integer, str);
        }
    }

}

struct Element {
    virtual int eval() const = 0;

};

struct Integer :Element {
    int value;
    explicit Integer(const int value):value{value}{}
    int eval() const override { return value; }
};

struct BinaryOperation : Element {
    enum Type{addition,subtraction} type;
    shared_ptr<Element> lhs, rhs;
    int eval() const override {
        if (type == addition) {
            return lhs->eval() + rhs->eval();
        }
        return lhs->eval() - rhs->eval();
    }
};

shared_ptr<Element> parse(const vector<Token>& tokens) {
    auto result = make_unique<BinaryOperation>();
    bool have_lhs = false;//this will need some explaining:)
    for (size_t i = 0; i < tokens.size(); i++) {
        auto token = tokens[i];
        switch (token.type) {
            // process each of the tokens in turn
        case Token::integer: {
            int value = boost::lexical_cast<int>(token.text);
            auto integer = make_shared<Integer>(value);
            if (!have_lhs) {
                result->lhs = integer;
                have_lhs = true;
            }
            else {
                result->rhs = integer;
            }
        }
        case Token::plus:
            result->type = BinaryOperation::addition;
            break;
        case Token::minus:
            result->type = BinaryOperation::subtraction;
            break;
        case Token::lparen:
            int j = i;
            for (; j < tokens.size(); j++) {
                if (tokens[j].type == Token::rparen)
                    break;//found it!
                vector<Token> subexpression(&tokens[i + 1], &tokens[j]);
                auto element = parse(subexpression);
                if (!have_lhs) {
                    result->lhs = element;
                    have_lhs = true;
                }
                else {
                    result->rhs = element;
                }
                i = j;//advance
            }
        }
    }
    return result;
}

int main() {

    string  input{ "(13-4)-(12+1)" };
    auto tokens = lex(input);
    auto parsed = parse(tokens);
    cout << input << " = " << parsed->eval() << endl;
    // prints "(13-4)-(12+1)=-4"
}

```
### 使用Boost.Spirit解析
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;


int better_atoi(const char* str)
{
    int val{ 0 };
    while (*str) {
        // 先*str ,再-'0'，变成数字
        // 然后+val*10,本质就是将str->int
        val = val * 10 + (*str++ - '0');
    }
    return val;
}

struct Token {
    enum Type{integer,plus,minus,lparen,rparen} type;
    string text;
    explicit Token(Type type,const string& text):type{type},text{text}{}

    friend ostream& operator<<(ostream& os, const Token& obj) {
        return os << "`" << obj.text << "`";
    }
};

vector<Token> lex(const string& input) {
    vector<Token> result;
    for (int i = 0; i < input.size(); i++) {
        switch (input[i]) {
        case '+':
            result.emplace_back(Token::plus, "+");
            break;
        case '-':
            result.emplace_back(Token::minus, "-");
            break;
        case '(':
            result.emplace_back(Token::lparen, "(");
            break;
        case ')':
            result.emplace_back(Token::rparen, ")");
            break;
        default:
            cout << "default" << endl;
        }

        // map<BinaryOperation::Type,char>
        ostringstream buffer;
        buffer << input[i];
        for (int j = i + 1; j < input.size(); ++j) {
            if (isdigit(input[j])) {
                buffer << input[j];
                ++i;
            }
            else {
                result.emplace_back(Token::integer, buffer.str());
                buffer.str("");
                break;
            }
        }
        if (auto str = buffer.str(); str.length() > 0) {
            result.emplace_back(Token::integer, str);
        }
    }

}

struct Element {
    virtual int eval() const = 0;

};

struct Integer :Element {
    int value;
    explicit Integer(const int value):value{value}{}
    int eval() const override { return value; }
};

struct BinaryOperation : Element {
    enum Type{addition,subtraction} type;
    shared_ptr<Element> lhs, rhs;
    int eval() const override {
        if (type == addition) {
            return lhs->eval() + rhs->eval();
        }
        return lhs->eval() - rhs->eval();
    }
};

shared_ptr<Element> parse(const vector<Token>& tokens) {
    auto result = make_unique<BinaryOperation>();
    bool have_lhs = false;//this will need some explaining:)
    for (size_t i = 0; i < tokens.size(); i++) {
        auto token = tokens[i];
        switch (token.type) {
            // process each of the tokens in turn
        case Token::integer: {
            int value = boost::lexical_cast<int>(token.text);
            auto integer = make_shared<Integer>(value);
            if (!have_lhs) {
                result->lhs = integer;
                have_lhs = true;
            }
            else {
                result->rhs = integer;
            }
        }
        case Token::plus:
            result->type = BinaryOperation::addition;
            break;
        case Token::minus:
            result->type = BinaryOperation::subtraction;
            break;
        case Token::lparen:
            int j = i;
            for (; j < tokens.size(); j++) {
                if (tokens[j].type == Token::rparen)
                    break;//found it!
                vector<Token> subexpression(&tokens[i + 1], &tokens[j]);
                auto element = parse(subexpression);
                if (!have_lhs) {
                    result->lhs = element;
                    have_lhs = true;
                }
                else {
                    result->rhs = element;
                }
                i = j;//advance
            }
        }
    }
    return result;
}

struct ast_element {
    virtual ~ast_element() = default;
    virtual void accept(ast_element_visitor& visitor) = 0;
};

struct ast_element_visitor{
};

struct type_specification :ast_element {
    void accept(ast_element_visitor& visitor) override {
        
    }
};

struct property : ast_element {
    vector<wstring> names;
    type_specification type;
    bool is_constatnt{ false };
    wstring default_value;

    void accept(ast_element_visitor& visitor) override {
        visitor.visit(*this);
    }
};

BOOST_FUSION_ADAPT_STRUCT(
    tlon::property,
    (vector<wstring>, names),
    (tlon::type_specificatioin, type),
    (bool, is_constant),
    (wstring, default_value)
)

typedef variant<function_body, property, function_signature> class_number;

template<typename TLanguagePrinter, typename Iterator>
wstring parse(Iterator first,Iterator last)
{
    using spirit::qi::phrase_pase;

    file f;
    file_paser<wstring::const_iterator> f{};
    auto b = phrase_paser(first, last, fp, space, f);
    if (b) {
        return TLanguagePrinter{}.pretty_print(f);
    }
    return wstring(L"FALL");
}

struct default_value_visitor :static_visitor<> {
    capp_printer& printer;
    explicit default_value_visitor(cpp_printer& printer):printer{printer}{}

    void operator() (const basic_type& bt) const {
        // for a scalar value,we just dump its default
        printer.buffer << printer.default_value_for(bt.name);
    }

    void opertor() (const tuple_signature& ts) const{
        for (auto& e : ts.elements) {
            this->operator()(e.type);
            printer.buffer << ", ";
        }
        printer.backtrack(2);
    }
};

int main() {

    class_declaration_rule %= lit(L"class ") >> +(alum) % '.'
        >> -(lit(L"(") >> -parameter_declaration_rule % ',' >> lit(")"))
        >> "{"
        >> *(function_body_rule | property_rule | function_signature_rule)
        >> "}";
}

```

# 第十六章迭代器模式
### 标准库中的迭代器
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;


int main() {
	vector<string> names{ "john","jane","jill","jack" };

	vector<string>::iterator it = names.begin();// begin(names)

	cout << "first name is " << *it << "\n";

	vector<string>::const_reverse_iterator jack = crbegin(names);
	//*jack += " reacher";
}
```

### 遍历二叉树
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
using namespace std;

template <typename T> struct Node {
	T value;
	Node<T>* left{ nullptr };
	Node<T>* right{ nullptr };
	Node<T>* parent{ nullptr };
	BinaryTree<T>* tree{ nullptr };

	explicit Node(const T& value):value(value) {

	}

	Node(const T& value, Node<T>* const left, Node<T>* const right) :value{ value }, left{ left }, right{ right } {
		this->left->tree = this->right->tree = tree;
		this->left->parent = this->right->parent = this;
	}

	void set_tree(BinaryTree<T>* t) {
		tree = t;
		if (left) left->set_tree(t);
		if (right) right->set_tree(t);
	}
};

template <typename T> struct BinaryTree {
	Node<T>* root = nullptr;
	explicit BinaryTree(Node<T>* const root) :root{ root } {
		root->set_tree(this);
	}

	typedef PreOrderIterator<T> iterator;
	iterator begin() {
		Node<T>* n = root;
		if (n) {
			while (n->left) {
				n = n->left;
			}
			return iterator{ n };
		}
	}

	iterator end() {
		return iterator{ nullptr };
	}

public:
	class pre_order_traversal {
		BinaryTree<T>& tree;
	public:
		pre_order_traversal(BinaryTree<T>& tree) :tree{ tree } {}
		iterator begin() { return tree.begin(); }
		iterator end() { return tree.end(); }
	}pre_order;

};

template <typename U>
struct PreOrderIterator {
public:
	Node<U>* current;
	explicit PreOrderIterator(Node<U>* current):current{current}{}

	bool operator!=(const PreOrderIterator<U>& other) {
		return current != other.current;
	}

	Node<U>& operator*() { return *current; }

	PreOrderIterator<U>& opertor++() {
		if (current->right) {
			current = current->right;
			while (current->left) {
				current = current->left;
			}
		}
		else {
			Node<T>* p = current->parent;
			while (p && current == p->right) {
				current = p;
				p = p->next;
			}
			current = p;
		}
		return *this;
	}


};


int main() {
	BinaryTree<string> family{
		new Node<string>{"me",new Node<string>{"mother",new Node<string>{"mother's mother"},new Node<string>{"mother's ,father"}},
	new Node<string>{"father"}}
	};
	for (auto it = family.begin(); it != family.end(); it++) {
		cout << (*it)->value << "\n";
	}

	for (const auto& it : family.pre_order) {
		cout << it.value << "\n";
	}
}

```

### 使用协程的迭代
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

template <typename T> struct Node {
	T value;
	Node<T>* left{ nullptr };
	Node<T>* right{ nullptr };
	Node<T>* parent{ nullptr };
	BinaryTree<T>* tree{ nullptr };

	explicit Node(const T& value) :value(value) {

	}

	Node(const T& value, Node<T>* const left, Node<T>* const right) :value{ value }, left{ left }, right{ right } {
		this->left->tree = this->right->tree = tree;
		this->left->parent = this->right->parent = this;
	}

	void set_tree(BinaryTree<T>* t) {
		tree = t;
		if (left) left->set_tree(t);
		if (right) right->set_tree(t);
	}
};

template <typename T> struct BinaryTree {
	Node<T>* root = nullptr;
	explicit BinaryTree(Node<T>* const root) :root{ root } {
		root->set_tree(this);
	}

	typedef PreOrderIterator<T> iterator;
	iterator begin() {
		Node<T>* n = root;
		if (n) {
			while (n->left) {
				n = n->left;
			}
			return iterator{ n };
		}
	}

	iterator end() {
		return iterator{ nullptr };
	}

public:
	class pre_order_traversal {
		BinaryTree<T>& tree;
	public:
		pre_order_traversal(BinaryTree<T>& tree) :tree{ tree } {}
		iterator begin() { return tree.begin(); }
		iterator end() { return tree.end(); }
	}pre_order;

	generator<Node<T>*> post_order_impl(Node<T>* node) const {
		if (node) {
			for (auto x : post_order_impl(node->left)) {
				co_yield x;
			}
			for (auto y : post_order_impl(node->right)) {
				co_yield y;
			}
			co_yield node;
		}
	}
public:
	generator<Node<T>*> post_order() const {
		return post_order_impl(root);
	}

};

template <typename U>
struct PreOrderIterator {
public:
	Node<U>* current;
	explicit PreOrderIterator(Node<U>* current):current{current}{}

	bool operator!=(const PreOrderIterator<U>& other) {
		return current != other.current;
	}

	Node<U>& operator*() { return *current; }

	PreOrderIterator<U>& operator++() {
		if (current->right) {
			current = current->right;
			while (current->left) {
				current = current->left;
			}
		}
		else {
			Node<T>* p = current->parent;
			while (p && current == p->right) {
				current = p;
				p = p->next;
			}
			current = p;
		}
		return *this;
	}


};


int main() {
	BinaryTree<string> family{
		new Node<string>{"me",new Node<string>{"mother",new Node<string>{"mother's mother"},new Node<string>{"mother's ,father"}},
	new Node<string>{"father"}}
	};
	for (auto it = family.begin(); it != family.end(); it++) {
		cout << (*it).value << "\n";
	}

	for (const auto& it : family.pre_order) {
		cout << it.value << "\n";
	}

	for (auto it : family.post_order()) {
		cout << it->value<< endl;
	}
}
```

# 第十七章中介者模式
### 聊天室
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct Person {
	string name;
	ChatRoom* room{ nullptr };
	vector<string> chat_log;

	Person(const string& name);

	void receive(const string& origin, const string& message);
	void say(const string& message) const;
	void pm(const string& who, const string& message) const;
};

struct ChatRoom {
public:
	vector<Person*> people; // assume append-only
	void join(Person* p);
	void broadcast(const string& origin, const string& message);
	void message(const string& origin, const string& who, const string& message);
};

void ChatRoom::join(Person* p) {
	string join_msg = p->name + " joins the chat";
	broadcast("room", join_msg);
	p->room = this;
	people.push_back(p);
}

void ChatRoom::broadcast(const string& origin, const string& message) {
	for (auto p : people) {
		if (p->name != origin) {
			p->receive(origin, message);
		}
	}
}

void Person::say(const string& message) const {
	room->broadcast(name, message);
}

void Person::pm(const string& who, const string& message) const {
	room->message(name, who, message);
}

void Person::receive(const string& origin, const string& message) {
	string s{ origin + ": \"" + message + "\"" };
	cout << "[" << name << "'s chat session] " << s << "\n";
	chat_log.emplace_back(s);

}


int main() {
	ChatRoom room;
	Person john{ "john" };
	Person jane{ "jane" };
	room.join(&john);
	room.join(&jane);
	john.say("hi room");
	jane.say("oh, hey john");

	Person simon("simon");
	room.join(&simon);
	simon.say("hi everyone!");
	jane.pm("simon", "glad you could join us, simon");
}

```

### 中介者与事件
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct EventData {
	virtual ~EventData() = default;
	virtual void print() const = 0;
};

struct PlayerScoredData : EventData {
	string player_name;
	int goals_scored_so_far;

	PlayerScoredData(const string& player_name, const int goals_scored_so_far):player_name(player_name),goals_scored_so_far(goals_scored_so_far) {

	}

	void print() const override {
		cout << player_name << " has scored!(their " << goals_scored_so_far << " goal)" << "\n";
	}
};

struct Game {
	signal<void(EventData*) > events; // observer
};

struct Player {
	string name;
	int goals_scored = 0;
	Game& game;

	Player(const string& name, Game& game):name(name),game(game){}

	void score() {
		goals_scored++;
		PlayerScoredData ps{ name,goals_scored };
		game.events(&ps);
	}
};

struct Coach {
	Game& game;
	explicit Coach(Game& game) : game(game) {
		game.events.connect([](EventData* e) {
			PlayerScoredData* ps = dynamic_cast<PlayerScoredData*>(e);
			if (ps && ps->goals_scored_so_far < 3) {
				cout << "coach says: well done, " << ps->player_name << "\n";
			}
			});
	}
};

int main() {
	Game game;
	Player player{ "Sam",game };
	Coach coach{ game };

	player.score();
	player.score();
	player.score(); // ignore by coach

}

```

# 第十八章备忘录模式
### 银行账户
```cpp
class BankAccount {
	int balance = 0;

public:
	explicit BankAccount(const int balance):balance(balance){}
	Memento deposit(int amount) {
		balance += amount;
		return { balance };
	}

	void restore(const Memento& m) {
		balance = m.balance;
	}

	friend ostream& operator<<(ostream& os, const BankAccount& obj) {
		return os << "Balance: " << obj.balance << "\n";
	}
};

class Memento final {
public:
	int balance;
public:
	Memento(int balance):balance(balance){}
	friend class  BankAccount;
};

void memento() {
	BankAccount ba{ 100 };
	auto m1 = ba.deposit(50);
	auto m2 = ba.deposit(25);
	cout << ba << "\n";// Balance : 175

	// undo to m1
	ba.restore(m1);
	cout << ba << "\n";

	//redo 
	ba.restore(m2);
	cout << ba << "\n";

}

```


### 撤销功能与恢复功能
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

class BankAccount {
	int balance = 0;

public:
	explicit BankAccount(const int balance):balance(balance){}
	Memento deposit(int amount) {
		balance += amount;
		return { balance };
	}

	void restore(const Memento& m) {
		balance = m.balance;
	}

	friend ostream& operator<<(ostream& os, const BankAccount& obj) {
		return os << "Balance: " << obj.balance << "\n";
	}
};

class Memento final {
public:
	int balance;
public:
	Memento(int balance):balance(balance){}
	friend class  BankAccount;
};

void memento() {
	BankAccount ba{ 100 };
	auto m1 = ba.deposit(50);
	auto m2 = ba.deposit(25);
	cout << ba << "\n";// Balance : 175

	// undo to m1
	ba.restore(m1);
	cout << ba << "\n";

	//redo 
	ba.restore(m2);
	cout << ba << "\n";

}


class BankAccount2 { // supports undo/redo
	int balance = 0;
	vector<shared_ptr<Memento>> changes;
	int current;
public:
	explicit BankAccount2(const int balance) :balance(balance) {
		changes.emplace_back(make_shared<Memento>(balance));
		current = 0;
	}

	shared_ptr<Memento> deposit(int amount) {
		balance += amount;
		auto m = make_shared<Memento>(balance);
		changes.push_back(m);
		++current;
		return m;
	}

	void restore(const shared_ptr<Memento>& m) {
		if (m) {
			balance = m->balance;
			changes.push_back(m);
			current = changes.size() - 1;
		}
	}

	shared_ptr<Memento> undo() {
		if (current > 0) {
			--current;
			auto m = changes[current];
			balance = m->balance;
			return m;
		}
		return {};
	}

	shared_ptr<Memento> redo() {
		if (current + 1 < changes.size()) {
			++current;
			auto m = changes[current];
			balance = m->balance;
			return m;
		}
		return {};
	}

	friend ostream& operator<<(ostream& os, const BankAccount2& obj) {
		return os << "Balance: " << obj.balance << "\n";
	}
};

int main() {
	BankAccount2 ba{ 100 };
	ba.deposit(50);
	ba.deposit(25);
	cout << ba << "\n";
	ba.undo();
	cout << "Undo 1: " << ba << "\n";
	ba.undo();
	cout << "Undo 2: " << ba << "\n"; // Undo 2: 100
	ba.redo();
	cout << "Redo 2: " << ba << "\n"; // Redo 2: 150
	ba.undo();// back to 100 again

}

```

### 内存注意事项
```cpp
class BankAccount3 {
	boost::circular_buffer<shared_ptr<Memento>> changes{ 5 };
	// as before
};  
```

# 第十九章空对象模式
### 预想方案
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct Logger {
	virtual ~Logger() = default;
	virtual void info(const string& s) = 0;
	virtual void warn(const string& s) = 0;
};

class BankAccount {
	shared_ptr<Logger> log;
public:
	string name;
	int balance = 0;
	BankAccount(const shared_ptr<Logger>& logger, const string& name,int balance):log{logger},name{name},balance{balance}{}
	// more members here

	void deposit(int amount) {
		balance += amount;
		log->info("deposited $" + to_string(amount) + " to " + name + ",balance is now $" + to_string(balance));
	}
};

struct ConoleLogger : Logger {
	void info(const string& s) override {
		cout << "INFO: " << s << endl;
	}
	void warn(const string& s) override {
		cout << "WARNING!!!" << s << endl;
	}
};

struct NullLogger final : Logger {
	void info(const string& s) override {}
	void warn(const string& s) override {}
};

int main() {
	

}

```

### 隐式空对象
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct Logger {
	virtual ~Logger() = default;
	virtual void info(const string& s) = 0;
	virtual void warn(const string& s) = 0;
};

class BankAccount {
	shared_ptr<Logger> log;
public:
	string name;
	int balance = 0;
	static shared_ptr<Logger> no_logging;
	shared_ptr<OptionalLogger> logger;

	BankAccount(const shared_ptr<Logger>& logger, const string& name,int balance):log{logger},name{name},balance{balance}{}
	// more members here
	BankAccount(const string& name, int balance, const shared_ptr<Logger>& logger = no_logging) :log{make_shared<OptionalLogger>(logger)}, name{ name }, balance{ balance } {}


	void deposit(int amount) {
		balance += amount;
		log->info("deposited $" + to_string(amount) + " to " + name + ",balance is now $" + to_string(balance));
	}
};

struct ConoleLogger : Logger {
	void info(const string& s) override {
		cout << "INFO: " << s << endl;
	}
	void warn(const string& s) override {
		cout << "WARNING!!!" << s << endl;
	}
};

struct NullLogger final : Logger {
	void info(const string& s) override {}
	void warn(const string& s) override {}
};

struct OptionalLogger :Logger {
	shared_ptr<Logger> impl;
	static shared_ptr<Logger> no_logging;
	OptionalLogger(const shared_ptr<Logger>& logger): impl{logger}{}
	virtual void info(const string& s) override {
		if (impl) impl->info(s); // null check here
	}
	// and similar checks for other members
};

shared_ptr<Logger> BankAccount::no_logging;

int main() {
	BankAccount account{ "primary account",1000 };
	account.deposit(2000); // no crash

}

```

# 第二十章观察者模式
### 属性观察器
```cpp
struct Person {
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		age = value;
	}
private:
	int age;
};

```

### Observer\<T\>
```cpp
struct Person {
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		age = value;
	}
private:
	int age;
};

struct PersonListener {
	virtual void person_changed(Person& p, const string& property_name) = 0;

};

template<typename T> struct Observer {
	virtual void field_changed(T& source, const string& fiele_name) = 0;
};

struct ConsolePersonObserver : Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		if (field_name == "age") {
			cout << "Person's age has changed to " << source.get_age() << ".\n";
		}
	}
};

struct ConsolePersonObserver : Observer<Person> //, Observer<Creature>
{
	// void field_changed(Person& source,...){...}
	// void field_changed(Creature& source,...){...}
};

```

### Observable\<T\>

```cpp
struct Person : Observable<Person>{
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		if (this->age == age) return;
		this->age = age;
		notify(*this, "age");
	}
private:
	int age;
};

struct PersonListener {
	virtual void person_changed(Person& p, const string& property_name) = 0;

};

template<typename T> struct Observer {
	virtual void field_changed(T& source, const string& fiele_name) = 0;
};

struct ConsolePersonObserver : Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		if (field_name == "age") {
			cout << "Person's age has changed to " << source.get_age() << ".\n";
		}
	}
};

struct ConsolePersonObserver : Observer<Person> //, Observer<Creature>
{
	// void field_changed(Person& source,...){...}
	// void field_changed(Creature& source,...){...}
};

template <typename T> struct Observable {
	void notify(T& source,const string& name){
		for (auto obs : observers) {
			obs->field_changed(source, name);
		}
	
	}
	void subscribe(Observer<T>* f) {
		observers.push_back(f);
	}
	void unsubscribe(Observer<T>* f) {

	}

private:
	vector < Observer<T>*>) observers;
};
```


### 连接观察者和被观察者
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct Person : Observable<Person>{
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		if (this->age == age) return;
		this->age = age;
		notify(*this, "age");
	}
private:
	int age;
};

struct PersonListener {
	virtual void person_changed(Person& p, const string& property_name) = 0;

};

template<typename T> struct Observer {
	virtual void field_changed(T& source, const string& fiele_name) = 0;
};

struct ConsolePersonObserver : Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		if (field_name == "age") {
			cout << "Person's age has changed to " << source.get_age() << ".\n";
		}
	}
};

struct ConsolePersonObserver : Observer<Person> //, Observer<Creature>
{
	// void field_changed(Person& source,...){...}
	// void field_changed(Creature& source,...){...}
};

template <typename T> struct Observable {
	void notify(T& source,const string& name){
		for (auto obs : observers) {
			obs->field_changed(source, name);
		}
	
	}
	void subscribe(Observer<T>* f) {
		observers.push_back(f);
	}
	void unsubscribe(Observer<T>* f) {

	}

private:
	vector < Observer<T>*>) observers;
};

struct ConsolePersonObserver :Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		cout << "Person's " << field_name << " has changed to " << source.get_age() << ".\n";
	}
};

int main() {
	Person p{ 20 };
	ConsolePersonObserver cpo;
	p.subscribe(&cpo);
	p.set_age(21); // Person's age has changed to 21.
	p.set_age(22); // Person's age has changed to 22.
}

```

### 依赖问题
```cpp
	void set_age(const int value) {
		if (age == value) return;
		age = value;

		//auto old_can_vote = can_vote(); // store old value
		//notify(*this, "age");
		//if (old_can_vote != can_vote()) // check value has changed
		//{
		//	notify(*this,"can_vote");
		//}
	}
```

### 取消订阅与线程安全
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct Person : Observable<Person>{
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		if (age == value) return;
		age = value;

		//auto old_can_vote = can_vote(); // store old value
		//notify(*this, "age");
		//if (old_can_vote != can_vote()) // check value has changed
		//{
		//	notify(*this,"can_vote");
		//}
	}

	bool get_can_vote() const {
		return age >= 16;
	}


private:
	int age;
};

struct PersonListener {
	virtual void person_changed(Person& p, const string& property_name) = 0;

};

template<typename T> struct Observer {
	virtual void field_changed(T& source, const string& fiele_name) = 0;
};

struct ConsolePersonObserver : Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		if (field_name == "age") {
			cout << "Person's age has changed to " << source.get_age() << ".\n";
		}
	}
};

struct ConsolePersonObserver : Observer<Person> //, Observer<Creature>
{
	// void field_changed(Person& source,...){...}
	// void field_changed(Creature& source,...){...}
};

template <typename T> struct Observable {
	void notify(T& source,const string& name){
		scoped_lock<mutex> lock{ mtx }; // 加锁
		for (auto obs : observers) {
			obs->field_changed(source, name);
		}
	
	}
	void subscribe(Observer<T>* f) {
		observers.push_back(f);
	}
	void unsubscribe(Observer<T>* observer) {
		observers.erase(
			remove(observers.begin(), observers.end(), observer), observers.end()
		)
	}

private:
	vector < Observer<T>*>) observers;
	mutex tex;
};

struct ConsolePersonObserver :Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		cout << "Person's " << field_name << " has changed to " << source.get_age() << ".\n";
	}
};

int main() {
	Person p{ 20 };
	ConsolePersonObserver cpo;
	p.subscribe(&cpo);
	p.set_age(21); // Person's age has changed to 21.
	p.set_age(22); // Person's age has changed to 22.
}

```

### 可重入性
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct Person : Observable<Person>{
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		if (age == value) return;
		age = value;

		//auto old_can_vote = can_vote(); // store old value
		//notify(*this, "age");
		//if (old_can_vote != can_vote()) // check value has changed
		//{
		//	notify(*this,"can_vote");
		//}
	}

	bool get_can_vote() const {
		return age >= 16;
	}


private:
	int age;
};

struct PersonListener {
	virtual void person_changed(Person& p, const string& property_name) = 0;

};

template<typename T> struct Observer {
	virtual void field_changed(T& source, const string& fiele_name) = 0;
};

struct ConsolePersonObserver : Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		if (field_name == "age") {
			cout << "Person's age has changed to " << source.get_age() << ".\n";
		}
	}
};

struct ConsolePersonObserver : Observer<Person> //, Observer<Creature>
{
	// void field_changed(Person& source,...){...}
	// void field_changed(Creature& source,...){...}
};

template <typename T> struct Observable {
	void notify(T& source,const string& name){
		vector<Observer<T>*> observers_copy;
		{
			lock_guard<mutex_t> lock{ mtx };
			observers_copy = observers;
		}
		for (auto obs : observers_copy) {
			if (obs) {
				obs->field_changed(source, name);
			}
		}

		//scoped_lock<mutex> lock{ mtx }; // 加锁
		//for (auto obs : observers) {
		//	obs->field_changed(source, name);
		//}
	
	}
	void subscribe(Observer<T>* f) {
		observers.push_back(f);
	}
	void unsubscribe(Observer<T>* observer) {
		observers.erase(
			remove(observers.begin(), observers.end(), observer), observers.end()
		);
		auto it = find(observers.begin(), observers.end(), o);
		if (it != observers.end()) {
			*it = nullptr;
		}
	}

private:
	vector < Observer<T>*>) observers;
	mutex tex;
};

struct ConsolePersonObserver :Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		cout << "Person's " << field_name << " has changed to " << source.get_age() << ".\n";
	}
};

struct TrafficAdministration : Observer<Person> {
	void field_changed(Person& source, const string& field_name) override {
		if (field_name == "age") {
			if (field_name == "age") {
				if (source.get_age() < 17) {
					cout << "Whoa there, you are not old enough to drive!\n";
				}
				else {
					cout << "We no longer care!\n";
					source.unsubscribe(this);
				}
			}
		}
	}
};

int main() {
	Person p{ 20 };
	ConsolePersonObserver cpo;
	p.subscribe(&cpo);
	p.set_age(21); // Person's age has changed to 21.
	p.set_age(22); // Person's age has changed to 22.
}

```

### Boost.Signals2中的观察者模式
```cpp
template <typename T>
struct Observable {
	signal<void(T&, const string&)> property_chagned;
};

struct Person : Observable<Person>{
	Person(int age) : age{age}{}
	int get_age() const {
		return age;
	}
	void set_age(const int value) {
		if (age == value) return;
		this->age = value;
		property_changed(*this, "age");
	}

	bool get_can_vote() const {
		return age >= 16;
	}


private:
	int age;
};

int main() {
	Person p{ 123 };
	auto conn = p.property_changed.connect([](Person&, const string& prop_name) {
		cout << prop_name << " has been changed" << endl;
		});
	p.set_age(20); // age has been changed
	
	// later,optionally
	conn.disconnect();
}
```

### 视图
```cpp
template <typename T>
struct Observable {
	signal<void(T&, const string&)> property_chagned;
};

struct Person {
	string name;
};

struct PersonView : Observable<Person> {
protected:
	Person person;
public:
	explicit PersonView(const Person& person) : person(person){}
	string& get_name() {
		return person.name;
	}
	void set_name(const string& value) {
		if (value != person.name) return;
		person.name = value;
		property_changed(person, "name");
	}
};
int main() {

}

```

# 第二十一章状态模式
### 状态驱动的状态转换
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct State {
	virtual void on(LightSwitch* ls) {
		cout << "Light is already on\n";
	}

	virtual void off(LightSwitch* ls) {
		cout << " Light is already off\n";
	}

};

class LightSwitch {
	State* state{ nullptr };

public:
	LightSwitch() {
		state = new OffState();
	}

	void set_state(State* state) {
		this->state = state;
	}
	
	void on() {
		state->on(this);
	}
	void off() {
		state->off(this);
	}
};

struct OnState : State {
	OnState() {
		cout << "Light turned on\n";
	}
	void off(LightSwitch* ls) override;
};

struct OffState : State {
	OffState() {
		cout << "Light turned off\n";
	}
	void on(LightSwitch* ls) override;
};

void OnState::off(LightSwitch* ls) {
	cout << "Switching light off...\n";
	ls->set_state(new OffState());
	delete this;
}

void OffState::on(LightSwitch* ls) {
	cout << "Switching light on...\n";
	ls->set_state(new OnState());
	delete this;
}

int main() {
	LightSwitch ls; // Light turned off
	ls.on(); // Switching light on ...
	// Light turned on
	ls.off(); // Switching light off...
	// Light turned off
	ls.off();
	// Light is already off
}
```

### 设计状态机
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

enum class State {
	off_hook,
	connecting,
	connected,
	on_hold,
	on_hook
};

enum class Trigger {
	call_dialed,
	hung_up,
	call_connected,
	placed_on_hold,
	taken_off_hold,
	left_message,
	stop_using_phone
};

State currentState{ State::off_hook }, exitState{ State::on_hook };

int main() {
	map<State, vector<pair<Trigger, State>>> rules;
	rules[State::off_hook] = {
		{Trigger::call_dialed,State::connecting},
		{Trigger::stop_using_phone,State::on_hook}
	};
	rules[State::connecting] = {
		{Trigger::hung_up,State::off_hook},
		{Trigger::call_connected,State::connected}
	};

	while (true) {
		cout << "The phone is currently " << currentState << endl;
	select_trigger:
		cout << "Select a trigger:" << "\n";
		int i = 0;
		for( auto item : rules[currentState]){
			cout << i++ << ". " << item.first << "\n";
		}
		int input;
		cin >> input;
		if (input < 0 || (input + 1)>rules[currentState].size()) {
			cout << "Incorrect option. Please try again." << "\n";
			goto select_trigger;
		}
		currentSate = rules[currentState][input].second;
		if (currentSate == exitState) break;
	}
}
```

### 基于开关的状态机
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <experimental/generator>
#include <coroutine>
using namespace std;

enum class State {
	locked,
	failed,
	unlocked
};

int main() {
	const string code{ "1274" };
	auto state{ State::locked };
	string entry;

	while (true) {
		switch (state) {
		case State::locked: {
			entry += (char)getchar();
			getchar(); // consume return
			if (entry == code) {
				state = State::unlocked;
				break;
			}

			if (!code.starts_with(entry)) {
				state = State::unlocked;
				break;
			}

			if (!code.starts_with(entry)) {
				state = State::failed;

			}
			break;
		}

		case State::failed:
			cout << "FAILED\n";
			return;
		case State::unlocked:
			cout << "UNLOCKED\n";
			return;
	}
}
```

### Boost.MSM状态机
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <utility>
#include <map>
#include <functional>
#include <thread>
#include <array>
#include <stack>
#include <set>
#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/signals2.hpp>
#include <algorithm>
#include <numeric>
#include <sstream>
#include <boost/msm/active_state_switching_policies.hpp>
#include <experimental/generator>
#include <coroutine>
using namespace std;

struct PhoneStateMachine :state_machine_def<PhoneStateMachine> {

};

struct OffHook:state<>{};

struct Connecting :state<> {
	template <class Event,class FSM>
	void on_entry(Event const& evt, FSM&) {
		cout << "We are connecting..." << endl;
	}
	//also on_exit
};
//other states omitted

struct PhoneBeingDestroyed {
	template <class EVT,class FSM,class SourceState,class TargetState>
	void operator() (EVT const&, FSM&, SourceState&, TargetState&) {
		cout << "Phone breaks into a million pieces" << endl;
	}
};

struct CanDestroyPhone {
	template <class EVT,class FSM,class SourceState,class TargetState>
	bool operator() (EVT const&, FSM& fsm, SourceState&, TargetState&) {
		return fsm.angry;
	}
};

struct transition_table :mpl::vector<Row<OffHook, CallDialed, Connecting>, Row<Connecting, CallConnected, Connected>,
	Row<Connected, PlaceOnHold, PhoneThrownIntoWall, PhoneDestroyed, PhoneBeingDestroyed, CanDestroyPhone>>{};

typedef OffHook initial_state;
template<class FSM, class Event>
void no_transition(Event const& e, FSM&, int state) {
	cout << "No transition from state " << state_names[state] << " on event " << typeid(e).name << endl;
}


int main() {
	msm::back::state_machine<PhoneStateMachine> phone;
	info(); // The phone is currently off hook
	phone.process_event(CallDialed{}); // We are connecting...
	info(); // The phone is currently connecting
	phone.process_event(CallConnected{});
	info(); // The phone is currently connected
	phone.process_event(PlacedOnHold{}); 
	info(); // The phone is currently on hold
	phone.process_event(PhoneThrowIntoWall{});
	// Phone breaks into a million piees
	info(); // The phone is currently destroyed

	phone.process_event(CallDialed{});
	// No transition from state destroyed on event struct CallDialed
}

```

# 第二十二章策略模式
### 动态策略
```cpp

```
