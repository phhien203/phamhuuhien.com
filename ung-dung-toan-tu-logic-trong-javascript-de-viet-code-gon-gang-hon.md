Chào các bạn, chủ đề hôm nay mình muốn chia sẻ với các bạn đó là làm thế nào để viết code JavaScript trông gọn gàng hơn bằng cách ứng dụng toán tử logic trong JavaScript nhé.

###  Toán tử logic và cách thức hoạt động

Trước tiên mình xin nói sơ qua về các toán tử logic có trong JavaScript. Chúng ta có tổng cộng ba toán tử logic đó là toán tử AND (&&), toán tử OR (||) và toán tử NOT (!).

Trong toán logic các bạn đã biết đó là toán tử AND sẽ trả về đúng khi tất cả các toán hạng trong đó đều đúng, và sẽ trả về sai khi tồn tại một toán hạng trong đó là sai. Ngược lại, toán tử OR sẽ trả về sai khi tất cả các toán hạng trong đó đều sai, và sẽ trả về đúng khi tồn tại một toán hạng đúng.

Trong JavaScript cũng tương tự như vậy nhưng có hơi mở rộng thêm một chút xíu.

### Toán tử AND

Cú pháp: `exp1 && exp2 && exp3 && ... && expn`

Toán tử AND sẽ trả về **giá trị** truthy của toán hạng cuối cùng (expn) nếu tất cả n toán hạng đều là truthy, và sẽ trả về **giá trị** falsy nếu có tồn tại một toán hạng falsy, trong trường hợp này thì toán hạng có giá trị falsy đầu tiên sẽ được trả về. Trong biểu thức trên giả sử mình có toán hạng exp2 là falsy, thì phép kiểm tra sẽ dừng lại ngay, các toán hạng đứng sau sẽ không được kiểm tra giá trị nữa, và giá trị falsy của exp2 sẽ được trả về. Cái này người ta gọi là short circuit.

### Toán tử OR

Cú pháp: `exp1 || exp2 || ... || expn`

Toán tử OR sẽ trã về **giá trị** falsy của toán hạng cuối cùng (expn) nếu tất cả n toán hạng đều là falsy, và trả về **giá trị** truthy nếu có tồn tại một toán hạng truthy, và trong trường hợp đó, toán hạng truthy đầu tiên sẽ được trả về giá trị. Toán tử OR cũng có short circuit như mình đã nói ở trên.

Như vậy thì trong hai trường hợp trên, chúng ta đều nhận được **giá trị** falsy hoặc truthy, các bạn để ý nhé, mình nhấn mạnh đến chữ giá trị truthy, falsy, nghĩa là giá trị đó có thể là Boolean, cũng có thể là non-Boolean. (Truthy và falsy là gì mình đã có bài viết ở [đây](https://phamhuuhien.com/truthy-va-falsy-trong-javascript/). Bạn có thể xem qua để hiểu kỹ hơn nhé). Đó là một tính năng quan trọng mà chúng ta sẽ vận dụng nó nhiều đấy.

### Toán tử NOT

Cú pháp: `!exp`

Toán tử NOT sẽ trả về true nếu exp là falsy, và trả về false nếu exp là truthy. Như vậy toán từ NOT chỉ trả về giá trị Boolean là true hoặc false thôi, không giống như toán tử AND và OR ở trên.

### Ứng dụng toán tử logic để viết code gọn gàng hơn

OK, đây là phần quan trọng nhất mà mình muốn nói tới đây. Biết được cách thức hoạt động của các toán tử logic như trên rồi, bây giờ mình sẽ vận dụng vào trong việc viết code như thế nào để cho gọn gàng hơn đây? Chúng ta cùng tìm hiểu qua một số kỹ thuật sau đây nhé.

#### Kỹ thuật 1: falback value (default value)

Giả sử mình có một đối tượng là employee và có các thông tin về tên, tuổi và địa chỉ. Yêu cầu chúng ta hãy viết một hàm in thông tin nhân viên ấy ra console, và thông tin về địa chỉ của nhân viên là không bắt buộc, nếu như có thông tin địa chỉ thì in thông tin đó ra, còn không có thì sẽ in ra chuỗi 'Vietnam'.

    var employee = {
      name: 'Nguyen Van A', // thong tin bat buoc
      YOB: 1999, // thong tin bat buoc
      address: 'Tan Binh' // thong tin khong bat buoc, co the co hoac khong co
    }

    function displayEmployee(employee) {
      console.log('Employee:', employee.name);
      console.log('YOB:', employee.YOB);

      if (employee.address) {
        console.log('Address:', employee.address);
      } else {
        console.log('Address: Vietnam');
      }
    }

Đây là cách viết mà chúng ta thường thấy, nhưng có một cách viết khác gọn hơn như thế này

    function displayEmployee(employee) {
      console.log('Employee:', employee.name);
      console.log('YOB:', employee.YOB);
      console.log('Address:', employee.address || 'Vietnam');
    }

Cách viết trên có thể được giải thích như sau:

Đây là toán tử logic OR, và bắt đầu từ trái qua phải, nếu employee.address có giá trị, nghĩa là truthy, thì giá trị đó sẽ được trả về. Ngược lại, nếu employee.address không có giá trị, nghĩa là falsy, thì phép so sách sẽ đi tiếp, tới chuỗi 'Vietnam', nó là truthy nên phép so sánh dừng lại và trả về giá trị là 'Vietnam'. OK, cũng không khó hiểu phải không các bạn, và kỹ thuật này người ta gọi là fallback value hay là default value. Như các bạn thấy, cách viết này trông gọn gàng hơn cách viết ban đầu vì nó không có nhiều if else.

Cách dùng fallback value này mở rộng ra mình sẽ có ví dụ sau. Cũng là hàm hiển thị thông tin nhân viên như trên nhưng lúc này trong nhân viên sẽ có 3 địa chỉ lần lượt là address1, address2, address3. Nếu địa chỉ thứ nhất không có giá trị, thì sẽ lấy địa chỉ thứ hai, nếu địa chỉ thứ hai không có giá trị, thì sẽ lấy địa chỉ thứ ba, và nếu địa chỉ thứ ba cũng không có giá trị luôn thì sẽ lấy giá trị mặc định là 'Vietnam'. Nếu viết theo cách `if else` thông thường, chúng ta sẽ có đoạn code như sau:

    function displayEmployee(employee) {
      console.log('Employee:', employee.name);
      console.log('YOB:', employee.YOB);

      if (employee.address1) {
        console.log('Address:', employee.address1);
      } else if (employee.address2) {
        console.log('Address:', employee.address2);
      } else if (employee.address3) {
        console.log('Address:', employee.address3);
      } else {
        console.log('Address: Vietnam');
      }
    }

Và nếu chúng ta áp dụng kỹ thuật fallback value, thì code sẽ như sau:

    function displayEmployee(employee) {
      console.log('Employee:', employee.name);
      console.log('YOB:', employee.YOB);
      console.log('Address:',
        employee.address1 || employee.address2 || employee.address3 || 'Vietnam');
    }

Chúng ta có thể thấy là cách viết thứ hai nhìn sẽ gọn gàng hơn vì không có nhiều `if else` nữa.

#### Kỹ thuật 2: truy cập giá trị trong các đối tượng lồng nhau nhiều cấp

Khi làm việc với cấu trúc dữ liệu gồm các đối tượng lồng nhau trong nhiều cấp, để lấy giá trị của một thuộc tính trong cấp trong cùng, chúng ta thường dùng các câu lệnh if để kiểm tra giá trị của từng đối tượng từ cấp cao nhất vào trong, ví dụ như ta có dữ liệu có cấu trúc như sau

    var responseJSON = {
      department: {
        employee: {
          name: 'Nguyen Van A',
          YOB: 1999,
          address: 'Can Tho'
        }
      }
    };

Trong đó, department, employee đều là những đối tượng không bắt buộc, nghĩa là responseJSON có thể có chứa department hoặc không, và trong department, có thể có hoặc không có employee, và address trong employee cũng có thể có hoặc không có giá trị. Yêu cầu đặt ra là chúng ta hãy viết hàm hiển thị thông tin của nhân viên đó, và nếu nhân viên đó không có địa chỉ thì in ra 'Vietnam'. Ta có hàm sau đây để hiển thị thông tin nhân viên đó

    function displayEmployee(responseJSON) {
      var department = responseJSON.department;

      if (department) {
        var employee = department.employee;

        if (employee) {
          console.log('Employee:', employee.name);
          console.log('YOB:', employee.YOB);

          if (employee.address) {
            console.log('Address:', employee.address);
          } else {
            console.log('Address: Vietnam');
          }
        }
      }
    }

Đoạn code trên chúng ta đi từng cấp một từ ngoài vào trong để kiểm tra đối tượng ở từng cấp có giá trị hay không. Chúng ta có thể ứng dụng toán tử logic AND và viết lại như sau

    function displayEmployee(responseJSON) {
      var employee = responseJSON.department && responseJSON.department.employee;
      if (employee) {
          console.log('Employee:', employee.name);
          console.log('YOB:', employee.YOB);
          console.log('Address:', employee.address || 'Vietnam');
      }
    }

#### Kỹ thuật 3: đảo ngược điều kiện if else nếu trong if có chứa toán tử phủ định

Đây không hẳn là một kỹ thuật nhưng mình cũng xin liệt kê ở đây vì mình nghĩ đó là một best practice mà chúng ta nên theo để code của chúng ta đọc dễ hiểu hơn. Ý tưởng cơ bản đó chính là nếu trong câu `if` chúng ta có toán tử phủ định trên một biểu thức, và theo sau `if` là một khối lệnh nằm trong `else`, thì chúng ta nên đảo ngược thứ tự của `if else` lại cho dễ hiểu. Như ví dụ sau

    if (!employee) {
      console.log('Employee not found');
    } else {
      displayEmployee(employee);
    }

Trong trường hợp này, chúng ta nên đảo ngược thứ tự lại trông sẽ hợp với ngôn ngữ tự nhiên hơn.

    if (employee) {
      displayEmployee(employee);
    } else {
       console.log('Employee not found');
    }

### Lời kết

Như vậy là mình đã trình bày cho các bạn các toán tử logic và cách thức nó hoạt động trong JavaScript. Và cách chúng ta áp dụng chúng vào việc viết code của chúng ta. Trong thực tế thì chúng ta có thể kết hợp cà toán tử AND và OR lại với nhau nữa chứ không phải là chỉ có AND hoặc là chỉ có OR không thôi.

Cảm ơn các bạn đã xem bài viết này, hẹn gặp lại vào những bài chia sẻ tiếp theo nhé!

https://youtu.be/PeWEik7D75w?list=PLn5_43v3hnX9OJueO2S4SzijvwtMzelN7
