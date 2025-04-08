
 
一、抽象类实战：家族基因传承 
```java 
// 抽象类：动物家族的基本模板 
abstract class Animal {
    private String name;  // 可以有成员变量 
    
    public Animal(String name) {  // 有构造方法 
        this.name = name;
    }
    
    // 具体方法：子类直接继承 
    public void eat() {
        System.out.println(name + "正在吃东西");
    }
    
    // 抽象方法：子类必须实现 
    public abstract void makeSound();
}
 
// 具体子类 
class Dog extends Animal {
    public Dog(String name) {
        super(name);  // 调用父类构造 
    }
    
    @Override 
    public void makeSound() {
        System.out.println("汪汪！");
    }
}
 
// 使用示例 
Animal myDog = new Dog("阿黄");
myDog.eat();        // 输出：阿黄正在吃东西（继承来的方法）
myDog.makeSound();  // 输出：汪汪！（必须实现的方法）
```
 
生活比喻：  
抽象类就像家族族谱，规定了所有后代必须有的特征（如必须会发出声音），但也允许继承一些现成的能力（如吃饭方法）。就像每个姓张的人都必须会写张字，但可能各自有不同的职业。
 
---
 
二、接口实战：能力证书认证 
```java 
// 接口：飞行能力认证 
interface Flyable {
    double MAX_HEIGHT = 1000;  // 默认是 public static final 
    
    // 抽象方法（Java8前唯一允许的类型）
    void fly();
    
    // 默认方法（Java8新增）
    default void emergencyLanding() {
        System.out.println("紧急迫降中...");
    }
    
    // 静态方法（Java8新增）
    static void showLicense() {
        System.out.println("国际飞行许可证");
    }
}
 
// 实现类1：飞机 
class Airplane implements Flyable {
    @Override 
    public void fly() {
        System.out.println("喷气引擎启动，飞行高度800米");
    }
}
 
// 实现类2：鸟 
class Bird implements Flyable {
    @Override 
    public void fly() {
        System.out.println("翅膀扇动，飞行高度50米");
    }
}
 
// 使用示例 
Flyable[] flyers = {new Airplane(), new Bird()};
for (Flyable f : flyers) {
    f.fly();  // 多态调用 
    f.emergencyLanding();  // 使用默认方法 
}
Flyable.showLicense();  // 直接通过接口调用静态方法 
```
 
生活比喻：  
接口就像职业资格证书，无论你是什么身份（飞机/鸟/超人），只要考取「飞行执照」（实现接口），就必须具备规定的飞行能力，但具体怎么飞可以自由发挥。
 
---
 
三、混合使用：继承+能力扩展 
```java 
// 继承抽象类 + 实现多个接口 
class SuperDog extends Animal implements Flyable, Swimmable {
    public SuperDog(String name) {
        super(name);
    }
    
    @Override 
    public void makeSound() {
        System.out.println("超级汪汪！");
    }
    
    @Override 
    public void fly() {
        System.out.println("用耳朵当翅膀飞");
    }
    
    @Override 
    public void swim() {
        System.out.println("狗刨式游泳");
    }
}
```
 
关键区别总结表：
| 场景                  | 抽象类                              | 接口                                |
|----------------------|-----------------------------------|-----------------------------------|
| 变量             | 可以有普通变量                     | 只能是 public static final 常量   |
| 构造方法         | 有                                | 无                                |
| 方法实现         | 可包含具体实现                     | Java8前只能是抽象方法             |
| 继承关系         | 单继承                            | 多实现                            |
| 设计意图         | "是什么"（is-a关系）              | "能做什么"（has-a能力）           |
| 代码复用         | 通过继承共享代码                  | 通过组合扩展能力                  |
 
---
 
四、什么时候用哪个？
1. 选抽象类：  
   - 需要定义对象的 核心身份（如`Employee`作为所有员工的基类）  
   - 需要 共享代码（如计算工资的通用方法）  
   - 需要控制 子类构造过程
 
2. 选接口：  
   - 需要定义 跨继承体系 的能力（如`Serializable`）  
   - 需要实现 多重行为（如一个设备同时可打印、可扫描）  
   - 要支持 未来扩展（通过默认方法添加新功能）
 
---
 
五、现实项目中的典型应用 
案例1：日志系统
```java 
// 接口定义标准 
interface Logger {
    void log(String message);
}
 
// 抽象类封装通用逻辑 
abstract class AbstractLogger implements Logger {
    protected String formatMessage(String msg) {
        return "[" + LocalDateTime.now() + "] " + msg;
    }
}
 
// 具体实现 
class FileLogger extends AbstractLogger {
    @Override 
    public void log(String message) {
        System.out.println("写入文件：" + formatMessage(message));
    }
}
```
 
案例2：支付网关
```java 
// 接口定义支付能力 
interface PaymentGateway {
    boolean processPayment(double amount);
    default String generateReceipt() {
        return "支付凭条-" + UUID.randomUUID();
    }
}
 
// 抽象类处理公共逻辑 
abstract class BankGateway implements PaymentGateway {
    protected boolean validateCard(String cardNumber) {
        // 通用银行卡校验逻辑 
        return cardNumber.startsWith("62");
    }
}
 
// 具体银行实现 
class ICBCGateway extends BankGateway {
    @Override 
    public boolean processPayment(double amount) {
        // 实现工商银行特有逻辑 
        return true;
    }
}
```
 
最终记忆技巧：下次写代码前问自己：  
👉 如果这个类型是多种对象的 根本身份（如`Vehicle`），用抽象类  
👉 如果这个类型是描述 某种能力（如`Drivable`），用接口  
👉 需要严格单继承时用抽象类，需要灵活多扩展时用接口
