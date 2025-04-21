# Vue là gì?

> Trong 30 ngày tới, chúng ta sẽ cùng nhau tìm hiểu mọi thứ bạn cần biết để bắt đầu với framework Vue. Từ **những điều cơ bản nhất** đến các chủ đề như **Vue Instance**, **Component**, và thậm chí cả **Testing**.

Mỗi ngày trong hành trình 30 ngày này sẽ xây dựng dựa trên kiến thức của ngày trước đó, giúp chúng ta làm quen với các thuật ngữ, khái niệm và nền tảng của framework Vue.

Loạt bài này chủ yếu dành cho những người chưa có kiến thức về Vue và đã có một chút hoặc một ít kinh nghiệm với JavaScript. Tuy nhiên, bạn hoàn toàn có thể nhảy qua lại giữa các bài nếu cảm thấy mình đã nắm vững một số khái niệm nhất định.

Với tất cả những điều đó, hãy bắt đầu thôi. Chúng ta sẽ bắt đầu từ những điều cơ bản nhất bằng cách tìm hiểu Vue là gì.

## Vue là gì?

Vue là một framework JavaScript mã nguồn mở hướng đến việc xây dựng giao diện người dùng, được tạo ra bởi [Evan You](https://twitter.com/youyuxi?lang=en). Nếu bạn ghé qua [trang chủ của Vue](https://vuejs.org/), bạn sẽ thấy Vue được gọi là framework JavaScript tiến bộ (**approachable**), **đa năng** (**versatile**) và **hiệu suất cao** (**performant**). Hãy cùng giải thích từng điểm này:

### Tiến bộ

Vue được coi là **tiến bộ** vì nó có thể mở rộng hoặc thu nhỏ tùy theo nhu cầu. Với các trường hợp đơn giản, bạn có thể dùng Vue giống như jQuery - chỉ cần thêm một thẻ script:

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Nhưng khi nhu cầu của bạn tăng lên, Vue cũng phát triển cùng bạn bằng cách cung cấp các công cụ trong hệ sinh thái giúp bạn làm việc hiệu quả hơn. Thường thì, Vue được gọi là framework có thể tiếp cận từng phần vì bạn có thể thêm dần các công cụ này khi cần thiết.

### Dễ tiếp cận

Vue được coi là **dễ tiếp cận** vì chỉ cần bạn biết HTML, CSS và JS cơ bản, bạn đã có thể bắt đầu làm việc với Vue để xây dựng các ứng dụng web phức tạp hơn.

### Đa năng

Framework Vue được coi là **đa năng** vì bản thân thư viện Vue nằm gọn trong một hệ sinh thái các công cụ tạo nên toàn bộ framework Vue. Các công cụ này bao gồm:

- [vue-cli](https://cli.vuejs.org/) (Công cụ dòng lệnh của Vue) giúp tạo nhanh và thiết lập các ứng dụng Vue-Webpack.
- [vue-router](https://router.vuejs.org/) giúp chúng ta thêm tính năng routing phía client vào ứng dụng một cách dễ dàng.
- [vuex](https://vuex.vuejs.org/guide/), thư viện lấy cảm hứng từ Elm và Flux giúp quản lý dữ liệu trong ứng dụng.
- [vue-test-utils](https://vue-test-utils.vuejs.org/), thư viện hỗ trợ kiểm thử giúp việc test các component Vue dễ dàng hơn.

Tất cả các công cụ trên đều được tạo ra và duy trì bởi đội ngũ phát triển Vue. Điều này giúp việc tích hợp và sử dụng chúng trong ứng dụng Vue trở nên liền mạch. Chúng ta sẽ tìm hiểu từng thư viện này ở các phần sau của loạt bài.

### Hiệu suất cao

Cuối cùng, Vue được coi là **hiệu suất cao** vì nó tận dụng virtual DOM để tái render cực nhanh. Thư viện lõi của Vue cũng được xây dựng để tối ưu hiệu suất mà không cần nhiều nỗ lực từ phía lập trình viên.

## Ồ… hay đấy… nhưng dùng nó như thế nào?

Cách đơn giản nhất để bắt đầu với Vue là thêm một thẻ script vào file HTML để tải phiên bản mới nhất của Vue từ CDN. Bạn cũng có thể tham chiếu đến một file JavaScript (ví dụ `main.js`) - nơi chứa mã Vue của bạn:

```html
<html>
  <body>
    <script src="https://unpkg.com/vue"></script>
    <script src="./main.js"></script>
  </body>
</html>
```

Khi thư viện Vue đã sẵn sàng, bạn có thể tạo một ứng dụng Vue mới. Chúng ta sẽ tạo ứng dụng này bằng cách khai báo Vue Instance - trái tim của ứng dụng Vue - trong file `main.js`. Vue instance được tạo bằng cách gọi hàm khởi tạo `new Vue({})`:

```javascript
new Vue({
  // options
});
```

Một Vue instance nhận vào một đối tượng **options** chứa các thông tin như template, data, methods, v.v. Ở instance gốc, chúng ta có thể chỉ định phần tử DOM mà instance sẽ được gắn vào, ví dụ:

```javascript
new Vue({
  el: "#app",
});
```

Chúng ta vừa sử dụng thuộc tính `el` để chỉ định phần tử HTML có `id` là `app` sẽ là **điểm gắn** của ứng dụng Vue.

I> Vue là một thư viện giao diện người dùng tập trung vào lớp _view_. Một ứng dụng Vue cần phụ thuộc vào một phần tử DOM HTML để kiểm soát cách phần tử đó hoạt động.

Vue instance cũng có thể trả về dữ liệu cần xử lý trong view. Dữ liệu này được khai báo trong thuộc tính `data`. Ví dụ, hãy khai báo một thuộc tính `greeting` với giá trị chuỗi `'Hello World!'`:

{lang=javascript,line-numbers=off}
<<[src/main.js](./src/main.js)

Để hiển thị giá trị của `greeting` trong template, trước tiên chúng ta cần khai báo phần tử mà ứng dụng Vue sẽ gắn vào (tức là phần tử có id `app`):

```html
<html>
  <body>
    <div id="app">
      <!-- nơi chứa mã template của Vue -->
    </div>
    <script src="https://unpkg.com/vue"></script>
    <script src="./main.js"></script>
  </body>
</html>
```

Bây giờ chúng ta có thể hiển thị giá trị của thuộc tính `greeting` trong Vue instance lên template HTML. Để bind dữ liệu vào nội dung văn bản của phần tử, chúng ta sử dụng [Cú pháp Mustache](https://vuejs.org/v2/guide/syntax.html#Text):

{lang=html,line-numbers=off}
<<[src/index.html](./src/index.html)

Thuộc tính `greeting` trong template giờ đã được liên kết trực tiếp với giá trị trong instance. Khi ứng dụng tải lên, bạn sẽ thấy dòng chữ Hello World!

<iframe src='./src/index.html'  ></iframe>

> Xem bản chạy thử - https://30dofv-helloworld.surge.sh

Dễ dàng phải không? Trong bài tiếp theo, chúng ta sẽ tìm hiểu sâu hơn về thuộc tính `data` của Vue instance và cách nó giúp ứng dụng Vue trở nên _reactive_ (phản ứng tự động với dữ liệu).
