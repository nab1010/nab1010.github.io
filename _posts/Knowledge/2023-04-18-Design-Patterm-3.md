---
layout: post
title: "Design Patterm 3 (Dive Into Design Partterms - Alexander Shvets)"
subtitle: "Design Patterm"
author: "nabang1010"
header-img: ""
# header-style: text
header-mask: 0.4
lang: vi
catalog: true
# hidden: false
section: Knowledge
seo-keywords:
  - design_patterm
tags:
  - Design Patterm
---

# Design Principles

## Encapsulate What Varies (Nguyên tắc đóng gói những gì thay đổi)

> ***Identify the aspects of your application that vary and separate them from what stays the same***

***Xác định các khía cạnh của ứng dụng của bạn mà thay đổi và tách chúng ra khỏi những gì vẫn giữ nguyên***

> Imagine that your program is a ship, and changes are hideous mines that linger under water. Struck by the mine, the ship sinks.

Tuơng tượng rằng chương trình của bạn là một con tàu, và các thay đổi là những quả mìn kinh khủng mà lẩn dưới nước. Bị đánh bởi mìn, con tàu chìm.

> The main goal of this principle is to minimize the effect caused by changes.

Mục tiêu chính của nguyên tắc này là giảm thiểu tác động gây ra bởi các thay đổi.

> Knowing this, you can divide the ship’s hull into independent compartments that can be safely sealed to limit damage to a single compartment. Now, if the ship hits a mine, the ship as a whole remains afloat.

Biết điều này, bạn có thể chia thành các khoang độc lập có thể được che chắn an toàn để giới hạn thiệt hại cho một khoang duy nhất. Bây giờ, nếu con tàu đâm vào một quả mìn, con tàu như một thể vẫn nổi.

> In the same way, you can isolate the parts of the program that vary in independent modules, protecting the rest of the code from adverse effects. As a result, you spend less time getting the program back into working shape, implementing and test- ing the changes. The less time you spend making changes, the more time you have for implementing features.

Tương tự, bạn có thể cô lập các phần của chương trình thay đổi trong các mô-đun độc lập, bảo vệ phần còn lại của mã khỏi các tác động tiêu cực. Kết quả là, bạn sẽ tiêu tốn ít thời gian để đưa chương trình trở lại trạng thái hoạt động, triển khai và kiểm tra các thay đổi. Một cách bạn tiêu ít thời gian thay đổi, bạn sẽ có nhiều thời gian hơn để triển khai các tính năng.

### Encapsulation on a method level (Đóng gói ở level `method`)

> Say you’re making an e-commerce website. Somewhere in your code, there’s a getOrderTotal method that calculates a grand total for the order, including taxes.

Hãy nói rằng bạn đang tạo một trang web thương mại điện tử. Ở đâu đó trong mã của bạn, có một phương thức `getOrderTotal` tính toán tổng cộng cho đơn hàng, bao gồm thuế.


> We can anticipate that tax-related code might need to change in the future. The tax rate depends on the country, state or even city where the customer resides, and the actual formula may change over time due to new laws or regulations. As a result, you’ll need to change the getOrderTotal method quite often. But even the method’s name suggests that it doesn’t care about how the tax is calculated.

Chúng ta có thể dự đoán rằng mã liên quan đến thuế có thể cần thay đổi trong tương lai. Tỷ lệ thuế phụ thuộc vào quốc gia, tiểu bang hoặc thậm chí thành phố mà khách hàng cư trú, và công thức thực tế có thể thay đổi theo thời gian do luật pháp hoặc quy định mới. Kết quả là, bạn sẽ cần thay đổi phương thức `getOrderTotal` khá thường xuyên. Nhưng ngay cả tên của phương thức cũng cho thấy rằng nó không quan tâm đến cách tính thuế.

**BEFORE**

```cpp
class Item {
  public:
    float price;
    int quantity;
}

class Oder {
  public:
    std::vector<Item> lineItems;
    std::string country;
}

float getOrderTotal(Order order) {
  float total = 0;
  for (Item item : order.lineItems) {
    total += item.price * item.quantity;
  }
  
  if (order.country == "US") {
    total += total * 0.07;
  } else if (order.country == "EU") {
    total += total * 0.2; 
  }

  return total;
}

```


> You can extract the tax calculation logic into a separate method, hiding it from the original method.

Bạn có thể trích xuất logic tính toán thuế vào một phương thức riêng, ẩn nó khỏi phương thức ban đầu.

**AFTER**

```cpp

class Item {
  public:
    float price;
    int quantity;
}

class Oder {
  public:
    std::vector<Item> lineItems;
    std::string country;
}

float getOrderTotal(Order order) {
  float total = 0;
  for (Item item : order.lineItems) {
    total += item.price * item.quantity;
  }
  
  total += total * getTaxRate(order.country);

  return total;
}

float getTaxRate(country) {
  if (country == "US") {
    return 0.07;
  } else if (country == "EU") {
    return 0.2;
  } else {
    return 0;
  }
}

```

> Tax-related changes become isolated inside a single method. Moreover, if the tax calculation logic becomes too complicated, it’s now easier to move it to a separate class.

Các thay đổi liên quan đến thuế trở nên cô lập trong một phương thức duy nhất. Hơn nữa, nếu logic tính toán thuế trở nên quá phức tạp, bây giờ dễ dàng hơn để chuyển nó sang một lớp riêng.

### Encapsulation on a class level (Đóng gói ở level `class`)

> Over time you might add more and more responsibilities to a method which used to do a simple thing. These added behaviors often come with their own helper fields and methods that eventually blur the primary responsibility of the containing class. Extracting everything to a new class might make things much more clear and simple.

Theo thời gian, bạn có thể thêm nhiều và nhiều tác vụ vào một phương thức mà trước đây chỉ làm một việc đơn giản. Những hành vi được thêm vào thường đi kèm với các trường và phương thức trợ giúp riêng của chúng mà cuối cùng làm mờ trách nhiệm chính của lớp chứa. Trích xuất tất cả vào một lớp mới có thể làm cho mọi thứ trở nên rõ ràng và đơn giản hơn nhiều.


## Program to an Interface, not an Implementation (Lập trình theo một `interface`, không phải một `implementation`)

> ***Program to an interface, not an implementation. Depend on abstractions, not on concrete classes***

***Lập trình theo một `interface`, không phải một `implementation`. Phụ thuộc vào trừu tượng, không phải trên các lớp cụ thể***

> You can tell that the design is flexible enough if you can easily extend it without breaking any existing code. Let’s make sure that this statement is correct by looking at another `cat` example. A Cat that can eat any food is more flexible than one that can eat just sausages. You can still feed the first cat with sausages because they are a subset of “any food”; however, you can extend that cat’s menu with any other food. 

Bạn có thể nói rằng thiết kế đủ linh hoạt nếu bạn có thể mở rộng nó một cách dễ dàng mà không làm hỏng bất kỳ mã hiện tại nào. Hãy đảm bảo rằng câu nói này là chính xác bằng cách xem xét một ví dụ khác về `cat`. Một con mèo có thể ăn bất kỳ thức ăn nào linh hoạt hơn một con mèo chỉ có thể ăn xúc xích. Bạn vẫn có thể cho con mèo đầu tiên ăn xúc xích vì chúng là một tập con của “bất kỳ thức ăn nào”; tuy nhiên, bạn có thể mở rộng menu của con mèo đó với bất kỳ thức ăn nào khác. 

> When you want to make two classes collaborate, you can start by making one of them dependent on the other. Hell, I often start by doing that myself. However, there’s another, more flex- ible way to set up collaboration between objects:

Khi bạn muốn làm cho hai lớp làm việc với nhau, bạn có thể bắt đầu bằng cách làm cho một trong số chúng phụ thuộc vào cái kia. Tôi thường bắt đầu bằng cách làm điều đó mình. Tuy nhiên, có một cách khác, linh hoạt hơn để thiết lập sự hợp tác giữa các đối tượng:

> 1. Determine what exactly one object needs from the other: which methods does it execute?
> 2. Describe these methods in a new interface or abstract class.
> 3. Make the class that is a dependency implement this interface.
> 4. Now make the second class dependent on this interface rather than on the concrete class. You still can make it work with objects of the original class, but the connection is now much more flexible

1. Xác định chính xác một đối tượng cần gì từ đối tượng khác: phương thức nào nó thực thi?
2. Mô tả các phương thức này trong một `interface` hoặc lớp trừu tượng mới.
3. Làm cho lớp đó là một phụ thuộc thực hiện `interface` này.
4. Bây giờ làm cho lớp thứ hai phụ thuộc vào `interface` này thay vì vào lớp cụ thể. Bạn vẫn có thể làm cho nó hoạt động với các đối tượng của lớp ban đầu, nhưng kết nối bây giờ linh hoạt hơn nhiều

> After making this change, you won’t probably feel any immedi- ate benefit. On the contrary, the code has become more complicated than it was before. However, if you feel that this might be a good extension point for some extra functionality, or that some other people who use your code might want to extend it here, then go for it. 

Sau khi thực hiện thay đổi này, bạn có thể không cảm thấy bất kỳ lợi ích nào ngay lập tức. Ngược lại, mã đã trở nên phức tạp hơn so với trước đây. Tuy nhiên, nếu bạn cảm thấy rằng điều này có thể là một điểm mở rộng tốt cho một số chức năng bổ sung, hoặc rằng một số người khác sử dụng mã của bạn có thể muốn mở rộng nó ở đây, thì hãy thử.

## Favor Composition Over Inheritance (Ưu tiên `composition` hơn là `inheritance`)

> Inheritance is probably the most obvious and easy way of reusing code between classes. You have two classes with the same code. Create a common base class for these two and move the similar code into it. Piece of cake!

Kế thừa có lẽ là cách dễ nhìn và dễ dàng nhất để tái sử dụng mã giữa các lớp. Bạn có hai lớp với cùng mã. Tạo một lớp cơ sở chung cho hai lớp này và di chuyển mã tương tự vào đó. Dễ như ăn bánh!

> Unfortunately, inheritance comes with caveats that often become apparent only after your program already has tons of classes and changing anything is pretty hard. Here’s a list of those problems.

Thật không may, kế thừa đi kèm với những lưu ý mà thường chỉ trở nên rõ ràng sau khi chương trình của bạn đã có hàng tấn lớp và thay đổi bất kỳ điều gì cũng khá khó khăn. Dưới đây là một danh sách các vấn đề đó.

> - **A subclass can’t reduce the interface of the superclass**. You have to implement all abstract methods of the parent class even if you won’t be using them.
> - **When overriding methods you need to make sure that the new behavior is compatible with the base one**. It’s important because objects of the subclass may be passed to any code that expects objects of the superclass and you don’t want that code to break.
> - **Inheritance breaks encapsulation of the superclass** because the internal details of the parent class become available to the subclass. There might be an opposite situation where a pro- grammer makes a superclass aware of some details of subclasses for the sake of making further extension easier. 
> - **Subclasses are tightly coupled to superclasses**. Any change in a superclass may break the functionality of subclasses.
> - **Trying to reuse code through inheritance can lead to creating parallel inheritance hierarchies**. Inheritance usually takes place in a single dimension. But whenever there are two or more dimensions, you have to create lots of class combinations, bloating the class hierarchy to a ridiculous size. <br>
> There’s an alternative to inheritance called composition. Whereas inheritance represents the “is a” relationship between classes (a car is a transport), composition represents the “has a” relationship (a car has an engine). <br>
> I should mention that this principle also applies to aggregation—a more relaxed variant of composition where one object may have a reference to the other one but doesn’t manage its lifecycle. Here’s an example: a car has a driver, but he or she may use another car or just walk without the car 

- **Một lớp con không thể giảm giao diện của lớp cha**. Bạn phải triển khai tất cả các phương thức trừu tượng của lớp cha ngay cả khi bạn không sử dụng chúng.
- **Khi ghi đè phương thức, bạn cần đảm bảo rằng hành vi mới tương thích với hành vi cơ bản**. Điều này quan trọng vì các đối tượng của lớp con có thể được chuyển đến bất kỳ mã nào mong đợi các đối tượng của lớp cha và bạn không muốn mã đó bị hỏng.
- **Kế thừa phá vỡ tính đóng gói của lớp cha** vì các chi tiết nội bộ của lớp cha trở nên có sẵn cho lớp con. Có thể có tình huống ngược lại khi một lập trình viên làm cho lớp cha nhận thức về một số chi tiết của các lớp con vì lợi ích của việc mở rộng tiếp theo dễ dàng hơn.
- **Lớp con được liên kết chặt chẽ với lớp cha**. Bất kỳ thay đổi nào trong lớp cha cũng có thể làm hỏng chức năng của lớp con.
- **Cố gắng tái sử dụng mã thông qua kế thừa có thể dẫn đến việc tạo ra các cấu trúc kế thừa song song**. Kế thừa thường diễn ra trong một chiều. Nhưng mỗi khi có hai hoặc nhiều chiều, bạn phải tạo ra nhiều kết hợp lớp, làm phình to cấu trúc lớp thành một kích thước ngớ ngẩn. <br>
- Có một phương án thay thế cho kế thừa gọi là `composition`. Trong khi kế thừa đại diện cho mối quan hệ “là một” giữa các lớp (một chiếc xe là một phương tiện), `composition` đại diện cho mối quan hệ “có một” (một chiếc xe có một động cơ). <br>
- Tôi nên đề cập rằng nguyên tắc này cũng áp dụng cho `aggregation`—một biến thể linh hoạt hơn của `composition` nơi một đối tượng có thể có một tham chiếu đến đối tượng khác nhưng không quản lý vòng đời của nó. Dưới đây là một ví dụ: một chiếc xe có một tài xế, nhưng anh ấy hoặc cô ấy có thể sử dụng một chiếc xe khác hoặc chỉ đi bộ mà không cần xe

----

# References

* [Dive Into Design Partterms - Alexander Shvets]()