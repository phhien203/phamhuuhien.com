Chào các bạn. Hôm nay mình sẽ chia sẻ với các bạn làm thế nào để nhớ các giá trị truthy và falsy trong JavaScript thật dễ dàng nhé.

## truthy và falsy value là gì?

"Bạn hãy cho biết truthy và falsy value trong Javascript là gì" có lẽ là câu hỏi được hỏi phổ biến trong các buổi phỏng vấn về Javascript. Và mình nghĩ đa phần các bạn có thể sẽ lúng túng trước câu hỏi này vì bạn không nhớ được chính xác những giá trị đó. Vì thế hôm nay mình viết bài này để chia sẻ với các bạn về vấn đề này, hy vọng có thể giúp ích được các bạn phần nào.

Các bạn cứ nghĩ đơn giản như thế này, falsy values là những giá trị trong Javascript mà khi ép kiểu về Boolean, thì sẽ cho ra giá trị false. Tương tự, truthy values là những giá trị mà khi ép kiểu về Boolean, thì sẽ cho ra giá trị true. Và trong Javascript có **06** giá trị sau được coi là falsy, còn lại tất cả những giá trị khác không phải là những giá trị này đều được xem là truthy hết. Mình có cách nhớ sáu giá trị đó như sau giúp các bạn không thể nào quên được. Đó là các bạn dựa vào những kiểu dữ liệu trong Javascript để nhớ mà thôi.

## Cách nhớ truthy và falsy value thật đơn giản

* Kiểu dữ liệu Boolean có giá trị true và false, vậy giá trị **false** dĩ nhiên sẽ là falsy rồi.
* Kiểu dữ liệu thứ hai là số (Number). Number thì chứa tất cả những con số, nhưng có hai giá trị đặc biệt đó là **số không (0)** và **Not a Number (NaN)**, vậy 0 và NaN là falsy value.
* Kiểu dữ liệu chuỗi (String) thì mình có **chuỗi rỗng** (chuỗi không chứa bất kỳ một ký tự nào khác) là falsy value.
* Và còn lại 2 giá trị **null** và **undefined** cũng sẽ là falsy value.

Nói thì hơi dài dòng như vậy nhưng mình vẽ ra bảng dưới đây cho các bạn dễ hình dung

| Kiểu dữ liệu | Falsy value |
|--------------|------------ |
| Boolean | false |
| Number | 0 và NaN |
| String | '' |
| null | null |
| undefined | undefined |

Xong, như vậy là mình có tổng cộng 6 falsy values. Còn tất cả những giá trị còn lại khác những giá trị trên thì là truthy value hết, ví dụ như một chuỗi chứa số 0 ('0') thì vẫn là truthy value như thường.

Mình hy vọng với cách nhớ này các bạn có thể tự tin trả lời được câu hỏi dạng như thế này. Dĩ nhiên là người ta có thể không hỏi bạn một cách trực tiếp như thế mà có thể hỏi một cách gián tiếp. Và cách nhớ này cũng sẽ hữu ích trong công việc hằng ngày của các bạn khi mình phải làm công việc đó là code review.

https://youtu.be/OJrr2Dw1_KQ
