---
title: 软件设计模式
tags: [基础知识学习]
style: fill
color: dark
description: 常见的软件设计模式总结.
---


#设计模式的六大原则：

* 单一职责原则：就一个类而言，应该只有一个引起它变化的原因。

* 开放封闭原则：软件实体可以被扩展，但是不能被修改。

* 接口隔离原则：每个接口中不存在派生类用不到但却必须实现的方法，如果有，那么需要将接口拆分。

* 依赖倒转原则：抽象不应该依赖于细节，细节应该依赖于抽象。即针对接口编程，不针对实现编程。

* 迪米特法则：如果两个类不直接通信，那么他们就不应当发生直接的相互作用。如果一个类需要调用另一个类的方法，可以通过第三个类转发这个调用。

* 里氏代换原则：一个软件实体如果使用的是一个基类，那么一定适用于其派生类。即在软件中，把派生类替换成基类后程序的行为没有发生变化。


# 设计模式分类：

* 创造性模式：单例模式、工厂模式、原型模式等；

* 结构型模式：适配器模式、装饰模式、代理模式；

* 行为型模式：观察者模式、迭代器模式等；


# 单例模式

概念：保证一个类只有一个实例，并提供一个访问它的全局访问点。

应用场景：打印机、文件系统、网络的config

实现方式：
  
  * **默认构造函数、拷贝构造函数、赋值构造函数**声明为私有的，以禁止在类的外部创建对象

  * 全局访问点也要定义成**静态类型的成员函数**，没有参数，返回该类的指针类型。因为使用实例化对象的时候是通过类直接调用该函数，并不是先创建一个该类的对象，通过对象调用。

有**饿汉式**与**懒汉式**两种实现：

  * 懒汉式：直到第一次用到类的实例时才去实例化；

  * 饿汉式：类定义的时候就去实例化；


饿汉式本身是线程安全的：

```C
class Singleton{
private:
    static Singleton* instance;
    Singleton(){}
    Singleton(const Singleton& temp){}
    Singleton& operator=(const Singleton& temp){}
public:
    static Singleton* getInstance(){
        return instance;
    }
};

Singleton* Singleton::instance = new Singleton();
```

初始版本的懒汉式不是线程安全的：

```C
class Singleton{
private:
    static Singleton* instance;
    Singleton(){}
    Singleton(const Singleton& temp){}
    Singleton& operator=(const Singleton& temp){}
public:
    static Singleton* getInstance(){
        if(instance == NULL){
            instance = new Singleton();
        }
        return instance;
    }
};
Singleton Singleton::instance = NULL;
```

问题：多线程时可能会有多个线程进入```if(instance == NULL)```的判断，从而重复创建实例。

改进版本1：加锁：

```c
class Singleton{
private:
    static pthread_mutex_t mutex;
    static Singleton* instance;
    Singleton(){
        pthread_mutex_init(&mutex, NULL);
    }
    Singleton(const Singleton& temp){}
    Singleton& operator=(const Singleton& temp){}
public:
    static Singleton* getInstance(){
        pthread_mutex_lock(&mutex);
        if(instance == NULL){
            instance = new Singleton();
        }
        pthread_mutex_unlock(&mutex);
        retrun instance;
    }
};
Singleton& Singleton::instance = NULL;
pthread_mutex_t Singleton::mutex;
```

问题：每次获取实例对象时都要加锁判断是否为空，会造成线程阻塞。

改进版本2：双重判断：

```c
class Singleton{
private:
    static Singleton* instance;
    pthread_mutex_t mutex;
    Singleton(){
        pthread_mutex_init(&mutex, NULL);
    }
    Singleton(const Singleton& temp){}
    Singleton& operator=(const Singleton& temp){}
public:
    static Singleton getInstance(){
        if(instance == NULL){
            pthread_mutex_lock(&mutex);
            if(instance == NULL){
                instance = new Singleton();
            }
            pthread_mutex_unlock(&mutex);
        }
        return instance;
    }
};
Singleton Singleton::instance = NULL;
pthread_mutex_t Singleton::mutex;
```

先进入第一个```if(instance == NULL)```的进程获得锁，并实例化对象，后进入的进程直接获取到它实例化的对象。

# 工厂模式

包括：

  * 简单工厂模式：主要用于创建对象。工厂根据输入的条件产生不同的类，然后根据不同类的虚函数得到不同结果。

  * 工厂方法模式：修正了简单工厂模式中不遵守开放封闭的原则。把选择判断移到了客户端实现，如果想添加新功能就不用修改原来的类，直接修改客户端即可。

  * 抽象工厂模式：定义了一个创建一系列相关或相互依赖的接口，而无需指定他们的具体类。
  

简单工厂模式：

```c
class Operation{
public:
    int var1, var2;
    virtual double getResult(){
        double res = 0;
        return res;
    }
};

class AddOperation: public Operation{
public:
    virtual double getResult(){
        return var1 + var2;
    }
};

class SubOperation: public Operation{
public:
    virtual double getResult(){
        return var1 - var2;
    }
};

class MulOperation: public Operation{
public:
    virtual double getResult(){
        return var1 * var2;
    }
};

class DivOperation: public Operation{
public:
    virtual double getResult(){
        return var1 / var2;
    }
}

class Factory{
public:
    static Operation* createProduct(char ch){
        switch(ch){
            case '+': return new AddOperation();
            case '-': return new SubOperation();
            case '*': return new MulOperation();
            case '/': return new SubOperation();
            default: return new AddOperation();
        }
    }
};

int main(){
    Factory myFactory = new Factory();
    Operation* product = myFactory->createProduct('+');
    product->var1 = 1.5;
    product->var2 = 6.3;
    product->getResult();
    return 0;
}
```

问题：违背了开放封闭原则，每次添加新产品都要修改工厂端代码。

工厂方法模式：修正了简单工厂模式中不遵守开放封闭原则的缺陷。

```c
class Operation{
public:
    int var1, var2;
    virtual double getResult(){
        double res = 0;
        return res;
    }
};

class AddOperation : public Operation{
public:
    virtual double getResult(){
        return var1 + var2;
    }
};

class SubOperation : public Operation{
public:
    virtul double getResult(){
        return var1 - var2;
    }
};

class MulOperation : public Operation{
public:
    virtual double getResult(){
        return var1 * var2;
    }
};

class DivOperation : public Operation{
public:
    virtual double getResult(){
        return var1 / var2;
    }
};

class Factory{
public:
    virtual Operation* createProduct() = 0;
};

class AddFactroy : public Factory{
public:
    virtual Operation* createProduct(){
        return new AddOperation();
    }
};

class SubFactory : public Factory{
public:
    virtual Operation* createProduct(){
        return new SubOperation();
    }
};

class MulFactory : public Factory{
public:
    virtual Operation* createProduct(){
        return new MulOperation();
    }
};

class DivFactory : public Factory{
public:
    virtual Operation* createProduct(){
        return new DivOperation();
    }
};

int main(){
    Factory* myFac = new AddFactory();
    Operation* myOper = myFac->createProduct();
    myOper->var1 = 2;
    myOper->var2 = 6;
    myOper->getResult();
    return 0;
}
```

抽象工厂模式：扩大了工厂能够生产产品范围，允许工厂生产一个产品族。

```c
class Operation_Pos{
public:
    int var1, var2;
    virtual double getResult(){
        int res = 0;
        return 0;
    }
};

class Operation_Neg{
public:
    int var1, var2;
    virtual double getResult(){
        int res = 0;
        return 0;
    }
};

class AddOperation_Pos{
public:
    virtual double getResult(){
        return var1 + var2;
    }
};

class AddOperation_Neg{
public:
    virtual double getResult(){
        return -(var1 + var2);
    }
};

class SubOperation_Pos{
public:
    virtual double getResult(){
        return var1 - var2;
    }
};

class SubOperation_Neg{
public:
    virtual double getResult(){
        return -(var1 - var2);
    }
};

class MulOperation_Pos{
public:
    virtual double getResult(){
        return var1 * var2;
    }
};

class MulOperation_Neg{
public:
    virtual double getResult(){
        return - (var1 * var2);
    }
};

class DivOperation_Pos{
public:
    virtual double getResult(){
        return var1 / var2;
    }
};

class DivOperation_Neg{
public:
    virtual double getResult(){
        return -(var1 / var2);
    }
};

class Factory{
public:
    virtual Operation_Pos* createProduct_Pos() = 0;
    virtual Operation_Neg* createProduct_Neg() = 0;
}

class AddFactory : public Factory{
public:
    virtual Operation_Pos* createProduct_Pos(){
        return new AddOperation_Pos();
    }

    virtual Operation_Neg* createProduct_Neg(){
        return new AddOperation_Neg();
    }
};

class SubFactory : public Factory{
public：
    virtual Operation_Pos* createProduct_Pos(){
        return new SubOperation_Pos();
    }

    virtual Operation_Neg* createProduct_Neg(){
        return new SubOperation_Neg();
    }
};

class MulFactory : public Factory{
public:
    virtual Operation_Pos* createProduct_Pos(){
        return new MulOperation_Pos();
    }

    virtual Operation_Neg* createProduct_Neg(){
        return new MulOperation_Neg();
    }
};

class DivFactory : public Factory{
public:
    virtual Operation_Pos* createProduct_Pos(){
        return new DivOperation_Pos();
    }

    virtual Operation_Neg* createProduct_Neg(){
        return new DivOperation_Neg();
    }
};

int main(){
    Factory myFac = new AddFactory();
    Operation_Pos* myOper = myFac->createProduct_Pos();
    myOper->val1 = 2;
    myOper->val2 = 6;
    myOper->getResult();

    Operation_Neg* myOper_Neg = myFac->createProduct_Neg();
    myOper_Neg->val1 = 8;
    myOper_Neg->Val2 = 6;
    myOper_Neg->getResult();
}
```

# 观察者模式

概念：定义一种一对多的关系，让多个观察者同时监听一个被观察对象，当被观察对象的状态发生变化时，会通知所有的观察对象，使他们能够更新自己的状态。

```c
class Subject;

class Observer{
protected:
    string name;
    Subject* sub;
public:
    Observer(string name, Subject* sub){
        this->name = name;
        this->sub = sub;
    }
    virtual void update() = 0;
};

class StockObserver : public Observer{
public:
    StockObserver(string name, Subject* sub) : Observer(name, sub){}

    void update();
}

class NBAObserver : public Observer{
public:
    NBAObserver(string name, Subject* sub) : Observer(name, sub){}

    void update();
};

class Subject{
private:
    list<Observer*> obs;
public:
    string action;
    virtual void attach(Obserser*) = 0;
    virtual void detach(Observer*) = 0;
    virtual void notify() = 0;
};

class Secretary : public Subject{
public:
    void attch(Oberser* observer){
        obs.push_back(observer);
    }

    void detach(Observer* observer){
        list<Observer*>:: iterator iter = observers.begin();
        while(iter != observers.end()){
            if((*iter) == observer){
                observers.erase(iter);
                return;
            }
        }
        ++iter;
    }

    void notify(){
        list<Observer*>::iterator iter = observers.begin();
        while(iter != observers.end()){
            (*iter)->update();
            ++iter;
        }
    }
};

void StockObserver::update(){
    cout << name << "msg received." << sub->action << endl;
    if(sub->action == "sos"){
        cout << "run!" << endl;
    }
}

void NBAObserver::update(){
    cout << name << "msg received." << sub->action << endl;
    if(sub->action == "sos"{
        cout << "run!!!" << endl;
    })
}

int main(){
    Subject* secretary = new Secretary();

    Observer* a = new NBAObserver("NBA_a", secretary);
    Observer* b = new NBAObserver("NBA_b", secretary);

    Observer* c = new StockObserver("Stock_c", secretary);
    Observer* d = new StockObserver("Stock_d", secretary);

    secretary->attach(a);
    secretary->attach(b);
    secretary->attach(c);
    secretary->attach(d);

    secretary->action = "hahaha";
    secretary->notify();
    cout << endl;

    secretary->action = "sos";
    secretary->notify();
    cout << endl;
    
    return 0;
}
```