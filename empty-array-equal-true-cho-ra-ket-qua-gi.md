Chào các bạn, trong bài viết trước mình nói về [truthy và falsy value trong JavaScript][1] và cách để có thể nhớ những falsy value thật đơn giản và dễ dàng. Nếu bạn đã đọc bài viết đó thì đến đây chúng ta đã biết được rằng một mảng rỗng không có phần tử nào là một truthy value, vì nó không thuộc một trong sáu giá trị của falsy value là

1.  0
2.  NaN
3.  '' (empty string)
4.  null
5.  undefined
6.  false

Vậy có một điều tưởng chừng như hơi nghịch lý đó là một mảng rỗng `[]`, khi so sánh `==` với giá trị `true` thì lại cho ra kết quả là... `false`. Sau đây là một vài ví dụ:

    if ([]) {
        console.log('dong message nay luon duoc in ra');
    }

    if ([] == true) {
        console.log('dong message nay khong bao gio duoc in ra');
    }

    if ([] == false) {
        console.log('dong message nay luon luon duoc in ra, hehe');
    }

Vậy bản chất của vấn đề ở đây là gì? Mình cùng nhau tìm hiểu nhé.

### Boolean Context

Chúng ta hãy cùng xem xét câu lệnh `if` đầu tiên, trong câu lệnh này `if ([])`, mảng rỗng được đặt trong một cặp mở ngoặc và đóng ngoặc của một câu `if`, do đó nó sẽ cần một **boolean context** trong trường hợp này, nghĩa là mảng rỗng đó sẽ được ép kiểu về `Boolean`, mà `Boolean([])` sẽ cho ra kết quả `true`, vì vậy mà dòng lệnh `console.log('dong message nay luon duoc in ra')` được gọi.

OK, trong câu lệnh tiếp theo, `if ([] == true)`, trong cặp mở ngoặc và đóng ngoặc của câu lệnh `if` lúc này là một biểu thức so sánh `==` chứ không phải là một giá trị như lúc nãy, biểu thức này sẽ cho ra kết quả là `true` hoặc `false` và câu lệnh `if` sẽ dựa vào kết quả đó mà làm tiếp. Vậy bây giờ chúng ta hãy đi từng bước để tìm ra giá trị trả về của `[] == true` là chúng ta sẽ hiểu được vấn đề. Các bạn hãy đọc thật chậm để có thể hiểu được nhé.

### Thuật toán khi so sánh với `==` trong JavaScript

Trước hết, chúng ta thấy `==` là **Equality Comparison**, và theo spec của [ECMAScript 5.1][2] thì thuật toán để tìm ra kết quả là như sau, mình xin trích dẫn ra đây và sẽ phân tích nó nha:

> **The Abstract Equality Comparison Algorithm**

> The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:

> 1. If Type(x) is the same as Type(y), then
    1. If Type(x) is Undefined, return true.
    2. If Type(x) is Null, return true.
    3. If Type(x) is Number, then
        1. If x is NaN, return false.
        2. If y is NaN, return false.
        3. If x is the same Number value as y, return true.
        4. If x is +0 and y is −0, return true.
        5. If x is −0 and y is +0, return true.
        6. Return false.
    4. If Type(x) is String, then return true if x and y are exactly the same sequence of characters (same length and same characters in corresponding positions). Otherwise, return false.
    5. If Type(x) is Boolean, return true if x and y are both true or both false. Otherwise, return false.
    6. Return true if x and y refer to the same object. Otherwise, return false.
2. If x is null and y is undefined, return true.
3. If x is undefined and y is null, return true.
4. If Type(x) is Number and Type(y) is String, return the result of the comparison x == ToNumber(y).
5. If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y.
6. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
7. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
8. If Type(x) is either String or Number and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).
9. If Type(x) is Object and Type(y) is either String or Number, return the result of the comparison ToPrimitive(x) == y.
10. Return false.

Rồi bây giờ mình sẽ đi từng bước một dựa vào thuật toán trên nha các bạn.

- Bước 0: Xác định đâu là `x` và đâu là `y` tương ứng trong spec trên. Biểu thức chúng ta đang tìm hiểu là `[] == true`, cho nên trong trường hợp này `x` là `[]`, và `y` là `true`

- Bước 1: Theo như thuật toán trên thì biểu thức so sánh của chúng ta là `[] == true` sẽ rơi vào trường hợp thứ 7 *If Type(y) is Boolean, return the result of the comparison x == ToNumber(y)* , do đó biểu thức lúc này sẽ là `[] == ToNumber(true)`. Mà chúng ta biết `Number(true)` sẽ là `1`, nên cuối cùng nó sẽ là `[] == 1`. Lúc này `x` là `[]`, và `y` là `1`

- Bước 2: Chạy tiếp thuật toán cho biểu thức ở bước 1 chúng ta sẽ thấy nó rơi vào trường hợp thứ 9 *If Type(x) is Object and Type(y) is either String or Number, return the result of the comparison ToPrimitive(x) == y*, bởi vì mảng rỗng bản chất nó là một Object, và số `1` thì chắc chắn là Number rồi. Do đó, biểu thức của chúng ta sẽ trở thành `ToPrimitive([]) == 1`

- Bước 3: Tiếp theo, bây giờ chúng ta cần tìm xem `ToPrimitive([])` thì sẽ cho ra kết quả gì nhé. Trong cái [spec][3] này, `ToPrimitive` của một Object sẽ gọi internal method `[[DefaultValue]]` của Object đó để trả về giá trị. `[[DefaultValue]]` được lấy như thế nào, các bạn có thể đọc thêm tại [đây][4] nhé, mình không muốn đưa vào đây vì có thể sẽ làm cho các bạn bị rối thêm, nhưng mình sẽ tóm tắt thuật toán như sau: trước tiên phương thức `valueOf` của Object đó sẽ được gọi, nếu giá trị trả về chưa phải là primitive, thì phương thức `toString` của Object đó sẽ được gọi để trả về giá trị. Áp dụng cách tính trên vào đây, mình có `[[DefaultValue]]` của `[]` trong trường hợp này sẽ gọi phương thức `valueOf`, nghĩa là ta sẽ có `[].valueOf() // tra ve chinh cai mang rong do []`. Do kết quả trả về vẫn là một cái mảng rỗng, chưa phải là một primitive value, nên nó sẽ gọi tiếp phương thức `toString`, nghĩa là: `[].toString() // tra ve chuoi rong ''`. Đến lúc này thì chúng ta đã có primitive value đó là chuỗi rồng `''`, do đó, biểu thức chúng ta nhận được ở bước 3 này sẽ là `'' == 1`. Lúc này `x` sẽ là `''` và `y` sẽ là `1`.

- Bước 4: Chạy tiếp thuật toán so sánh trên chúng ta sẽ thấy nó rơi vào trường hợp 5 *If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y*, vậy thì biểu thức lúc này sẽ trở thành `ToNumber('') == 1`. Mà chúng ta đã biết là `Number('')` sẽ là `0`, do đó biểu thức cuối cùng sẽ là `0 == 1`. Lúc này `x` là `0` và `y` là `1`

- Bước 5: Chạy tiếp thuật toán một lần nữa chúng ta sẽ thấy nó rơi vào trường hợp 1.3.6 bởi vì cả `x` và `y` đều cùng một kiểu là Number, và `x` có giá trị `0` khác với giá trị của `y` là `1`, do đó kết quả cuối cùng là `false`.

### Kiểm Chứng

Lúc nghiên cứu cái này mình vẫn còn hơi ngờ ngợ không biết cách tiếp cận này của mình đã chính xác hay chưa, vì thế mà mình nghĩ ra một cách để kiểm chứng, đó là mình sẽ can thiệp vào hai hàm `valueOf` và `toString` của `Array` để xem nó chạy như thế nào, dưới đây là đoạn code kiểm chứng của mình, bạn có thể mở một tab mới trong Chrome, vào console chạy đoạn code này, sau đó thì gõ lệnh `[] == true` sẽ thấy kết quả nó in ra nhé. Đoạn code cần chạy để tiến hành việc kiểm chứng

    var arrayValueOf = Array.prototype.valueOf;
    Array.prototype.valueOf = function() {
        console.log('Array.prototype.valueOf is called with', this);
        var value = arrayValueOf.call(this);
        console.log('result is', value);
        return value;
    }

    var arrayToString = Array.prototype.toString;
    Array.prototype.toString = function() {
        console.log('Array.prototype.toString is called with', this);
        var value = arrayToString.call(this);
        console.log('result is', value);
        return value;
    }

và đây là kết quả

    [] == true
    VM821:3 Array.prototype.valueOf is called with []
    VM821:5 result is []
    VM821:11 Array.prototype.toString is called with []
    VM821:13 result is
    false

### Lời Kết

Đến đây chắc các bạn đã có thể giải thích được tại sao `[] == true` cho kết quả là `false` rồi chứ? Đây là một biểu thức so sánh bình thường và không có liên quan gì đến truthy, falsy ở đây cả. Đó là điểm chính yếu khiến mình bị nhầm lẫn. Và vì nó là biểu thức so sánh bình thường nên sẽ có thuật toán để tìm ra kết quả như mình đã giới thiệu ở trên.

Nếu như các bạn vẫn chưa thể hiểu hết thì cũng là chuyện bình thường, vì lúc mình nghiên cứu cái này cũng phải đọc đi đọc lại 3, 4 lần mới hiểu được. Hy vọng với một chút ít kiến thức chia sẻ ở đây có thể giúp các bạn hiểu rõ hơn về cơ chế làm việc của `==` trong JavaScript, để bạn có thể tự tin hơn về kiến thức JavaScript của mình, cũng như có thêm động lực để học hỏi và chia sẻ nhé các bạn.

Mình rất mong nhận được những ý kiến đóng góp của các Cao Nhân để mình và các bạn đây có thể học hỏi thêm nhiều điều hay ho khác nữa. Mến chào các bạn, hẹn gặp lại các bạn trong những bài viết tiếp theo nha. Đây là những link mà mình đã tham khảo cho bài viết này:

<http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3>
<http://www.ecma-international.org/ecma-262/5.1/#sec-9.1>
<http://www.ecma-international.org/ecma-262/5.1/#sec-8.12.8>
<http://stackoverflow.com/questions/5814949/how-to-get-objects-defaultvalue>

 [1]: https://phamhuuhien.com/truthy-va-falsy-trong-javascript/
 [2]: http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3
 [3]: http://www.ecma-international.org/ecma-262/5.1/#sec-9.1
 [4]: http://www.ecma-international.org/ecma-262/5.1/#sec-8.12.8
