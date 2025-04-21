# Vue Instance - Ứng dụng hướng dữ liệu

> Ở bài hôm qua, chúng ta đã hiểu cách dữ liệu hoạt động một cách phản ứng trong Vue. Hôm nay, chúng ta sẽ dành thêm thời gian để bàn về hành vi này vì nó đóng vai trò đặc biệt quan trọng trong cách xây dựng ứng dụng với Vue.

## Dữ liệu phản ứng (Reactive Data)

Dữ liệu trong Vue được xử lý theo kiểu phản ứng vì việc thay đổi dữ liệu thường trực tiếp làm cho view được cập nhật. Với mỗi cặp key-value mà chúng ta khai báo trong thuộc tính `data` của một instance, thư viện Vue sẽ tạo ra các _getter_ và _setter_ cho thuộc tính đó. Các setter và getter này hoạt động _ngầm_ để theo dõi các thuộc tính dữ liệu và khiến template render lại khi có thay đổi.

I> Để hiểu chi tiết hơn về reactivity của dữ liệu - hãy đọc phần [Reactivity in Depth](https://vuejs.org/v2/guide/reactivity.html) trong tài liệu chính thức của Vue.

Nếu bạn từng làm việc với [React](https://reactjs.org/), bạn sẽ nhận thấy trạng thái phản ứng (tức là dữ liệu) trong Vue khác với cách xử lý dữ liệu trong React. Trong React, [state thường nên được coi là _bất biến_](https://reactjs.org/docs/react-component.html#state). Tính phản ứng của state là một trong những điểm khác biệt chính làm cho Vue trở nên độc đáo. Việc quản lý state (dữ liệu) trong Vue thường trực quan và dễ hiểu vì thay đổi state sẽ trực tiếp làm view cập nhật.

## Ứng dụng hướng dữ liệu

Tính chất phản ứng của dữ liệu trong Vue giúp chúng ta xây dựng các ứng dụng theo hướng _data-driven_ (hướng dữ liệu). Để hiểu rõ hơn, hãy xem lại ứng dụng đơn giản mà chúng ta đã xây dựng ở bài hôm qua.

<iframe src='../day-02/src/simple-data-change-example/index.html'
        height="250"
        scrolling="no"
         >
</iframe>

> Xem bản chạy thử - <https://30dofv-simpledatachange.surge.sh>

Giả sử chúng ta muốn mở rộng ứng dụng và bổ sung các tính năng như:

- Hiển thị ngày giờ hiện tại.
- Chuyển đổi thông tin người dùng và thành phố từ một danh sách lựa chọn.
- Thay đổi màu nền khi nhấn nút.
- v.v...

Với các tính năng này, chúng ta sẽ tận dụng tính phản ứng của Vue và bổ sung các thuộc tính dữ liệu mới như `date` (giá trị là ngày hiện tại `new Date()`) hoặc `cities` (một mảng chứa các thành phố như `['Lagos', 'New York', 'Tokyo', 'Toronto']`).

Cú pháp Mustache và một số directive (chúng ta sẽ bắt đầu thấy ở bài tiếp theo) sẽ giúp chúng ta liên kết toàn bộ hoặc một phần thông tin này vào template. Với sự hỗ trợ của các phương thức và các khả năng inline khác, **chúng ta có thể kích hoạt thay đổi dữ liệu của instance** để cập nhật template theo đúng tình huống mong muốn. Đây chính là tư duy _hướng dữ liệu_ khi xây dựng UI.

I> Nếu bạn từng dùng [React](https://reactjs.org), [Angular](https://angular.io/), hoặc các framework/library frontend hiện đại khác; bạn sẽ quen với mô hình mà việc thay đổi dữ liệu/state _làm thay đổi giao diện ứng dụng_.

Ngược lại, hãy thử tái hiện chức năng của ứng dụng ở bài trước (tức là chuyển đổi thông điệp chào mừng khi nhấn nút) chỉ với JavaScript thuần (vanilla JS). Có nhiều cách để làm điều này, ví dụ như sau:

### HTML

```html
<html>
  <head>
    <link rel="stylesheet" href="./styles.css" />
  </head>

  <body>
    <div id="app">
      <h2>Hello World!</h2>
      <p>by Hassan Djirdeh who lives in Toronto</p>
      <button onclick="changeGreeting()">Change Greeting</button>
    </div>
    <script src="./main.js"></script>
  </body>
</html>
```

### JS

```javascript
// Cách làm với JS thuần

let greetingTag = document.getElementsByTagName("h2")[0];

changeGreeting = () => {
  if (greetingTag.textContent === "Hello World!") {
    greetingTag.textContent = "What is up!";
  } else {
    greetingTag.textContent = "Hello World!";
  }
};
```

Chức năng chuyển đổi trực tiếp giữa các nội dung văn bản là tương tự như trước:

- Kiểm tra nếu nội dung của thẻ `<h2>` là `Hello World!`.
- Nếu đúng thì đổi thành `What is up!`.
- Nếu không thì đổi lại thành `Hello World!`.

Điểm khác biệt giữa hai cách tiếp cận nằm ở chỗ chúng ta truy cập và thay đổi nội dung của thẻ `<h2>` như thế nào. **Với JavaScript thuần, DOM là nguồn dữ liệu duy nhất (single source of truth)**. Để xác định nội dung của thẻ `<h2>`, chúng ta phải truy vấn DOM, tìm phần tử, rồi kiểm tra giá trị `textContent` của nó. Vì DOM là _nơi duy nhất_ chứa thông tin này!

Còn với ví dụ Vue, chúng ta chỉ cần lấy và thay đổi giá trị của thuộc tính dữ liệu được dùng trong template (`greeting`), mà không cần truy vấn DOM. Đó là lý do **nguồn dữ liệu duy nhất trong ứng dụng Vue là thuộc tính data của Vue instance**. Trong các ứng dụng Vue, chúng ta hầu như không cần dùng các phương thức như `document.getElementsByTagName` hay `document.querySelector('img').setAttribute()`, mà thay vào đó sử dụng các thuộc tính `data` của instance để _điều khiển sự thay đổi giao diện_.

## Thuộc tính dữ liệu trong Vue

Vue khởi tạo tính phản ứng cho một instance ngay khi khởi tạo, vì vậy chúng ta cần khai báo trước các thuộc tính sẽ dùng. Do đó, chúng ta không thể thêm (hoặc xóa) thuộc tính mới trực tiếp vào một instance đã được tạo. Ví dụ sau sẽ không hoạt động:

```javascript
new Vue({
  el: "#app",
  data: {
    user: "Hassan Djirdeh",
    city: "Toronto",
  },
  methods: {
    addGreeting() {
      // greeting chưa được khởi tạo :(
      this.greeting = "Hello World!";
    },
  },
});
```

Trong ví dụ trên, Vue sẽ cảnh báo trên console như sau:

```shell
Property "greeting" is not defined on the instance...
```

Chúng ta cần khai báo trước tất cả các thuộc tính sẽ sử dụng.

```javascript
new Vue({
  el: "#app",
  data: {
    greeting: "", // greeting đã được khởi tạo
    user: "Hassan Djirdeh",
    city: "Toronto",
  },
  methods: {
    addGreeting() {
      // giờ có thể cập nhật greeting!
      this.greeting = "Hello World!";
    },
  },
});
```

I> Vue 3.0 (dự kiến ra mắt năm 2019) sẽ sử dụng [Proxy-based observation mechanism](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) để phát hiện thay đổi dữ liệu. Điều này sẽ cho phép chúng ta xóa hoặc thêm thuộc tính mới sau khi instance đã được khởi tạo! Chúng ta sẽ tìm hiểu sâu về các cập nhật của Vue 3.0 ở bài gần cuối loạt bài này - **Vue 3.0 và tương lai của Vue**.

Để biết thêm về reactivity trong Vue và việc khai báo thuộc tính phản ứng từ đầu, hãy xem phần [Reactivity in Depth](https://vuejs.org/v2/guide/reactivity.html) trong tài liệu Vue.

Vậy là xong cho hôm nay! Ở bài tiếp theo, chúng ta sẽ bắt đầu bàn về một số directive quan trọng và rất hữu ích của Vue.
