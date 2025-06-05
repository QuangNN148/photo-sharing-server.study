1. Let và Const

Mô tả:
- let: Khai báo biến với phạm vi khối (block-scoped), không bị hoisting như var.
- const: Khai báo hằng số, không thể gán lại giá trị (nhưng đối tượng/array có thể thay đổi nội dung).

        // let: Phạm vi khối
        if (true) {
        let x = 10;
        console.log(x); // 10
        }
        // console.log(x); // Lỗi: x is not defined

        // const: Không thể gán lại
        const PI = 3.14;
        // PI = 3.14159; // Lỗi: Assignment to constant variable
        const obj = { name: "Grok" };
        obj.name = "AI"; // Được phép thay đổi thuộc tính
        console.log(obj.name); // AI
Mẹo: Dùng const mặc định, chỉ dùng let khi cần thay đổi giá trị biến. Tránh dùng var.

2. Arrow Functions
Mô tả: Cú pháp ngắn gọn cho hàm, không có this riêng (sử dụng this của ngữ cảnh xung quanh).

        // Hàm thông thường
        function add(a, b) {
        return a + b;
        }

        // Arrow function
        const add = (a, b) => a + b;
        console.log(add(2, 3)); // 5

        // Với 1 tham số
        const square = x => x * x;
        console.log(square(4)); // 16

        // Trong array method
        const numbers = [1, 2, 3];
        const doubled = numbers.map(num => num * 2);
        console.log(doubled); // [2, 4, 6]
Mẹo: Dùng arrow function cho các hàm ngắn hoặc callback. Chú ý this khi dùng trong object.

3. Template Literals
Mô tả: Chuỗi ký tự sử dụng dấu backtick (`) cho phép nội suy (interpolation) và chuỗi đa dòng.

        const name = "Grok";
        const greeting = `Xin chào, ${name}!`; // Nội suy
        console.log(greeting); // Xin chào, Grok!

        // Chuỗi đa dòng
        const message = `
        Đây là dòng 1
        Đây là dòng 2
        `;
        console.log(message);
Mẹo: Sử dụng template literals thay cho nối chuỗi (+) để code dễ đọc hơn.

4. Destructuring Assignment
Mô tả: Phá vỡ cấu trúc của object/array để lấy giá trị dễ dàng.

        // Object destructuring
        const user = { name: "Grok", age: 3 };
        const { name, age } = user;
        console.log(name, age); // Grok 3

        // Array destructuring
        const numbers = [1, 2, 3];
        const [first, second] = numbers;
        console.log(first, second); // 1 2

        // Trong tham số hàm
        const printUser = ({ name, age }) => {
        console.log(`Tên: ${name}, Tuổi: ${age}`);
        };
        printUser(user); // Tên: Grok, Tuổi: 3
        Mẹo: Dùng destructuring để rút ngắn code khi làm việc với object/array.

5. Default Parameters
Mô tả: Cho phép đặt giá trị mặc định cho tham số hàm.

        const greet = (name = "Khách") => `Xin chào, ${name}!`;
        console.log(greet()); // Xin chào, Khách!
        console.log(greet("Grok")); // Xin chào, Grok!
Mẹo: Sử dụng để tránh kiểm tra undefined trong hàm.

6. Rest và Spread Operators
Mô tả:
- Rest (...): Thu thập các phần tử còn lại thành một array.
- Spread (...): Phân rã array/object thành các phần tử riêng lẻ.

        // Rest
        const sum = (first, ...rest) => first + rest.reduce((a, b) => a + b, 0);
        console.log(sum(1, 2, 3, 4)); // 10

        // Spread
        const arr1 = [1, 2];
        const arr2 = [...arr1, 3, 4];
        console.log(arr2); // [1, 2, 3, 4]

        const obj1 = { a: 1 };
        const obj2 = { ...obj1, b: 2 };
        console.log(obj2); // { a: 1, b: 2 }
Mẹo: Dùng spread để sao chép array/object, rest để xử lý tham số động.

7. Promises
Mô tả: Xử lý bất đồng bộ (asynchronous) một cách rõ ràng hơn callback.

        const fetchData = () => {
        return new Promise((resolve, reject) => {
            setTimeout(() => resolve("Dữ liệu đã lấy!"), 1000);
        });
        };

        fetchData()
        .then(data => console.log(data)) // Dữ liệu đã lấy!
        .catch(err => console.error(err));
Mẹo: Dùng Promise khi làm việc với API hoặc tác vụ bất đồng bộ như setTimeout.

8. Async/Await
Mô tả: Cú pháp đơn giản hóa Promise, cho phép viết code bất đồng bộ giống như đồng bộ.

        const fetchData = () => {
        return new Promise(resolve => {
            setTimeout(() => resolve("Dữ liệu đã lấy!"), 1000);
        });
        };

        const getData = async () => {
        try {
            const data = await fetchData();
            console.log(data); // Dữ liệu đã lấy!
        } catch (err) {
            console.error(err);
        }
        };

        getData();
Mẹo: Sử dụng async/await thay vì .then để code dễ đọc hơn.

9. Modules (Import/Export)
Mô tả: Cho phép tổ chức code thành các module, hỗ trợ tái sử dụng.

    // math.js
    export const add = (a, b) => a + b;
    export const subtract = (a, b) => a - b;

    // main.js
    import { add, subtract } from "./math.js";
    console.log(add(2, 3)); // 5
    console.log(subtract(5, 3)); // 2
Mẹo: Sử dụng type: "module" trong package.json để dùng ES6 modules trong Node.js:
json

        {
        "type": "module"
        }
10. Array Methods (map, filter, reduce, find)
Mô tả: Các phương thức mạnh mẽ để xử lý mảng.

        const numbers = [1, 2, 3, 4];

        // map: Tạo mảng mới
        const doubled = numbers.map(num => num * 2);
        console.log(doubled); // [2, 4, 6, 8]

        // filter: Lọc phần tử
        const evens = numbers.filter(num => num % 2 === 0);
        console.log(evens); // [2, 4]

        // reduce: Tính tổng
        const sum = numbers.reduce((total, num) => total + num, 0);
        console.log(sum); // 10

        // find: Tìm phần tử đầu tiên
        const found = numbers.find(num => num > 2);
        console.log(found); // 3
Mẹo: Sử dụng các phương thức này thay vì vòng lặp for để code ngắn gọn và rõ ràng.