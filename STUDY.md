## Tổng quan về MongoDB và Mongoose
**MongoDB là gì?**
 MongoDB là một cơ sở dữ liệu NoSQL dạng tài liệu (document-oriented), lưu trữ dữ liệu dưới dạng JSON-like (gọi là BSON - Binary JSON). Nó không yêu cầu cấu trúc bảng cố định như SQL, phù hợp cho các ứng dụng linh hoạt với dữ liệu thay đổi thường xuyên.

**Đặc điểm chính:**
- Dữ liệu được tổ chức thành collections (tương tự bảng trong SQL) chứa các documents (tương tự hàng).
- Hỗ trợ truy vấn mạnh mẽ, index, và phân mảnh (sharding) cho hiệu suất cao.
- Phù hợp với ứng dụng thời gian thực, phân tán, hoặc có lượng dữ liệu lớn.

**Mongoose là gì?**
Mongoose là một ODM (Object Data Modeling) thư viện cho MongoDB, giúp tương tác với MongoDB trong Node.js bằng cách định nghĩa schema, xác thực dữ liệu, và đơn giản hóa các thao tác CRUD.

**Đặc điểm chính:**
- Cung cấp schema để định nghĩa cấu trúc dữ liệu.
- Hỗ trợ middleware (pre/post hooks) và validation.
- Dễ dàng tích hợp với Express.js.

Ứng dụng trong dự án: Quản lý schema cho User và Photo, thực hiện các truy vấn phức tạp như đếm ảnh/bình luận trong API của bạn.

## Cách cài đặt
**Cài đặt MongoDB**

- Cài đặt MongoDB Community Server:
    - Truy cập trang chính thức MongoDB.
    - Chọn phiên bản phù hợp với hệ điều hành (Windows, macOS, Linux).
    - Theo hướng dẫn cài đặt:
    Windows: Chạy file .msi, chọn cài đặt như dịch vụ và tạo thư mục dữ liệu (mặc định là C:\data\db).
    macOS/Linux: Sử dụng brew install mongodb-community (macOS với Homebrew) hoặc tải gói cài đặt.
    - Sau cài đặt, khởi động MongoDB:
    Windows: Dùng Command Prompt: mongod.
    macOS/Linux: sudo service mongod start hoặc chạy trực tiếp mongod.
    - Kiểm tra cài đặt:
    Mở terminal, gõ mongo để vào shell MongoDB. Nếu thành công, bạn sẽ thấy giao diện lệnh MongoDB.
- Cài đặt MongoDB Compass (tùy chọn):
Tải MongoDB Compass để quản lý dữ liệu qua giao diện GUI.
- Cài đặt Mongoose trong dự án Node.js
    - Khởi tạo dự án:
    Nếu chưa có, tạo thư mục dự án: mkdir my-project && cd my-project.
    - Khởi tạo Node.js: npm init -y.
    - Cài đặt các gói cần thiết:
    - Cài Express, Mongoose, và Dotenv:
    npm install express mongoose dotenv

- Cấu hình file .env:
    - Tạo file .env trong thư mục dự án:
    
        MONGODB_URI=mongodb://localhost:27017 myDatabase 
        PORT=3001
    - Thêm .env vào .gitignore để bảo mật.

## Kết nối MongoDB với Mongoose

Tạo file dbConnection.js
    
        const mongoose = require('mongoose');

        const connectDB = async () => {
        try {
            await mongoose.connect(process.env.MONGODB_URI, {
            useNewUrlParser: true,
            useUnifiedTopology: true,
            });
            console.log('MongoDB connected successfully');
        } catch (err) {
            console.error('MongoDB connection error:', err);
            process.exit(1);
        }
        };

        module.exports = connectDB;
    
Sử dụng trong file chính (như app.js/server.js)

        require('dotenv').config();
        const connectDB = require('./dbConnection');
        connectDB();

## Cách sử dụng cơ bản

**Tạo SChema:**

    const mongoose = require('mongoose');

    const userSchema = new mongoose.Schema({
    first_name: { type: String, required: true },
    last_name: { type: String, required: true },
    location: String,
    description: String,
    occupation: String,
    });

    module.exports = mongoose.model('User', userSchema);

## Hướng dẫn tổng quát về Async/Await và Try/Catch

**Async/Await là gì?**

- Async: Là từ khóa khai báo một hàm bất đồng bộ, trả về một Promise.
- Await: Chỉ được sử dụng bên trong hàm async, giúp tạm dừng thực thi cho đến khi Promise được giải quyết (resolved) hoặc từ chối (rejected).
- Mục đích: Làm cho mã bất đồng bộ dễ đọc hơn so với sử dụng .then() và .catch().

**Try/Catch là gì?**

- Try: Khối mã mà bạn muốn thực thi, nơi có thể xảy ra lỗi.
- Catch: Xử lý lỗi nếu có bất kỳ ngoại lệ nào xảy ra trong khối try.
- Mục đích: Đảm bảo ứng dụng không crash khi có lỗi, đồng thời cung cấp phản hồi phù hợp.

**Một số lưu ý khi sử dụng Async/Await và Try/Catch**

- Chỉ sử dụng await trong hàm async
- Xử lý lỗi trong catch:
    - Luôn trả về phản hồi phù hợp cho client (như res.status(500)), tránh để ứng dụng crash.
    - Ghi log lỗi để dễ debug (như console.error trong mã của bạn).
- Sử dụng Promise.all khi cần xử lý nhiều tác vụ bất đồng bộ song song

## Promise
1. Promise là gì?
Promise là một đối tượng trong JavaScript dùng để xử lý các thao tác bất đồng bộ (asynchronous). Nó đại diện cho một giá trị mà có thể chưa có ngay lập tức nhưng sẽ được giải quyết (resolved) hoặc bị từ chối (rejected) trong tương lai. Promise giúp viết mã bất đồng bộ dễ đọc hơn so với sử dụng callback thuần túy, tránh hiện tượng "callback hell".

        const myPromise = new Promise((resolve, reject) => {
        // Thực hiện tác vụ bất đồng bộ
        if (/* thành công */) {
            resolve(value); // Trả về kết quả thành công
        } else {
            reject(error); // Trả về lỗi
        }
        });

- resolve: Hàm được gọi khi tác vụ hoàn thành thành công, trả về một giá trị.
- reject: Hàm được gọi khi tác vụ thất bại, trả về một lỗi hoặc lý do thất bại.

2. Trạng thái của Promise
Một Promise có ba trạng thái:
- Pending: Trạng thái ban đầu, Promise chưa hoàn thành hoặc bị từ chối.
- Fulfilled: Promise hoàn thành thành công, giá trị được trả về qua resolve.
- Rejected: Promise thất bại, lỗi được trả về qua reject.

Lưu ý: Một khi Promise chuyển sang trạng thái Fulfilled hoặc Rejected, trạng thái của nó không thể thay đổi.

3. Cách sử dụng Promise
Promise được sử dụng với các phương thức .then(), .catch(), và .finally() để xử lý kết quả hoặc lỗi.

        myPromise
        .then(result => {
            // Xử lý khi Promise fulfilled
            console.log(result);
        })
        .catch(error => {
            // Xử lý khi Promise rejected
            console.error(error);
        })
        .finally(() => {
            // Thực hiện bất kể Promise fulfilled hay rejected
            console.log("Hoàn thành!");
        });

- .then(onFulfilled, onRejected): Xử lý kết quả thành công hoặc lỗi. Tham số thứ hai (onRejected) hiếm khi được dùng vì .catch rõ ràng hơn.
- .catch(): Chuyên xử lý lỗi, tương đương với .then(null, onRejected).

- .finally(): Chạy mã sau khi Promise hoàn thành (bất kể thành công hay thất bại).

        const fetchData = new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = true;
            if (success) {
            resolve("Dữ liệu đã lấy thành công!");
            } else {
            reject("Lỗi khi lấy dữ liệu!");
            }
        }, 1000);
        });

        fetchData
        .then(data => console.log(data)) // "Dữ liệu đã lấy thành công!"
        .catch(error => console.error(error))
        .finally(() => console.log("Hoàn tất xử lý"));
4. Các phương thức tĩnh của Promise
Promise cung cấp các phương thức tĩnh để xử lý nhiều Promise hoặc tạo Promise đặc biệt:

4.1. Promise.all(iterable)

Chức năng: Chờ tất cả Promise trong mảng iterable hoàn thành, trả về mảng kết quả theo thứ tự.

Kết quả:
Nếu tất cả Promise đều fulfilled, trả về mảng các giá trị.
Nếu bất kỳ Promise nào rejected, Promise.all ngay lập tức bị từ chối với lý do của Promise bị từ chối đầu tiên.

Ứng dụng: Xử lý nhiều tác vụ bất đồng bộ song song, như trong đoạn code bạn cung cấp trước đó (xử lý nhiều ảnh và bình luận).
javascript

    const p1 = Promise.resolve(1);
        const p2 = Promise.resolve(2);
        const p3 = Promise.resolve(3);

        Promise.all([p1, p2, p3])
        .then(results => console.log(results)) // [1, 2, 3]
        .catch(error => console.error(error));
4.2. Promise.allSettled(iterable)

Chức năng: Chờ tất cả Promise hoàn thành (bất kể fulfilled hay rejected), trả về mảng các đối tượng mô tả trạng thái và kết quả của từng Promise.

Kết quả: Mỗi phần tử trong mảng là một đối tượng với thuộc tính:
{ status: "fulfilled", value: result } (nếu thành công).
{ status: "rejected", reason: error } (nếu thất bại).

Ứng dụng: Dùng khi bạn muốn biết kết quả của tất cả Promise, kể cả khi một số bị từ chối.

    const p1 = Promise.resolve(1);
    const p2 = Promise.reject("Lỗi!");
    const p3 = Promise.resolve(3);

    Promise.allSettled([p1, p2, p3])
    .then(results => console.log(results));
    // [
    //   { status: "fulfilled", value: 1 },
    //   { status: "rejected", reason: "Lỗi!" },
    //   { status: "fulfilled", value: 3 }
    // ]
4.3. Promise.race(iterable)

Chức năng: Trả về kết quả của Promise đầu tiên hoàn thành (hoặc bị từ chối) trong mảng iterable.

Ứng dụng: Dùng để lấy kết quả nhanh nhất, như kiểm tra timeout hoặc cuộc đua giữa các tác vụ.

    const p1 = new Promise(resolve => setTimeout(resolve, 100, "Nhanh"));
    const p2 = new Promise(resolve => setTimeout(resolve, 200, "Chậm"));

    Promise.race([p1, p2])
    .then(result => console.log(result)); // "Nhanh"
4.4. Promise.any(iterable)

Chức năng: Trả về Promise đầu tiên fulfilled trong mảng iterable. Nếu tất cả Promise đều rejected, trả về một AggregateError chứa tất cả lý do từ chối.

Ứng dụng: Dùng khi bạn chỉ cần một tác vụ thành công, bất kể các tác vụ khác.

    const p1 = Promise.reject("Lỗi 1");
    const p2 = Promise.resolve("Thành công");
    const p3 = Promise.reject("Lỗi 2");

    Promise.any([p1, p2, p3])
    .then(result => console.log(result)) // "Thành công"
    .catch(error => console.error(error));
4.5. Promise.resolve(value)

Chức năng: Tạo một Promise đã fulfilled với giá trị value.

Ứng dụng: Dùng để chuẩn hóa giá trị thành Promise hoặc trả về kết quả ngay lập tức.
4.6. Promise.reject(reason)

Chức năng: Tạo một Promise đã rejected với lý do reason.

Ứng dụng: Dùng để tạo lỗi ngay lập tức trong luồng Promise.

5. Async/Await và Promise
- async/await là một cú pháp xây dựng trên Promise, giúp viết mã bất đồng bộ trông giống mã đồng bộ hơn.
- Hàm async: Hàm được khai báo với từ khóa async luôn trả về một Promise.
- Từ khóa await: Chỉ được dùng trong hàm async, tạm dừng thực thi cho đến khi Promise được giải quyết (fulfilled hoặc rejected).
- await với Promise.all: Await Promise.all được dùng để chờ tất cả Promise trong mảng hoàn thành, như xử lý nhiều ảnh hoặc bình luận.
6. Ứng dụng thực tế của Promise
- Gọi API: Sử dụng Promise với fetch hoặc thư viện như Axios để lấy dữ liệu từ server.
- Xử lý nhiều tác vụ bất đồng bộ: Như trong đoạn code bạn cung cấp, Promise.all được dùng để xử lý song song các truy vấn cơ sở dữ liệu.
- Timeout hoặc retry logic: Kết hợp Promise.race với setTimeout để xử lý timeout.
- Xử lý luồng dữ liệu phức tạp: Promise chain (liên kết .then()) để thực hiện các tác vụ theo thứ tự.

        fetch("https://api.example.com/data")
        .then(response => response.json())
        .then(data => console.log(data))
        .catch(error => console.error("Lỗi:", error));
7. Lưu ý và vấn đề thường gặp
- Xử lý lỗi: Luôn sử dụng .catch() hoặc try...catch với async/await để xử lý lỗi, tránh Promise bị từ chối không được xử lý (unhandled promise rejection).
- Hiệu suất với Promise.all: Khi xử lý số lượng lớn Promise, cần cân nhắc chia nhỏ hoặc giới hạn số lượng tác vụ đồng thời để tránh quá tải.
- Chuỗi Promise (Promise chain): Có thể nối nhiều .then() để xử lý tuần tự, nhưng cần chú ý tránh lạm dụng dẫn đến mã khó đọc.
Không lạm dụng Promise: Với các tác vụ đơn giản, không cần thiết phải dùng Promise nếu không có bất đồng bộ.

8. Tóm tắt các kiến thức liên quan
- Khái niệm: Promise là đối tượng xử lý bất đồng bộ, có ba trạng thái: Pending, Fulfilled, Rejected.
- Phương thức chính: .then(), .catch(), .finally().
- Phương thức tĩnh: Promise.all, Promise.allSettled, Promise.race, Promise.any, Promise.resolve, Promise.reject.
- Async/Await: Cú pháp dựa trên Promise, giúp mã dễ đọc hơn.
- Ứng dụng: Gọi API, xử lý song song, quản lý luồng bất đồng bộ phức tạp.
- Lưu ý: Xử lý lỗi cẩn thận, tối ưu hiệu suất với số lượng lớn Promise.

## Populate trong mongoose

1. Populate trong Mongoose là gì?

Populate cho phép bạn lấy dữ liệu từ một collection khác dựa trên các trường tham chiếu (thường là ObjectId) trong schema.
Thay vì chỉ trả về ObjectId của document liên quan, populate sẽ thay thế ObjectId đó bằng dữ liệu thực tế từ collection được tham chiếu.

Ví dụ: Nếu trường user_id trong bình luận là một ObjectId tham chiếu đến collection User, populate sẽ lấy toàn bộ (hoặc một phần) dữ liệu của người dùng từ collection User.

Cú pháp cơ bản:
    
    Model.find(query).populate({
        path: 'field_name', // Trường cần populate
        select: 'field1 field2', // Các trường muốn lấy
        model: 'ModelName', // (Tùy chọn) Tên model tham chiếu
        match: { /* Điều kiện lọc */ }, // (Tùy chọn) Lọc dữ liệu populate
        options: { /* Tùy chọn khác */ } // (Tùy chọn) Ví dụ: sort, limit
        });
        
2. Áp dụng populate vào đoạn code của bạn
Trong đoạn code bạn cung cấp, bạn đang sử dụng Promise.all để truy vấn thông tin người dùng (User.findById) cho từng bình luận. Thay vì làm điều này thủ công, bạn có thể sử dụng populate để Mongoose tự động lấy thông tin người dùng từ collection User.

Giả định schema
Giả sử bạn có các schema sau:

    const userSchema = new mongoose.Schema({
    _id: mongoose.Schema.Types.ObjectId,
    first_name: String,
    last_name: String,
    // Các trường khác
    });

    const commentSchema = new mongoose.Schema({
    comment: String,
    date_time: Date,
    user_id: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }, // Tham chiếu đến User
    });

    const photoSchema = new mongoose.Schema({
    user_id: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    file_name: String,
    date_time: Date,
    comments: [commentSchema], // Comment là subdocument
    });

    const User = mongoose.model('User', userSchema);
    const Photo = mongoose.model('Photo', photoSchema);

Trường user_id trong comments của Photo là một ObjectId tham chiếu đến collection User.

Bạn muốn lấy _id, first_name, và last_name của người dùng cho mỗi bình luận.

Sử dụng populate thay vì sử dụng Promise.all để truy vấn từng user_id trong bình luận, bạn có thể viết lại đoạn code với populate như sau:

    app.get("/photosOfUser/:id", async (req, res) => {
    try {
        const user = await User.findById(req.params.id).lean();
        if (!user) {
        return res.status(400).json({ error: "Invalid user ID" });
        }

        const photos = await Photo.find({ user_id: req.params.id })
        .select("_id user_id file_name date_time comments")
        .populate({
            path: "comments.user_id", // Populate trường user_id trong comments
            select: "_id first_name last_name", // Chỉ lấy các trường cần thiết
        })
        .lean();

        res.json(photos);
    } catch (err) {
        console.error("Error fetching photos:", err);
        res.status(400).json({ error: "Invalid user ID" });
    }
    });
3. Giải thích chi tiết đoạn code với populate
- Photo.find({ user_id: req.params.id }):
    - Tìm tất cả các ảnh có user_id khớp với req.params.id.
    - select("_id user_id file_name date_time comments") giới hạn các trường trả về từ Photo.
- .populate({ path: "comments.user_id", select: "_id first_name last_name" }):
    - path: "comments.user_id": Chỉ định rằng bạn muốn populate trường user_id trong mảng comments (một subdocument).
    - select: "_id first_name last_name": Chỉ lấy các trường _id, first_name, và last_name từ collection User cho mỗi user_id.
    - Mongoose sẽ tự động thay thế mỗi ObjectId trong comments.user_id bằng document người dùng tương ứng, chỉ chứa các trường được chỉ định.
- .lean():
    - Chuyển kết quả thành đối tượng JavaScript thuần thay vì document Mongoose, giúp giảm chi phí xử lý và phù hợp khi bạn chỉ cần dữ liệu thô để trả về client.

Kết quả:

Biến photos sẽ chứa danh sách các ảnh, với mỗi bình luận trong comments có trường user_id được thay thế bằng một object chứa _id, first_name, và last_name của người dùng.

Ví dụ đầu ra:

    [
    {
        _id: "photo1_id",
        user_id: "user_id",
        file_name: "photo.jpg",
        date_time: "2025-06-04T10:59:00Z",
        comments: [
        {
            _id: "comment1_id",
            comment: "Nice photo!",
            date_time: "2025-06-04T11:00:00Z",
            user_id: {
            _id: "user1_id",
            first_name: "John",
            last_name: "Doe"
            }
        },
        // ...
        ]
    },
    // ...
    ]
4. So sánh với cách dùng Promise.all
- Cách cũ (dùng Promise.all):
    - Bạn lặp qua từng bình luận và gọi User.findById cho mỗi user_id trong comments.
    - Điều này tạo ra nhiều truy vấn riêng lẻ tới cơ sở dữ liệu, có thể gây chậm nếu có nhiều bình luận.
    - Mã phức tạp hơn vì phải xử lý thủ công việc ánh xạ và lỗi.

- Cách mới (dùng populate):
    - Mongoose tự động xử lý việc lấy thông tin người dùng từ collection User trong một truy vấn duy nhất (hoặc ít truy vấn hơn so với cách cũ).
    - Mã ngắn gọn và dễ đọc hơn.
    - Hiệu suất tốt hơn vì Mongoose tối ưu hóa truy vấn, đặc biệt khi bạn sử dụng select để giới hạn trường.
5. Lưu ý khi sử dụng populate

Hiệu suất:
- populate thực hiện các truy vấn bổ sung để lấy dữ liệu từ collection được tham chiếu. Nếu bạn populate nhiều trường hoặc có lượng dữ liệu lớn, hãy đảm bảo chỉ lấy các trường cần thiết với select.
- Với số lượng lớn document, cân nhắc sử dụng lean() để giảm chi phí xử lý.

Cấu trúc schema:
- Trường cần populate (như user_id) phải được định nghĩa trong schema với thuộc tính ref (ví dụ: ref: 'User').
- Nếu user_id không tồn tại trong collection User, trường user_id trong kết quả sẽ là null.

Xử lý lỗi:
- Nếu user_id không hợp lệ hoặc collection User không tồn tại, populate sẽ không ném lỗi mà chỉ trả về null cho trường đó. Bạn có thể cần kiểm tra và xử lý trường hợp này nếu cần (ví dụ, gán giá trị mặc định như trong code cũ của bạn).

Sử dụng match:
- Nếu bạn muốn lọc dữ liệu populate (ví dụ, chỉ lấy người dùng có first_name cụ thể), bạn có thể thêm match vào populate:

    .populate({
    path: "comments.user_id",
    select: "_id first_name last_name",
    match: { first_name: { $ne: null } } // Chỉ lấy người dùng có first_name
    })

Populate nhiều cấp:
Nếu bạn cần populate các trường trong document được populate (nested populate), bạn có thể dùng:

    .populate({
    path: "comments.user_id",
    select: "_id first_name last_name",
    populate: { path: "another_field", select: "field1 field2" }
    })

## Authentication

**Tổng quan về Xác thực:**
Xác thực là quá trình xác minh danh tính của người dùng. Trong Node.js, có hai phương pháp chính:
- Stateful Authentication (Xác thực có trạng thái): Máy chủ lưu trữ thông tin phiên (session) để theo dõi trạng thái người dùng qua các yêu cầu.
- Stateless Authentication (Xác thực không trạng thái): Máy chủ không lưu trữ thông tin phiên; thay vào đó, sử dụng token (thường là JWT) do client gửi để xác minh danh tính.

### 1. Stateful Authentication (Xác thực có trạng thái)

**Nguyên lý hoạt động**
- Sau khi đăng nhập thành công, máy chủ tạo một phiên (session) và lưu thông tin phiên (như ID người dùng) ở phía máy chủ (thường trong bộ nhớ hoặc cơ sở dữ liệu như Redis).
- Máy chủ gửi một cookie chứa session ID đến client. Cookie này được lưu trữ ở client và gửi lại trong mỗi yêu cầu tiếp theo.
- Máy chủ sử dụng session ID để tra cứu thông tin phiên và xác thực người dùng.

Cấu hình cần thiết
- Phía Backend: Cần cấu hình CORS để cho phép gửi cookie:

        const corsOptions = {
        origin: true,
        credentials: true,
        };
- Phía Client: Khi sử dụng fetch, cần thêm credentials: "include" để gửi cookie trong các yêu cầu cross-origin:

        const response = await fetch("http://localhost:8080/api", {
        method: "post",
        credentials: "include",
        });
Ví dụ mã nguồn:
Sử dụng middleware express-session để thiết lập xác thực dựa trên phiên:

    const express = require("express");
    const session = require("express-session");
    const app = express();

    app.use(
    session({
        secret: "your_secret_key",
        resave: false,
        saveUninitialized: false,
        cookie: {
        httpOnly: true,
        maxAge: 30 * 60 * 1000, // Hết hạn sau 30 phút
        },
    })
    );

    // Route đăng nhập
    app.post("/login", (req, res) => {
    const { username, password } = req.body;
    const user = users.find((u) => u.username === username && u.password === password);
    if (user) {
        req.session.userId = user.id; // Lưu ID người dùng vào phiên
        res.send("Đăng nhập thành công");
    } else {
        res.status(401).send("Thông tin đăng nhập không hợp lệ");
    }
    });

    // Route được bảo vệ
    app.get("/home", (req, res) => {
    if (req.session.userId) {
        res.send(`Chào mừng đến với trang chủ, User ${req.session.userId}!`);
    } else {
        res.status(401).send("Không được phép truy cập");
    }
    });

    // Route đăng xuất
    app.get("/logout", (req, res) => {
    req.session.destroy((err) => {
        if (err) {
        res.status(500).send("Lỗi khi đăng xuất");
        } else {
        res.redirect("/"); // Chuyển hướng về trang chủ
        }
    });
    });

**Ưu điểm**
- Hiệu suất tốt: Thông tin phiên được lưu trên máy chủ, giảm tải xử lý phía client.
- Bảo mật cao: Cookie với thuộc tính httpOnly ngăn chặn truy cập từ JavaScript, tăng cường bảo mật.
- Trực quan: Phù hợp với các ứng dụng có khái niệm "phiên" liên tục (như các trang web truyền thống).

**Nhược điểm**
- Yêu cầu bộ nhớ: Máy chủ cần lưu trữ dữ liệu phiên, có thể tốn tài nguyên nếu có nhiều người dùng.
- Tải trọng máy chủ: Máy chủ phải quản lý và tra cứu phiên, gây áp lực khi lưu lượng lớn.
- Phụ thuộc vào hiệu quả mạng: Hiệu suất phụ thuộc vào việc lưu trữ và truy xuất phiên.
Khi nào sử dụng?
- Phù hợp với các ứng dụng web truyền thống (như trang quản trị, ứng dụng nội bộ) nơi cần duy trì trạng thái người dùng.
- Thích hợp khi bạn có thể quản lý bộ nhớ máy chủ (ví dụ: sử dụng Redis) và không cần mở rộng quy mô lớn.
### 2. Stateless Authentication (Xác thực không trạng thái)
**Nguyên lý hoạt động**
- Máy chủ không lưu trữ thông tin phiên. Thay vào đó, sau khi đăng nhập thành công, máy chủ tạo một token (thường là JWT) và gửi cho client.
- Client lưu token (thường trong localStorage hoặc sessionStorage) và gửi token trong header Authorization (dạng Bearer <token>) cho mỗi yêu cầu tiếp theo.
- Máy chủ xác minh token để xác thực người dùng mà không cần tra cứu dữ liệu phiên.

Ví dụ mã nguồn:
Sử dụng thư viện jsonwebtoken để triển khai xác thực dựa trên JWT:

    const express = require("express");
    const jwt = require("jsonwebtoken");
    const app = express();
    const secretKey = "your_secret_key";

    // Route đăng nhập
    app.post("/login", (req, res) => {
    const { username, password } = req.body;
    const user = users.find((u) => u.username === username && u.password === password);
    if (user) {
        jwt.sign({ user }, secretKey, { expiresIn: "1h" }, (err, token) => {
        if (err) {
            res.status(500).send("Lỗi tạo token");
        } else {
            res.json({ token });
        }
        });
    } else {
        res.status(401).send("Thông tin đăng nhập không hợp lệ");
    }
    });

    // Middleware xác minh token
    function verifyToken(req, res, next) {
    const token = req.headers["authorization"];
    if (token) {
        jwt.verify(token.split(" ")[1], secretKey, (err, decoded) => {
        if (err) {
            res.status(403).send("Token không hợp lệ");
        } else {
            req.user = decoded.user;
            next();
        }
        });
    } else {
        res.status(401).send("Không được phép truy cập");
    }
    }

    // Route được bảo vệ
    app.get("/dashboard", verifyToken, (req, res) => {
    res.send("Chào mừng đến với trang dashboard");
    });
**Ưu điểm**
- Dễ mở rộng: Không cần lưu trữ phiên, phù hợp với hệ thống phân tán hoặc nhiều máy chủ.
Chịu lỗi tốt: Mỗi yêu cầu độc lập, không phụ thuộc vào trạng thái máy chủ.
- Tái sử dụng linh hoạt: Token có thể được sử dụng trên nhiều nền tảng (web, mobile).

**Nhược điểm**
- Không có khái niệm phiên liên tục: Máy chủ không lưu trữ lịch sử yêu cầu, cần thêm cơ chế để quản lý trạng thái nếu cần.
- Tốn tài nguyên xử lý: Xác minh token (đặc biệt với chữ ký số) có thể tốn CPU hơn so với tra cứu phiên.
- Phức tạp hơn: Cần xử lý token hết hạn, làm mới token (refresh token), và bảo mật token ở phía client.

**Khi nào sử dụng?**
Phù hợp với các ứng dụng API, ứng dụng di động, hoặc hệ thống phân tán (microservices).
Thích hợp khi cần hỗ trợ nhiều nền tảng (web, mobile) hoặc khi không muốn lưu trữ trạng thái trên máy chủ.

**So sánh Stateful và Stateless**

Tiêu chí|Stateful(Session-based)|Stateless(Token-based)|
|-------|-----------------------|----------------------|
Lưu trữ|Lưu thông tin phiên trên máy chủ|Không lưu trữ, sử dụng token|
Cách hoạt động|Sử dụng session ID trong cookie|Sử dụng token (JWT) trong header|
Hiệu suất|Nhanh hơn với tra cứu phiên|Có thể chậm hơn do xác minh token|
Khả năng mở rộng|Khó mở rộng do cần đồng bộ phiên|Dễ mở rộng, phù hợp với hệ thống phân tán|
Bảo mật|Cookie httpOnly tăng bảo mật|Token cần được bảo vệ ở client (nguy cơ XSS)|
Phức tạp|triển khai	Đơn giản hơn, đặc biệt với ứng dụng web|Phức tạp hơn, cần quản lý token và refresh token|
Trường hợp sử dụng|Web truyền thống, ứng dụng nội bộ|API, ứng dụng di động, hệ thống phân tán|

**Hướng dẫn lựa chọn**

Chọn Stateful khi:
- Bạn xây dựng ứng dụng web truyền thống (như trang quản trị, ứng dụng nội bộ).
- Có thể quản lý bộ nhớ máy chủ (ví dụ: sử dụng Redis để lưu phiên).
- Cần duy trì trạng thái người dùng liên tục và không yêu cầu mở rộng quy mô lớn.

Chọn Stateless khi:
- Xây dựng API cho ứng dụng di động, SPA (Single Page Application), hoặc hệ thống microservices.
- Muốn giảm tải cho máy chủ và hỗ trợ hệ thống phân tán.
- Cần xác thực trên nhiều nền tảng (web, mobile) mà không phụ thuộc vào cookie.

Lưu ý thực tế

Stateful:
- Đảm bảo cấu hình cookie an toàn (httpOnly, secure, sameSite).
- Sử dụng cơ sở dữ liệu phiên (như Redis) để tăng hiệu suất và độ tin cậy.

Stateless:
- Bảo vệ token khỏi các cuộc tấn công XSS bằng cách lưu trữ an toàn (tránh localStorage nếu có thể).
- Triển khai cơ chế refresh token để xử lý token hết hạn.
- Sử dụng HTTPS để mã hóa dữ liệu truyền tải.