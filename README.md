# clean-code-javascript

## Table of Contents

1. [Introduction](#introduction)
2. [Variables](#variables)
3. [Functions](#functions)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)


## Giới thiệu

![Hình ảnh hài hước đánh giá chất lượng phần mềm đếm được bao nhiêu từ tục tĩu
bạn hét lên khi đọc mã](https://www.osnews.com/images/comics/wtfm.jpg)

Nguyên tắc công nghệ phần mềm, từ cuốn sách của Robert C. Martin
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
được điều chỉnh cho JavaScript. Đây không phải là một hướng dẫn về phong cách. Đó là hướng dẫn sản xuất
Phần mềm [có thể đọc, tái sử dụng và tái cấu trúc](https://github.com/ryanmcdermott/3rs-of-software-architecture) trong JavaScript.

Không phải mọi nguyên tắc ở đây đều phải được tuân thủ nghiêm ngặt, và thậm chí còn ít hơn nữa.
được nhất trí rộng rãi. Đây chỉ là những hướng dẫn và không có gì hơn, nhưng chúng
những điều được hệ thống hóa qua nhiều năm kinh nghiệm tập thể của các tác giả của
_Mã sạch_.

Nghề công nghệ phần mềm của chúng tôi mới chỉ hơn 50 tuổi một chút và chúng tôi đang
vẫn đang học hỏi rất nhiều. Khi kiến ​​trúc phần mềm cũng cũ như kiến ​​trúc
có thể khi đó chúng ta sẽ có những quy định khó tuân theo hơn. Bây giờ, hãy để những điều này
hướng dẫn đóng vai trò là tiêu chuẩn để đánh giá chất lượng của
Mã JavaScript mà bạn và nhóm của bạn tạo ra.

Một điều nữa: biết những điều này sẽ không ngay lập tức giúp bạn trở thành một phần mềm tốt hơn
nhà phát triển và làm việc với họ trong nhiều năm không có nghĩa là bạn sẽ không đạt được
những sai lầm. Mỗi đoạn mã đều bắt đầu như bản nháp đầu tiên, giống như đất sét ướt
được định hình thành dạng cuối cùng của nó. Cuối cùng, chúng tôi khắc phục những điểm không hoàn hảo khi
chúng tôi xem xét nó với các đồng nghiệp của chúng tôi. Đừng dằn vặt bản thân vì những bản nháp đầu tiên cần thiết
sự cải tiến. Thay vào đó hãy đánh bại mã!

## **Biến**

### Sử dụng tên biến có ý nghĩa và dễ phát âm

**Xấu:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Tốt:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Sử dụng cùng một từ vựng cho cùng một loại biến

**Xấu:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Tốt:**

```javascript
getUser();
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Sử dụng tên có thể tìm kiếm được

Chúng ta sẽ đọc nhiều mã hơn chúng ta sẽ viết. Điều quan trọng là mã chúng tôi
do write có thể đọc được và tìm kiếm được. Bằng cách _not_ đặt tên các biến kết thúc
có ý nghĩa đối với việc hiểu chương trình của chúng tôi, chúng tôi đã làm tổn thương độc giả của mình.
Làm cho tên của bạn có thể tìm kiếm được. Công cụ như
[buddy.js](https://github.com/danielstjules/buddy.js) và
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
có thể giúp xác định các hằng số chưa được đặt tên.

**Xấu:**

```javascript
// 86400000 là cái quái gì thế?
setTimeout(blastOff, 86400000);
```

**Tốt:**

```javascript
// Khai báo chúng dưới dạng các hằng có tên được viết hoa.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Sử dụng các biến giải thích

**Xấu:**

```javascript
địa chỉ const = "Một vòng lặp vô hạn, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Tốt:**

```javascript
địa chỉ const = "Một vòng lặp vô hạn, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, thành phố, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(thành phố, zipCode);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh lập bản đồ tư duy

Rõ ràng là tốt hơn ngầm.

**Xấu:**

```javascript
const location = ["Austin", "New York", "San Francisco"];
địa điểm.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Đợi đã, `l` dùng để làm gì?
  công văn(l);
});
```

**Tốt:**

```javascript
const location = ["Austin", "New York", "San Francisco"];
location.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  công văn(địa điểm);
});
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Không thêm ngữ cảnh không cần thiết

Nếu tên lớp/đối tượng của bạn cho bạn biết điều gì đó, đừng lặp lại điều đó trong
tên biến.

**Xấu:**

```javascript
const Xe = {
  carMake: "Honda",
  xeModel: "Hiệp định",
  Màu xe: "Xanh"
};

chức năng sơnCar(xe, màu) {
  car.carColor = màu sắc;
}
```

**Tốt:**

```javascript
const Xe = {
  tạo: "Honda",
  mô hình: "Hiệp định",
  màu sắc: "Xanh"
};

chức năng sơnCar(xe, màu) {
  car.color = màu sắc;
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Sử dụng tham số mặc định thay vì đoản mạch hoặc điều kiện

Các thông số mặc định thường sạch hơn đoản mạch. Hãy nhận biết rằng nếu bạn
sử dụng chúng, hàm của bạn sẽ chỉ cung cấp các giá trị mặc định cho `không xác định`
tranh luận. Các giá trị "giả" khác như `''`, `""`, `false`, `null`, `0`, và
`NaN`, sẽ không được thay thế bằng giá trị mặc định.

**Xấu:**

```javascript
hàm createMicrobrewery(name) {
  const nhà máy biaName = tên || "Công ty bia Hipster.";
  // ...
}
```

**Tốt:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Chức năng**

### Đối số của hàm (lý tưởng là 2 hoặc ít hơn)

Việc giới hạn số lượng tham số của hàm là vô cùng quan trọng vì nó
làm cho việc kiểm tra chức năng của bạn dễ dàng hơn. Có nhiều hơn ba dẫn đến một
vụ nổ tổ hợp trong đó bạn phải kiểm tra hàng tấn trường hợp khác nhau với
từng đối số riêng biệt.

Một hoặc hai đối số là trường hợp lý tưởng và nên tránh ba đối số nếu có thể.
Bất cứ điều gì nhiều hơn thế nên được củng cố. Thông thường, nếu bạn có
nhiều hơn hai đối số thì hàm của bạn đang cố gắng thực hiện quá nhiều. Trong trường hợp
ở những nơi không có, hầu hết trường hợp một đối tượng cấp cao hơn sẽ đủ làm
lý lẽ.

Vì JavaScript cho phép bạn tạo các đối tượng một cách nhanh chóng mà không cần nhiều lớp
bản soạn sẵn, bạn có thể sử dụng một đối tượng nếu bạn thấy mình cần một
rất nhiều lý lẽ.

Để làm rõ những thuộc tính mà hàm mong đợi, bạn có thể sử dụng ES2015/ES6
cú pháp phá hủy. Điều này có một số lợi thế:

1. Khi ai đó nhìn vào chữ ký hàm, sẽ hiểu ngay đó là gì
   tài sản đang được sử dụng.
2. Nó có thể được sử dụng để mô phỏng các tham số được đặt tên.
3. Việc hủy cấu trúc cũng sao chép các giá trị nguyên thủy được chỉ định của đối số
   đối tượng được truyền vào hàm. Điều này có thể giúp ngăn ngừa tác dụng phụ. Ghi chú:
   các đối tượng và mảng bị hủy cấu trúc khỏi đối tượng đối số KHÔNG
   nhân bản.
4. Linters có thể cảnh báo bạn về những tài sản không được sử dụng, điều này là không thể
   mà không bị phá hủy.

**Xấu:**

```javascript
hàm createMenu(tiêu đề, nội dung, nútText, có thể hủy) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Tốt:**

```javascript
hàm createMenu({ tiêu đề, nội dung, nútText, có thể hủy }) {
  // ...
}

createMenu({
  tiêu đề: "Foo",
  nội dung: "Thanh",
  nútText: "Baz",
  có thể hủy: đúng
});
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Hàm chỉ làm một việc

Đây là quy tắc quan trọng nhất trong công nghệ phần mềm. Khi chức năng
làm nhiều việc thì chúng sẽ khó soạn thảo, kiểm tra và lý luận hơn.
Khi bạn có thể tách một hàm thành một hành động duy nhất, nó có thể được cấu trúc lại
dễ dàng và mã của bạn sẽ đọc rõ ràng hơn nhiều. Nếu bạn không lấy đi thứ gì khác từ
hướng dẫn này, ngoài hướng dẫn này, bạn sẽ đi trước nhiều nhà phát triển.

**Xấu:**

```javascript
chức năng emailClients(client) {
  client.forEach(client => {
    const clientRecord = cơ sở dữ liệu.lookup(client);
    if (clientRecord.isActive()) {
      email khách hàng);
    }
  });
}
```

**Tốt:**

```javascript
hàm emailActiveClients(client) {
  client.filter(isActiveClient).forEach(email);
}

hàm isActiveClient(client) {
  const clientRecord = cơ sở dữ liệu.lookup(client);
  trả về clientRecord.isActive();
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tên hàm phải nói rõ chức năng của chúng

**Xấu:**

```javascript
hàm addToDate(ngày, tháng) {
  // ...
}

const ngày = Ngày mới();

// Thật khó để nói từ tên hàm những gì được thêm vào
addToDate(ngày, 1);
```

**Tốt:**

```javascript
hàm addMonthToDate(tháng, ngày) {
  // ...
}

const ngày = Ngày mới();
addMonthToDate(1, ngày);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Các hàm chỉ nên có một mức độ trừu tượng

Khi bạn có nhiều mức độ trừu tượng, hàm của bạn thường là
làm quá nhiều. Việc chia nhỏ các chức năng dẫn đến khả năng sử dụng lại và dễ dàng hơn
thử nghiệm.

**Xấu:**

```javascript
hàm phân tích cú phápBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  câu lệnh const = code.split(" ");
  mã thông báo const = [];
  REGEXES.forEach(REGEX => {
    câu lệnh.forEach(câu lệnh => {
      // ...
    });
  });

  const ast = [];
  token.forEach(token => {
    // lex...
  });

  ast.forEach(nút => {
    // phân tích...
  });
}
```

**Tốt:**

```javascript
hàm phân tích cú phápBetterJSAlternative(code) {
  mã thông báo const = mã thông báo (mã);
  const cú phápTree = phân tích cú pháp (mã thông báo);
  cú phápTree.forEach(node ​​=> {
    // phân tích...
  });
}

hàm tokenize(code) {
  const REGEXES = [
    // ...
  ];

  câu lệnh const = code.split(" ");
  mã thông báo const = [];
  REGEXES.forEach(REGEX => {
    câu lệnh.forEach(câu lệnh => {
      token.push(/* ... */);
    });
  });

  trả lại token;
}

chức năng phân tích cú pháp (mã thông báo) {
  cú pháp constTree = [];
  token.forEach(token => {
    cú phápTree.push(/* ... */);
  });

  trả về cú phápTree;
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Xóa mã trùng lặp

Hãy cố gắng hết sức để tránh mã trùng lặp. Mã trùng lặp là xấu vì nó
có nghĩa là có nhiều nơi để thay đổi điều gì đó nếu bạn cần thay đổi
logic nào đó.

Hãy tưởng tượng nếu bạn điều hành một nhà hàng và theo dõi hàng tồn kho của mình: tất cả
cà chua, hành, tỏi, gia vị, v.v. Nếu bạn có nhiều danh sách
bạn tiếp tục cài đặt này thì tất cả đều phải được cập nhật khi bạn phục vụ một món ăn với
cà chua trong đó. Nếu bạn chỉ có một danh sách thì chỉ có một nơi để cập nhật!

Thông thường bạn có mã trùng lặp vì bạn có hai hoặc nhiều hơn một chút
những thứ khác nhau, có nhiều điểm chung, nhưng sự khác biệt của chúng buộc bạn phải
có hai hoặc nhiều chức năng riêng biệt thực hiện nhiều công việc giống nhau. Đang xóa
mã trùng lặp có nghĩa là tạo ra một sự trừu tượng hóa có thể xử lý tập hợp này
những thứ khác nhau chỉ với một chức năng/mô-đun/lớp.

Việc có được sự trừu tượng đúng là rất quan trọng, đó là lý do tại sao bạn nên làm theo
Các nguyên tắc RẮN được trình bày trong phần _Classes_. Sự trừu tượng xấu có thể
tệ hơn mã trùng lặp, vì vậy hãy cẩn thận! Đã nói điều này, nếu bạn có thể làm
một sự trừu tượng tốt, hãy làm đi! Đừng lặp lại chính mình, nếu không bạn sẽ tìm thấy chính mình
cập nhật nhiều nơi bất cứ lúc nào bạn muốn thay đổi một điều.

**Xấu:**

```javascript
hàm showDeveloperList(developers) {
  nhà phát triển.forEach(nhà phát triển => {
    const mong đợiSalary = nhà phát triển.calateExpectedSalary();
    const experience = dev.getExperience();
    const githubLink = dev.getGithubLink();
    dữ liệu const = {
      mức lương mong đợi,
      kinh nghiệm,
      githubLiên kết
    };

    kết xuất (dữ liệu);
  });
}

hàm showManagerList(người quản lý) {
  người quản lý.forEach(người quản lý => {
    const mong đợiSalary = manager.calcExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    dữ liệu const = {
      mức lương mong đợi,
      kinh nghiệm,
      danh mục đầu tư
    };

    kết xuất (dữ liệu);
  });
}
```

**Tốt:**

```javascript
hàm showEmployeeList(nhân viên) {
  nhân viên.forEach(nhân viên => {
    const mong đợiSalary = nhân viên. tính toánExpectedSalary();
    const experience = nhân viên.getExperience();

    dữ liệu const = {
      mức lương mong đợi,
      kinh nghiệm
    };

    chuyển đổi (nhân viên.type) {
      trường hợp "người quản lý":
        data.portfolio = nhân viên.getMBAProjects();
        phá vỡ;
      trường hợp "nhà phát triển":
        data.githubLink = nhân viên.getGithubLink();
        phá vỡ;
    }

    kết xuất (dữ liệu);
  });
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Đặt đối tượng mặc định bằng Object.sign

**Xấu:**

```javascript
menu constConfig = {
  tiêu đề: vô giá trị,
  nội dung: "Thanh",
  nútText: null,
  có thể hủy: đúng
};

hàm createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Quán ba";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== không xác định ? config.cancellable : đúng;
}

createMenu(menuConfig);
```

**Tốt:**

```javascript
menu constConfig = {
  tiêu đề: "Đặt hàng",
  // Người dùng không bao gồm khóa 'body'
  nútText: "Gửi",
  có thể hủy: đúng
};

hàm createMenu(config) {
  hãy để FinalConfig = Object.sign(
    {
      tiêu đề: "Foo",
      nội dung: "Thanh",
      nútText: "Baz",
      có thể hủy: đúng
    },
    cấu hình
  );
  trả về cấu hình cuối cùng
  // config bây giờ bằng: {title: "Order", body: "Bar", ButtonText: "Send", cancelable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Không sử dụng cờ làm tham số hàm

Cờ cho người dùng biết rằng chức năng này thực hiện nhiều chức năng. Chức năng nên làm một việc. Tách các hàm của bạn ra nếu chúng đi theo các đường dẫn mã khác nhau dựa trên boolean.

**Xấu:**

```javascript
hàm createFile(name, temp) {
  nếu (tạm thời) {
    fs.create(`./temp/${name}`);
  } khác {
    fs.create(name);
  }
}
```

**Tốt:**

```javascript
hàm createFile(name) {
  fs.create(name);
}

hàm createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh tác dụng phụ (phần 1)

Một hàm tạo ra tác dụng phụ nếu nó làm bất cứ điều gì khác ngoài việc lấy một giá trị trong
và trả về một hoặc nhiều giá trị khác. Tác dụng phụ có thể là ghi vào một tập tin,
sửa đổi một số biến toàn cục hoặc vô tình chuyển toàn bộ số tiền của bạn sang một
người lạ.

Bây giờ, đôi khi bạn cần phải có tác dụng phụ trong một chương trình. Giống như trước đây
Ví dụ, bạn có thể cần ghi vào một tập tin. Những gì bạn muốn làm là
tập trung nơi bạn đang làm việc này. Không có nhiều hàm và lớp
ghi vào một tập tin cụ thể. Có một dịch vụ thực hiện điều đó. Một và chỉ một.

Điểm chính là tránh những cạm bẫy phổ biến như chia sẻ trạng thái giữa các đối tượng
không có bất kỳ cấu trúc nào, sử dụng các kiểu dữ liệu có thể thay đổi mà bất cứ thứ gì cũng có thể ghi vào,
và không tập trung vào nơi xảy ra tác dụng phụ của bạn. Nếu bạn có thể làm được điều này, bạn sẽ
hạnh phúc hơn đại đa số các lập trình viên khác.

**Xấu:**

```javascript
// Biến toàn cục được tham chiếu bởi hàm sau.
// Nếu chúng ta có một hàm khác sử dụng tên này thì bây giờ nó sẽ là một mảng và có thể phá vỡ nó.
let name = "Ryan McDermott";

hàm chiaIntoFirstAndLastName() {
  tên = name.split(" ");
}

chiaIntoFirstAndLastName();

console.log(tên); // ['Ryan', 'McDermott'];
```

**Tốt:**

```javascript
hàm chiaIntoFirstAndLastName(name) {
  tên trả về.split(" ");
}

tên const = "Ryan McDermott";
const newName = SplitIntoFirstAndLastName(name);

console.log(tên); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh tác dụng phụ (phần 2)

Trong JavaScript, một số giá trị không thể thay đổi (immutable) và một số có thể thay đổi được
(có thể thay đổi). Đối tượng và mảng là hai loại giá trị có thể thay đổi nên điều quan trọng là
để xử lý chúng một cách cẩn thận khi chúng được truyền dưới dạng tham số cho hàm. MỘT
Hàm JavaScript có thể thay đổi thuộc tính của đối tượng hoặc thay đổi nội dung của
một mảng có thể dễ dàng gây ra lỗi ở nơi khác.

Giả sử có một hàm chấp nhận một tham số mảng đại diện cho một
giỏ hàng. Nếu hàm thực hiện thay đổi trong mảng giỏ hàng đó -
bằng cách thêm một mặt hàng để mua chẳng hạn - sau đó bất kỳ chức năng nào khác
sử dụng cùng mảng `cart` đó sẽ bị ảnh hưởng bởi sự bổ sung này. Đó có thể là
tuyệt vời, tuy nhiên nó cũng có thể tệ. Hãy tưởng tượng một tình huống xấu:

Người dùng nhấp vào nút "Mua hàng" để gọi chức năng `mua hàng`
tạo ra một yêu cầu mạng và gửi mảng `cart` đến máy chủ. Bởi vì
do kết nối mạng kém, chức năng `purchase` phải tiếp tục thử lại
lời yêu cầu. Bây giờ, điều gì sẽ xảy ra nếu trong lúc đó người dùng vô tình nhấp vào "Thêm vào giỏ hàng"
trên một mục họ không thực sự muốn trước khi yêu cầu mạng bắt đầu?
Nếu điều đó xảy ra và yêu cầu mạng bắt đầu thì chức năng mua hàng đó
sẽ gửi mục vô tình được thêm vào vì mảng `cart` đã được sửa đổi.

Một giải pháp tuyệt vời là hàm `addItemToCart` luôn sao chép
`cart`, chỉnh sửa nó và trả lại bản sao. Điều này sẽ đảm bảo rằng các chức năng vẫn còn
sử dụng giỏ hàng cũ sẽ không bị ảnh hưởng bởi những thay đổi.

Hai lưu ý cần đề cập đến cách tiếp cận này:

1. Có thể có trường hợp bạn thực sự muốn sửa đổi đối tượng đầu vào,
   nhưng khi bạn áp dụng phương pháp lập trình này, bạn sẽ thấy rằng những trường hợp đó
   khá hiếm. Hầu hết mọi thứ có thể được tái cấu trúc để không có tác dụng phụ!

2. Nhân bản các đối tượng lớn có thể rất tốn kém về mặt hiệu suất. May mắn thay,
   đây không phải là một vấn đề lớn trong thực tế bởi vì có
   [thư viện tuyệt vời](https://facebook.github.io/immutable-js/) cho phép
   cách tiếp cận lập trình này nhanh và không tốn nhiều bộ nhớ như
   bạn sẽ phải sao chép thủ công các đối tượng và mảng.

**Xấu:**

```javascript
const addItemToCart = (giỏ hàng, mặt hàng) => {
  cart.push({ item, date: Date.now() });
};
```

**Tốt:**

```javascript
const addItemToCart = (giỏ hàng, mặt hàng) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Không ghi vào các hàm toàn cục

Gây ô nhiễm toàn cầu là một hành vi tồi trong JavaScript vì bạn có thể xung đột với một thế giới khác
thư viện và người dùng API của bạn sẽ không khôn ngoan hơn cho đến khi họ nhận được
ngoại lệ trong sản xuất. Hãy nghĩ về một ví dụ: nếu bạn muốn
mở rộng phương thức Array gốc của JavaScript để có phương thức `diff` có thể
cho thấy sự khác biệt giữa hai mảng? Bạn có thể viết chức năng mới của bạn
vào `Array.prototype`, nhưng nó có thể xung đột với một thư viện khác đã thử
để làm điều tương tự Điều gì sẽ xảy ra nếu thư viện kia chỉ sử dụng `diff` để tìm
sự khác biệt giữa phần tử đầu tiên và phần tử cuối cùng của một mảng? Đây là lý do tại sao nó
sẽ tốt hơn nhiều nếu chỉ sử dụng các lớp ES2015/ES6 và chỉ cần mở rộng `Array` trên toàn cầu.

**Xấu:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = Bộ mới (comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Tốt:**

```javascript
lớp SuperArray mở rộng mảng {
  khác biệt (so sánhArray) {
    const hash = Bộ mới (comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Ưu tiên lập trình chức năng hơn lập trình mệnh lệnh

JavaScript không phải là một ngôn ngữ chức năng như Haskell, nhưng nó có
một hương vị chức năng cho nó. Ngôn ngữ chức năng có thể sạch hơn và dễ kiểm tra hơn.
Hãy ủng hộ phong cách lập trình này khi bạn có thể.

**Xấu:**

```javascript
const lập trìnhOutput = [
  {
    tên: "Chú Bobby",
    dòngOfCode: 500
  },
  {
    tên: "Suzie Q",
    dòngOfCode: 1500
  },
  {
    tên: "Jimmy Gosling",
    dòngOfCode: 150
  },
  {
    tên: "Gracie Hopper",
    dòngOfCode: 1000
  }
];

đặt tổng đầu ra = 0;

for (let i = 0; i < lập trình viênOutput.length; i++) {
  TotalOutput += lập trình viênOutput[i].linesOfCode;
}
```

**Tốt:**

```javascript
const lập trìnhOutput = [
  {
    tên: "Chú Bobby",
    dòngOfCode: 500
  },
  {
    tên: "Suzie Q",
    dòngOfCode: 1500
  },
  {
    tên: "Jimmy Gosling",
    dòngOfCode: 150
  },
  {
    tên: "Gracie Hopper",
    dòngOfCode: 1000
  }
];

const TotalOutput = lập trình viênOutput.reduce(
  (totalLines, đầu ra) => TotalLines + out.linesOfCode,
  0
);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Đóng gói các điều kiện

**Xấu:**

```javascript
if (fsm.state === "đang tìm nạp" && isEmpty(listNode)) {
  // ...
}
```

**Tốt:**

```javascript
hàm nênShowSpinner(fsm, listNode) {
  return fsm.state === "đang tìm nạp" && isEmpty(listNode);
}

if (nênShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh dùng câu điều kiện phủ định

**Xấu:**

```javascript
hàm isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Tốt:**

```javascript
hàm isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh điều kiện

Đây dường như là một nhiệm vụ bất khả thi. Khi lần đầu tiên nghe điều này, hầu hết mọi người đều nói,
"làm sao tôi có thể làm bất cứ điều gì nếu không có câu lệnh `if`?" Câu trả lời là vậy
bạn có thể sử dụng tính đa hình để đạt được nhiệm vụ tương tự trong nhiều trường hợp. Thư hai
câu hỏi thường là "điều đó thật tuyệt nhưng tại sao tôi lại muốn làm điều đó?" Các
Câu trả lời là một khái niệm về clean code trước đây mà chúng ta đã học: một hàm chỉ nên làm
một điều. Khi bạn có các lớp và hàm có câu lệnh `if`, bạn
đang nói với người dùng của bạn rằng chức năng của bạn thực hiện nhiều hơn một việc. Nhớ,
chỉ cần làm một điều

**Xấu:**

```javascript
hạng Máy bay {
  // ...
  getCruisingAltitude() {
    chuyển đổi (this.type) {
      trường hợp "777":
        trả về this.getMaxAltitude() - this.getPassengerCount();
      Vụ án “Không Lực Một”:
        trả về this.getMaxAltitude();
      trường hợp "Cessna":
        trả về this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Tốt:**

```javascript
hạng Máy bay {
  // ...
}

hạng Boeing777 mở rộng Máy bay {
  // ...
  getCruisingAltitude() {
    trả về this.getMaxAltitude() - this.getPassengerCount();
  }
}

lớp AirForceOne mở rộng Máy bay {
  // ...
  getCruisingAltitude() {
    trả về this.getMaxAltitude();
  }
}

lớp Cessna mở rộng Máy bay {
  // ...
  getCruisingAltitude() {
    trả về this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh kiểm tra kiểu (phần 1)

JavaScript không được gõ, có nghĩa là các hàm của bạn có thể nhận bất kỳ loại đối số nào.
Đôi khi bạn bị sự tự do này cắn rứt và điều đó trở nên hấp dẫn để làm điều đó.
kiểm tra kiểu trong hàm của bạn. Có nhiều cách để tránh phải làm điều này.
Điều đầu tiên cần xem xét là các API nhất quán.

**Xấu:**

```javascript
hàm travelToTexas(xe) {
  if (ví dụ xe đạp) {
    Vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (ví dụ phương tiện của Ô tô) {
    Vehicle.drive(this.currentLocation, New Location("texas"));
  }
}
```

**Tốt:**

```javascript
hàm travelToTexas(xe) {
  car.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh kiểm tra kiểu (phần 2)

Nếu bạn đang làm việc với các giá trị nguyên thủy cơ bản như chuỗi và số nguyên,
và bạn không thể sử dụng đa hình nhưng bạn vẫn cảm thấy cần phải kiểm tra kiểu,
bạn nên cân nhắc sử dụng TypeScript. Nó là một sự thay thế tuyệt vời cho bình thường
JavaScript, vì nó cung cấp cho bạn kiểu gõ tĩnh trên JavaScript tiêu chuẩn
cú pháp. Vấn đề với việc kiểm tra kiểu thủ công bằng JavaScript thông thường là
làm tốt việc đó đòi hỏi phải dài dòng nhiều đến mức bạn nhận được "loại an toàn" giả tạo
không bù đắp cho khả năng đọc bị mất. Giữ JavaScript của bạn sạch sẽ, viết
các bài kiểm tra tốt và có đánh giá mã tốt. Nếu không, hãy làm tất cả những điều đó nhưng với
TypeScript (như tôi đã nói, là một sự thay thế tuyệt vời!).

**Xấu:**

```javascript
hàm kết hợp(val1, val2) {
  nếu như (
    (typeof val1 === "số" && typeof val2 === "số") ||
    (typeof val1 === "chuỗi" && typeof val2 === "chuỗi")
  ) {
    trả về giá trị1 + giá trị2;
  }

  ném Lỗi mới ("Phải thuộc loại Chuỗi hoặc Số");
}
```

**Tốt:**

```javascript
hàm kết hợp(val1, val2) {
  trả về giá trị1 + giá trị2;
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Đừng tối ưu hóa quá mức

Các trình duyệt hiện đại thực hiện rất nhiều tối ưu hóa trong thời gian chạy. rất nhiều
Đôi khi, nếu bạn đang tối ưu hóa thì bạn chỉ đang lãng phí thời gian. [Có tốt
tài nguyên](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
để xem thiếu tối ưu hóa ở đâu. Nhắm mục tiêu vào những người đó trong thời gian chờ đợi, cho đến khi
chúng được sửa nếu có thể.

**Xấu:**

```javascript
// Trên các trình duyệt cũ, mỗi lần lặp với `list.length` không được lưu trong bộ nhớ đệm sẽ rất tốn kém
// do tính toán lại `list.length`. Trong các trình duyệt hiện đại, điều này được tối ưu hóa.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Tốt:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Xóa mã chết

Mã chết cũng tệ như mã trùng lặp. Không có lý do gì để giữ nó trong
cơ sở mã của bạn. Nếu nó không được gọi, hãy loại bỏ nó! Sẽ vẫn an toàn
trong lịch sử phiên bản của bạn nếu bạn vẫn cần nó.

**Xấu:**

```javascript
hàm oldRequestModule(url) {
  // ...
}

hàm newRequestModule(url) {
  // ...
}

const req = newRequestModule;
InventoryTracker("táo", req, "www.inventory-awesome.io");
```

**Tốt:**

```javascript
hàm newRequestModule(url) {
  // ...
}

const req = newRequestModule;
InventoryTracker("táo", req, "www.inventory-awesome.io");
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Đối tượng và cấu trúc dữ liệu**

### Sử dụng getters và setters

Sử dụng getters và setters để truy cập dữ liệu trên các đối tượng có thể tốt hơn là chỉ đơn giản
tìm kiếm một thuộc tính trên một đối tượng. "Tại sao?" bạn có thể hỏi. Vâng, đây là một
danh sách không có tổ chức các lý do tại sao:

- Khi bạn muốn làm nhiều hơn ngoài việc có được một thuộc tính đối tượng, bạn không có
  để tra cứu và thay đổi mọi trình truy cập trong cơ sở mã của bạn.
- Làm cho việc thêm xác thực trở nên đơn giản khi thực hiện `set`.
- Đóng gói biểu diễn bên trong.
- Dễ dàng thêm tính năng ghi nhật ký và xử lý lỗi khi nhận và cài đặt.
- Bạn có thể lười tải các thuộc tính của đối tượng, giả sử lấy nó từ một
  máy chủ.

**Xấu:**

```javascript
hàm makeBankAccount() {
  // ...

  trở lại {
    số dư: 0
    // ...
  };
}

tài khoản const = makeBankAccount();
tài khoản.balance = 100;
```

**Tốt:**

```javascript
hàm makeBankAccount() {
  // cái này là riêng tư
  đặt số dư = 0;

  // một "getter", được công khai thông qua đối tượng được trả về bên dưới
  hàm getBalance() {
    trả lại số dư;
  }

  // một "setter", được công khai thông qua đối tượng được trả về bên dưới
  hàm setBalance(số tiền) {
    // ... xác thực trước khi cập nhật số dư
    số dư = số tiền;
  }

  trở lại {
    // ...
    có được số dư,
    bộSố dư
  };
}

tài khoản const = makeBankAccount();
account.setBalance(100);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tạo đối tượng có thành viên riêng tư

Điều này có thể được thực hiện thông qua việc đóng (đối với ES5 trở xuống).

**Xấu:**

```javascript
const Nhân viên = hàm(tên) {
  this.name = tên;
};

Nhân viên.prototype.getName = hàm getName() {
  trả lại cái này.name;
};

const nhân viên = Nhân viên mới("John Doe");
console.log(`Tên nhân viên: ${employee.getName()}`); // Tên nhân viên: John Doe
xóa nhân viên.name;
console.log(`Tên nhân viên: ${employee.getName()}`); // Tên nhân viên: chưa xác định
```

**Tốt:**

```javascript
hàm makeEmployee(name) {
  trở lại {
    getName() {
      trả lại tên;
    }
  };
}

const nhân viên = makeEmployee("John Doe");
console.log(`Tên nhân viên: ${employee.getName()}`); // Tên nhân viên: John Doe
xóa nhân viên.name;
console.log(`Tên nhân viên: ${employee.getName()}`); // Tên nhân viên: John Doe
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Các lớp học**

### Thích các lớp ES2015/ES6 hơn các hàm đơn giản ES5

Rất khó để có được sự kế thừa, xây dựng và phương thức của lớp có thể đọc được
định nghĩa cho các lớp ES5 cổ điển. Nếu bạn cần sự kế thừa (và hãy lưu ý
mà bạn có thể không), thì hãy ưu tiên các lớp ES2015/ES6. Tuy nhiên, thích các chức năng nhỏ hơn
các lớp cho đến khi bạn thấy mình cần các đối tượng lớn hơn và phức tạp hơn.

**Xấu:**

```javascript
const Động vật = hàm(tuổi) {
  if (!(phiên bản này của Animal)) {
    ném Lỗi mới("Khởi tạo động vật bằng `new`");
  }

  this.age = tuổi;
};

Animal.prototype.move = hàm move() {};

const Động vật có vú = function(age, furColor) {
  if (!(trường hợp này của động vật có vú)) {
    ném Lỗi mới("Khởi tạo động vật có vú bằng `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Động vật có vú.prototype = Object.create(Animal.prototype);
Động vật có vú.prototype.constructor = Động vật có vú;
Động vật có vú.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, ngôn ngữSpoken) {
  if (!(trường hợp này của Human)) {
    ném Lỗi mới("Khởi tạo con người bằng `new`");
  }

  Mammal.call(this, age, furColor);
  this.linguSpoken = ngôn ngữSpoken;
};

Human.prototype = Object.create(Động vật có vú.prototype);
Human.prototype.constructor = Con người;
Human.prototype.speak = function speak() {};
```

**Tốt:**

```javascript
lớp Động vật {
  hàm tạo (tuổi) {
    this.age = tuổi;
  }

  di chuyển() {
    /* ... */
  }
}

lớp Động vật có vú mở rộng Động vật {
  hàm tạo (tuổi, furColor) {
    siêu(tuổi);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

lớp Con người mở rộng Động vật có vú {
  hàm tạo (tuổi, furColor, ngôn ngữSpoken) {
    super(tuổi, furColor);
    this.linguSpoken = ngôn ngữSpoken;
  }

  nói chuyện() {
    /* ... */
  }
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Sử dụng chuỗi phương thức

Mẫu này rất hữu ích trong JavaScript và bạn thấy nó trong nhiều thư viện như
như jQuery và Lodash. Nó cho phép mã của bạn mang tính biểu cảm và ít dài dòng hơn.
Vì lý do đó, tôi nói, hãy sử dụng chuỗi phương thức và xem mã của bạn sạch như thế nào
sẽ là. Trong các hàm lớp của bạn, chỉ cần trả về `this` ở cuối mỗi hàm,
và bạn có thể xâu chuỗi các phương thức lớp khác vào đó.

**Xấu:**

```javascript
hạng xe ô tô {
  hàm tạo (tạo, kiểu, màu) {
    this.make = làm;
    this.model = mô hình;
    this.color = màu;
  }

  setMake(làm) {
    this.make = làm;
  }

  setModel(model) {
    this.model = mô hình;
  }

  setColor(màu) {
    this.color = màu;
  }

  cứu() {
    console.log(this.make, this.model, this.color);
  }
}

const car = Xe mới("Ford", "F-150", "đỏ");
car.setColor("hồng");
car.save();
```

**Tốt:**

```javascript
hạng xe ô tô {
  hàm tạo (tạo, kiểu, màu) {
    this.make = làm;
    this.model = mô hình;
    this.color = màu;
  }

  setMake(làm) {
    this.make = làm;
    // LƯU Ý: Trả lại cái này để xâu chuỗi
    trả lại cái này;
  }

  setModel(model) {
    this.model = mô hình;
    // LƯU Ý: Trả lại cái này để xâu chuỗi
    trả lại cái này;
  }

  setColor(màu) {
    this.color = màu;
    // LƯU Ý: Trả lại cái này để xâu chuỗi
    trả lại cái này;
  }

  cứu() {
    console.log(this.make, this.model, this.color);
    // LƯU Ý: Trả lại cái này để xâu chuỗi
    trả lại cái này;
  }
}

const car = Xe mới("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Thích thành phần hơn là kế thừa

Như đã nêu nổi tiếng trong [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) của Gang of Four,
bạn nên ưu tiên thành phần hơn là kế thừa nếu có thể. Có rất nhiều
lý do chính đáng để sử dụng tính kế thừa và rất nhiều lý do chính đáng để sử dụng kết hợp.
Điểm chính của câu châm ngôn này là nếu tâm trí bạn theo bản năng
kế thừa, hãy thử nghĩ xem liệu thành phần có thể mô hình hóa vấn đề của bạn tốt hơn không. Trong một số
trường hợp nó có thể.

Khi đó bạn có thể thắc mắc "khi nào tôi nên sử dụng tính kế thừa?" Nó
phụ thuộc vào vấn đề hiện tại của bạn, nhưng đây là một danh sách phù hợp khi kế thừa
có ý nghĩa hơn thành phần:

1. Quyền thừa kế của bạn đại diện cho mối quan hệ "is-a" chứ không phải "has-a"
   mối quan hệ (Con người-> Động vật so với Người dùng-> Chi tiết người dùng).
2. Bạn có thể sử dụng lại mã từ các lớp cơ sở (Con người có thể di chuyển như mọi loài động vật).
3. Bạn muốn thực hiện những thay đổi toàn cục đối với các lớp dẫn xuất bằng cách thay đổi lớp cơ sở.
   (Thay đổi mức tiêu thụ calo của tất cả các loài động vật khi chúng di chuyển).

**Xấu:**

```javascript
lớp Nhân viên {
  hàm tạo (tên, email) {
    this.name = tên;
    this.email = email;
  }

  // ...
}

// Xấu vì Nhân viên "có" dữ liệu thuế. Nhân viênTaxData không phải là một loại Nhân viên
lớp Nhân viênTaxData mở rộng Nhân viên {
  hàm tạo (ssn, lương) {
    siêu();
    this.ssn = ssn;
    this.salary = lương;
  }

  // ...
}
```

**Tốt:**

```javascript
lớp Nhân viênTaxData {
  hàm tạo (ssn, lương) {
    this.ssn = ssn;
    this.salary = lương;
  }

  // ...
}

lớp Nhân viên {
  hàm tạo (tên, email) {
    this.name = tên;
    this.email = email;
  }

  setTaxData(ssn, lương) {
    this.taxData = new Nhân viênTaxData(ssn, lương);
  }
  // ...
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **CHẤT RẮN**

### Nguyên tắc trách nhiệm duy nhất (SRP)

Như đã nêu trong Clean Code, "Không bao giờ có nhiều hơn một lý do cho một lớp
để thay đổi". Thật hấp dẫn khi nhồi nhét một lớp với nhiều chức năng, như
khi bạn chỉ có thể mang theo một chiếc vali trên chuyến bay của mình. Vấn đề với điều này là
rằng lớp của bạn sẽ không gắn kết về mặt khái niệm và điều đó sẽ có nhiều lý do
thay đổi. Giảm thiểu số lần bạn cần thay đổi lớp học là điều quan trọng.
Điều này quan trọng vì nếu có quá nhiều chức năng trong một lớp và bạn sửa đổi
một phần của nó, có thể khó hiểu điều đó sẽ ảnh hưởng như thế nào đến phần khác
các mô-đun phụ thuộc trong cơ sở mã của bạn.

**Xấu:**

```javascript
lớp Cài đặt người dùng {
  hàm tạo (người dùng) {
    this.user = người dùng;
  }

  thay đổiCài đặt(cài đặt) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  xác minhCredentials() {
    // ...
  }
}
```

**Tốt:**

```javascript
lớp UserAuth {
  hàm tạo (người dùng) {
    this.user = người dùng;
  }

  xác minhCredentials() {
    // ...
  }
}

lớp Cài đặt người dùng {
  hàm tạo (người dùng) {
    this.user = người dùng;
    this.auth = new UserAuth(user);
  }

  thay đổiCài đặt(cài đặt) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Nguyên tắc đóng/mở (OCP)

Như Bertrand Meyer đã nêu, "các thực thể phần mềm (lớp, mô-đun, chức năng,
v.v.) phải mở để mở rộng nhưng đóng để sửa đổi." Điều đó có tác dụng gì
có nghĩa là mặc dù? Nguyên tắc này về cơ bản nói rằng bạn nên cho phép người dùng
thêm các chức năng mới mà không thay đổi mã hiện có.

**Xấu:**

```javascript
lớp AjaxAdapter mở rộng Bộ điều hợp {
  người xây dựng() {
    siêu();
    this.name = "ajaxAdapter";
  }
}

lớp NodeAdapter mở rộng Adaptor {
  người xây dựng() {
    siêu();
    this.name = "nodeAdapter";
  }
}

lớp httpRequester {
  hàm tạo (bộ chuyển đổi) {
    this.adapter = bộ chuyển đổi;
  }

  tìm nạp(url) {
    if (this.adapter.name === "ajaxAdapter") {
      trả về makeAjaxCall(url).then(response => {
        // chuyển đổi phản hồi và trả về
      });
    } else if (this.adapter.name === "nodeAdapter") {
      trả về makeHttpCall(url).then(response => {
        // chuyển đổi phản hồi và trả về
      });
    }
  }
}

hàm makeAjaxCall(url) {
  // yêu cầu và trả lại lời hứa
}

hàm makeHttpCall(url) {
  // yêu cầu và trả lại lời hứa
}
```

**Tốt:**

```javascript
lớp AjaxAdapter mở rộng Bộ điều hợp {
  người xây dựng() {
    siêu();
    this.name = "ajaxAdapter";
  }

  yêu cầu(url) {
    // yêu cầu và trả lại lời hứa
  }
}

lớp NodeAdapter mở rộng Adaptor {
  người xây dựng() {
    siêu();
    this.name = "nodeAdapter";
  }

  yêu cầu(url) {
    // yêu cầu và trả lại lời hứa
  }
}

lớp httpRequester {
  hàm tạo (bộ chuyển đổi) {
    this.adapter = bộ chuyển đổi;
  }

  tìm nạp(url) {
    trả về this.adapter.request(url).then(response => {
      // chuyển đổi phản hồi và trả về
    });
  }
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Nguyên tắc thay thế Liskov (LSP)

Đây là một thuật ngữ đáng sợ cho một khái niệm rất đơn giản. Nó được định nghĩa chính thức là "Nếu S
là một kiểu con của T thì các đối tượng kiểu T có thể được thay thế bằng các đối tượng kiểu S
(tức là các đối tượng loại S có thể thay thế các đối tượng loại T) mà không làm thay đổi bất kỳ
các thuộc tính mong muốn của chương trình đó (tính chính xác, nhiệm vụ được thực hiện,
v.v.)." Đó là một định nghĩa thậm chí còn đáng sợ hơn.

Lời giải thích tốt nhất cho điều này là nếu bạn có một lớp cha và một lớp con,
thì lớp cơ sở và lớp con có thể được sử dụng thay thế cho nhau mà không cần
kết quả sai. Điều này có thể vẫn còn gây nhầm lẫn, vì vậy chúng ta hãy xem xét
ví dụ hình vuông-hình chữ nhật cổ điển. Về mặt toán học, hình vuông là hình chữ nhật, nhưng
nếu bạn lập mô hình nó bằng cách sử dụng mối quan hệ "is-a" thông qua tính kế thừa, bạn sẽ nhanh chóng
gặp rắc rối.

**Xấu:**

```javascript
lớp Hình chữ nhật {
  người xây dựng() {
    this.width = 0;
    cái này.height = 0;
  }

  setColor(màu) {
    // ...
  }

  kết xuất (khu vực) {
    // ...
  }

  setWidth(width) {
    this.width = chiều rộng;
  }

  setHeight(chiều cao) {
    this.height = chiều cao;
  }

  getArea() {
    trả về this.width * this.height;
  }
}

lớp Hình vuông mở rộng Hình chữ nhật {
  setWidth(width) {
    this.width = chiều rộng;
    this.height = chiều rộng;
  }

  setHeight(chiều cao) {
    this.width = chiều cao;
    this.height = chiều cao;
  }
}

hàm renderLargeRectangles(hình chữ nhật) {
  hình chữ nhật.forEach(hình chữ nhật => {
    hình chữ nhật.setWidth(4);
    hình chữ nhật.setHeight(5);
    diện tích const = hình chữ nhật.getArea(); // BAD: Trả về 25 cho Square. Đáng lẽ phải là 20.
    hình chữ nhật.render(diện tích);
  });
}

const hình chữ nhật = [Hình chữ nhật mới(), Hình chữ nhật mới(), Hình vuông mới()];
renderLargeRectangles(hình chữ nhật);
```

**Tốt:**

```javascript
lớp Hình dạng {
  setColor(màu) {
    // ...
  }

  kết xuất (khu vực) {
    // ...
  }
}

lớp Hình chữ nhật mở rộng Hình dạng {
  hàm tạo (chiều rộng, chiều cao) {
    siêu();
    this.width = chiều rộng;
    this.height = chiều cao;
  }

  getArea() {
    trả về this.width * this.height;
  }
}

lớp Quảng trường mở rộng Hình dạng {
  hàm tạo (độ dài) {
    siêu();
    this.length = chiều dài;
  }

  getArea() {
    trả về this.length * this.length;
  }
}

hàm renderLargeShapes(hình dạng) {
  hình dạng.forEach(hình dạng => {
    diện tích const = hình dạng.getArea();
    hình dạng.render(khu vực);
  });
}

const hình = [Hình chữ nhật mới (4, 5), Hình chữ nhật mới (4, 5), Hình vuông mới (5)];
renderLargeShapes(hình);
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Nguyên tắc phân chia giao diện (ISP)

JavaScript không có giao diện nên nguyên tắc này không được áp dụng nghiêm ngặt
như những người khác. Tuy nhiên, điều này quan trọng và phù hợp ngay cả khi JavaScript thiếu
hệ thống kiểu.

ISP tuyên bố rằng "Không nên ép buộc khách hàng phụ thuộc vào các giao diện
họ không sử dụng." Giao diện là những hợp đồng ngầm trong JavaScript vì
gõ vịt.

Một ví dụ điển hình để chứng minh nguyên tắc này trong JavaScript là dành cho
các lớp yêu cầu các đối tượng cài đặt lớn. Không yêu cầu khách hàng thiết lập
số lượng lớn các lựa chọn là có lợi, bởi vì hầu hết thời gian họ sẽ không cần
tất cả các cài đặt. Việc biến chúng thành tùy chọn sẽ giúp ngăn chặn việc có
"giao diện béo".

**Xấu:**

```javascript
lớp DOMTraverser {
  hàm tạo (cài đặt) {
    this.settings = cài đặt;
    this.setup();
  }

  cài đặt() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  đi qua() {
    // ...
  }
}

const $ = DOMTraverser mới({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Hầu hết chúng ta không cần tạo hiệu ứng khi di chuyển.
  // ...
});
```

**Tốt:**

```javascript
lớp DOMTraverser {
  hàm tạo (cài đặt) {
    this.settings = cài đặt;
    this.options = settings.options;
    this.setup();
  }

  cài đặt() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  đi qua() {
    // ...
  }
}

const $ = DOMTraverser mới({
  rootNode: document.getElementsByTagName("body"),
  tùy chọn: {
    hoạt hìnhModule() {}
  }
});
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Nguyên tắc đảo ngược phụ thuộc (DIP)

Nguyên tắc này nêu rõ hai điều thiết yếu:

1. Các mô-đun cấp cao không nên phụ thuộc vào các mô-đun cấp thấp. Cả hai nên
   phụ thuộc vào sự trừu tượng.
2. Sự trừu tượng không nên phụ thuộc vào chi tiết. Chi tiết nên phụ thuộc vào
   trừu tượng.

Điều này ban đầu có thể khó hiểu, nhưng nếu bạn đã từng làm việc với AngularJS,
bạn đã thấy cách triển khai nguyên tắc này dưới dạng Phụ thuộc
Tiêm (DI). Mặc dù chúng không phải là các khái niệm giống hệt nhau nhưng DIP vẫn giữ mức độ cao
mô-đun biết chi tiết về các mô-đun cấp thấp của nó và thiết lập chúng.
Nó có thể thực hiện điều này thông qua DI. Lợi ích to lớn của việc này là nó làm giảm
sự ghép nối giữa các module. Khớp nối là một mô hình phát triển rất xấu bởi vì
nó làm cho mã của bạn khó cấu trúc lại.

Như đã nêu trước đây, JavaScript không có giao diện nên các phần trừu tượng
phụ thuộc vào những hợp đồng ngầm. Nghĩa là, các phương pháp
và các thuộc tính mà một đối tượng/lớp hiển thị cho một đối tượng/lớp khác. bên trong
dụ dưới đây, hợp đồng ngầm định là bất kỳ mô-đun Yêu cầu nào cho một
`InventoryTracker` sẽ có phương thức `requestItems`.

**Xấu:**

```javascript
lớp InventoryRequester {
  người xây dựng() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(mục) {
    // ...
  }
}

lớp InventoryTracker {
  hàm tạo (mục) {
    this.items = vật phẩm;

    // BAD: Chúng tôi đã tạo sự phụ thuộc vào việc triển khai yêu cầu cụ thể.
    // Chúng ta chỉ nên có requestItems phụ thuộc vào phương thức yêu cầu: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const InventoryTracker = new InventoryTracker(["táo", "chuối"]);
InventoryTracker.requestItems();
```

**Tốt:**

```javascript
lớp InventoryTracker {
  hàm tạo (mục, người yêu cầu) {
    this.items = vật phẩm;
    this.requester = người yêu cầu;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

lớp InventoryRequesterV1 {
  người xây dựng() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(mục) {
    // ...
  }
}

lớp InventoryRequesterV2 {
  người xây dựng() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(mục) {
    // ...
  }
}

// Bằng cách xây dựng các phần phụ thuộc từ bên ngoài và đưa chúng vào, chúng ta có thể dễ dàng
// thay thế mô-đun yêu cầu của chúng tôi bằng một mô-đun mới ưa thích sử dụng WebSockets.
const InventoryTracker = InventoryTracker mới(
  ["táo", "chuối"],
  InventoryRequesterV2 mới ()
);
InventoryTracker.requestItems();
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Thử nghiệm**

Kiểm tra quan trọng hơn vận chuyển. Nếu bạn không có bài kiểm tra hoặc
số lượng không đủ thì mỗi lần gửi mã bạn sẽ không chắc chắn rằng mình
không phá vỡ bất cứ điều gì. Quyết định những gì cấu thành một số tiền thích hợp là tùy thuộc vào
với nhóm của bạn, nhưng có mức độ bao phủ 100% (tất cả các câu lệnh và nhánh) là cách
bạn đạt được sự tự tin rất cao và sự an tâm của nhà phát triển. Điều này có nghĩa rằng
Ngoài việc có một framework thử nghiệm tuyệt vời, bạn cũng cần sử dụng một
[công cụ đưa tin tốt](https://gotwarlost.github.io/istanbul/).

Không có lý do gì để không viết bài kiểm tra. Có [rất nhiều khung kiểm tra JS tốt](https://jstherightway.org/#testing-tools), vì vậy hãy tìm một khung mà nhóm của bạn thích.
Khi bạn tìm thấy một cách phù hợp với nhóm của mình, hãy đặt mục tiêu luôn viết bài kiểm tra
cho mọi tính năng/mô-đun mới mà bạn giới thiệu. Nếu phương pháp ưa thích của bạn là
Phát triển dựa trên thử nghiệm (TDD), điều đó thật tuyệt, nhưng điểm chính là chỉ
đảm bảo bạn đang đạt được mục tiêu phủ sóng của mình trước khi khởi chạy bất kỳ tính năng nào,
hoặc tái cấu trúc một cái hiện có.

### Một khái niệm cho mỗi bài kiểm tra

**Xấu:**

```javascript
xác nhận nhập khẩu từ "khẳng định";

mô tả("MomentJS", () => {
  it("xử lý ranh giới ngày", () => {
    hãy hẹn hò;

    ngày = Khoảnh khắc mớiJS ("1/1/2015");
    date.addDays(30);
    khẳng định.equal ("31/1/2015", ngày);

    ngày = Khoảnh khắc mớiJS ("1/2/2016");
    date.addDays(28);
    khẳng định.equal ("29/02/2016", ngày);

    ngày = Khoảnh khắc mớiJS ("1/2/2015");
    date.addDays(28);
    khẳng định.equal ("03/01/2015", ngày);
  });
});
```

**Tốt:**

```javascript
xác nhận nhập khẩu từ "khẳng định";

mô tả("MomentJS", () => {
  it("xử lý các tháng có 30 ngày", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    khẳng định.equal ("31/1/2015", ngày);
  });

  it("xử lý năm nhuận", () => {
    const date = MomentJS mới("1/2/2016");
    date.addDays(28);
    khẳng định.equal ("29/02/2016", ngày);
  });

  it("xử lý năm không nhuận", () => {
    const date = MomentJS mới("1/2/2015");
    date.addDays(28);
    khẳng định.equal ("03/01/2015", ngày);
  });
});
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Đồng thời**

### Sử dụng Lời hứa chứ không phải lệnh gọi lại

Lệnh gọi lại không rõ ràng và chúng gây ra tình trạng lồng nhau quá mức. Với ES2015/ES6,
Lời hứa là một loại toàn cầu được tích hợp sẵn. Sử dụng chúng!

**Xấu:**

```javascript
nhập {get } từ "yêu cầu";
nhập {writeFile } từ "fs";

lấy(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, phản hồi, nội dung) => {
    nếu (requestErr) {
      console.error(requestErr);
    } khác {
      writeFile("article.html", body, writeErr => {
        nếu (writeErr) {
          console.error(writeErr);
        } khác {
          console.log("Tập tin đã được ghi");
        }
      });
    }
  }
);
```

**Tốt:**

```javascript
nhập {get } từ "request-promise";
nhập {writeFile } từ "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(thân => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("Tập tin đã được ghi");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Async/Await thậm chí còn sạch hơn Promises

Lời hứa là một giải pháp thay thế rất rõ ràng cho lệnh gọi lại, nhưng ES2017/ES8 mang đến sự bất đồng bộ và chờ đợi
cung cấp một giải pháp thậm chí còn sạch hơn. Tất cả những gì bạn cần là một hàm có tiền tố
trong từ khóa `async`, sau đó bạn có thể viết logic của mình một cách bắt buộc mà không cần
một chuỗi hàm 'then`. Sử dụng cái này nếu bạn có thể tận dụng các tính năng của ES2017/ES8
Hôm nay!

**Xấu:**

```javascript
nhập {get } từ "request-promise";
nhập {writeFile } từ "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(thân => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("Tập tin đã được ghi");
  })
  .catch(err => {
    console.error(err);
  });
```

**Tốt:**

```javascript
nhập {get } từ "request-promise";
nhập {writeFile } từ "fs-extra";

hàm không đồng bộ getCleanCodeArticle() {
  thử {
    const body = đang chờ lấy(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    đang chờ writeFile("article.html", body);
    console.log("Tập tin đã được ghi");
  } bắt (lỗi) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Xử lý lỗi**

Lỗi ném là một điều tốt! Họ có nghĩa là thời gian chạy đã thành công
được xác định khi có điều gì đó trong chương trình của bạn gặp trục trặc và nó cho phép
bạn biết bằng cách dừng thực thi hàm trên ngăn xếp hiện tại, hủy bỏ
xử lý (trong Nút) và thông báo cho bạn trong bảng điều khiển bằng dấu vết ngăn xếp.

### Đừng bỏ qua các lỗi đã phát hiện

Không làm gì với một lỗi đã phát hiện sẽ không mang lại cho bạn khả năng sửa chữa
hoặc phản ứng với lỗi đã nói. Ghi lỗi vào bảng điều khiển (`console.log`)
không tốt hơn nhiều vì đôi khi nó có thể bị lạc trong một biển thứ được in
đến bảng điều khiển. Nếu bạn bọc bất kỳ đoạn mã nào trong `try/catch` thì điều đó có nghĩa là bạn
nghĩ rằng có thể xảy ra lỗi ở đó và do đó bạn nên có kế hoạch,
hoặc tạo đường dẫn mã khi nó xảy ra.

**Xấu:**

```javascript
thử {
  functionThatMightThrow();
} bắt (lỗi) {
  console.log(lỗi);
}
```

**Tốt:**

```javascript
thử {
  functionThatMightThrow();
} bắt (lỗi) {
  // Một tùy chọn (ồn ào hơn console.log):
  console.error(lỗi);
  // Một lựa chọn khác:
  thông báoUserOfError(lỗi);
  // Một lựa chọn khác:
  báo cáoErrorToService(lỗi);
  // HOẶC làm cả ba!
}
```

### Đừng bỏ qua những lời hứa bị từ chối

Vì lý do tương tự, bạn không nên bỏ qua các lỗi đã phát hiện
từ `thử/bắt`.

**Xấu:**

```javascript
lấy dữ liệu()
  .then(dữ liệu => {
    functionThatMightThrow(data);
  })
  .catch(lỗi => {
    console.log(lỗi);
  });
```

**Tốt:**

```javascript
lấy dữ liệu()
  .then(dữ liệu => {
    functionThatMightThrow(data);
  })
  .catch(lỗi => {
    // Một tùy chọn (ồn ào hơn console.log):
    console.error(lỗi);
    // Một lựa chọn khác:
    thông báoUserOfError(lỗi);
    // Một lựa chọn khác:
    báo cáoErrorToService(lỗi);
    // HOẶC làm cả ba!
  });
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Định dạng**

Định dạng là chủ quan. Giống như nhiều quy tắc ở đây, không có quy tắc cứng và nhanh
quy tắc mà bạn phải tuân theo. Điểm chính là KHÔNG TRANH LUẬN về định dạng.
Có [rất nhiều công cụ](https://standardjs.com/rules.html) để tự động hóa việc này.
Sử dụng một cái! Việc các kỹ sư tranh cãi về định dạng là lãng phí thời gian và tiền bạc.

Đối với những thứ không thuộc phạm vi định dạng tự động
(thụt lề, tab so với dấu cách, dấu ngoặc kép so với dấu ngoặc đơn, v.v.) hãy xem tại đây
để được hướng dẫn.

### Sử dụng cách viết hoa nhất quán

JavaScript không được gõ, vì vậy cách viết hoa cho bạn biết nhiều điều về các biến của bạn,
chức năng, v.v. Những quy tắc này mang tính chủ quan, vì vậy nhóm của bạn có thể chọn bất cứ điều gì
họ muốn. Vấn đề là, bất kể bạn chọn gì, chỉ cần nhất quán.

**Xấu:**

```javascript
hằng NGÀY_IN_WEEK = 7;
hằng số ngàyInMonth = 30;

const song = ["Back In Black", "Nấc thang lên thiên đường", "Hey Jude"];
const Nghệ sĩ = ["ACDC", "Led Zeppelin", "The Beatles"];

hàm erasDatabase() {}
hàm khôi phục_database() {}

lớp động vật {}
lớp Alpaca {}
```

**Tốt:**

```javascript
hằng NGÀY_IN_WEEK = 7;
hằng NGÀY_IN_MONTH = 30;

const SONGS = ["Trở lại màu đen", "Nấc thang lên thiên đường", "Này Jude"];
const NGHỆ SĨ = ["ACDC", "Led Zeppelin", "The Beatles"];

hàm erasDatabase() {}
hàm khôi phụcDatabase() {}

lớp Động vật {}
lớp Alpaca {}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Người gọi hàm và người được gọi phải thân thiết

Nếu một hàm gọi hàm khác, hãy giữ các hàm đó ở gần theo chiều dọc trong nguồn
tài liệu. Lý tưởng nhất là giữ người gọi ngay phía trên người được gọi. Chúng ta có xu hướng đọc mã từ
từ trên xuống dưới, giống như một tờ báo. Vì điều này, hãy làm cho mã của bạn đọc theo cách đó.

**Xấu:**

```javascript
lớp Đánh giá hiệu suất {
  nhà xây dựng (nhân viên) {
    this.employee = nhân viên;
  }

  tra cứuPeers() {
    return db.lookup(this.employee, "peer");
  }

  tra cứuManager() {
    return db.lookup(this.employee, "người quản lý");
  }

  getPeerReviews() {
    const ngang hàng = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(nhân viên);
review.perfReview();
```

**Tốt:**

```javascript
lớp Đánh giá hiệu suất {
  nhà xây dựng (nhân viên) {
    this.employee = nhân viên;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const ngang hàng = this.lookupPeers();
    // ...
  }

  tra cứuPeers() {
    return db.lookup(this.employee, "peer");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  tra cứuManager() {
    return db.lookup(this.employee, "người quản lý");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(nhân viên);
review.perfReview();
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## **Bình luận**

### Chỉ bình luận những thứ có logic nghiệp vụ phức tạp.

Bình luận là một lời xin lỗi, không phải là một yêu cầu. Mã tốt _chủ yếu_ là tài liệu.

**Xấu:**

```javascript
hàm hashIt(dữ liệu) {
  // Hàm băm
  hãy để hàm băm = 0;

  // Độ dài của chuỗi
  độ dài const = data.length;

  // Lặp qua từng ký tự trong dữ liệu
  for (let i = 0; i < length; i++) {
    // Lấy mã ký tự.
    const char = data.charCodeAt(i);
    // Tạo hàm băm
    băm = (băm << 5) - băm + char;
    // Chuyển đổi thành số nguyên 32 bit
    băm &= băm;
  }
}
```

**Tốt:**

```javascript
hàm hashIt(dữ liệu) {
  hãy để hàm băm = 0;
  độ dài const = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    băm = (băm << 5) - băm + char;

    // Chuyển đổi thành số nguyên 32 bit
    băm &= băm;
  }
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Đừng để lại mã đã nhận xét trong cơ sở mã của bạn

Kiểm soát phiên bản tồn tại là có lý do. Để lại mã cũ trong lịch sử của bạn.

**Xấu:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Tốt:**

```javascript
doStuff();
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Không có bình luận tạp chí

Hãy nhớ, sử dụng kiểm soát phiên bản! Không cần mã chết, mã nhận xét,
và đặc biệt là các bình luận tạp chí. Sử dụng `git log` để lấy lịch sử!

**Xấu:**

```javascript
/**
 * 20-12-2016: Loại bỏ monad, không hiểu chúng (RM)
 * 01-10-2016: Được cải tiến bằng cách sử dụng các đơn nguyên đặc biệt (JP)
 * 03-02-2016: Loại bỏ kiểm tra loại (LI)
 * 14-03-2015: Đã thêm kết hợp với kiểm tra loại (JR)
 */
hàm kết hợp(a, b) {
  trả lại a + b;
}
```

**Tốt:**

```javascript
hàm kết hợp(a, b) {
  trả lại a + b;
}
```

**[⬆ quay lại đầu trang](#table-of-contents)**

### Tránh đánh dấu vị trí

Họ thường chỉ thêm tiếng ồn. Đặt các hàm và tên biến cùng với
thụt lề và định dạng thích hợp sẽ mang lại cấu trúc trực quan cho mã của bạn.

**Xấu:**

```javascript
///////////////////////////////////////////////////////////////// //////////////////////////////
// Khởi tạo mô hình phạm vi
///////////////////////////////////////////////////////////////// //////////////////////////////
$scope.model = {
  thực đơn: "foo",
  điều hướng: "thanh"
};

///////////////////////////////////////////////////////////////// //////////////////////////////
// Thiết lập hành động
///////////////////////////////////////////////////////////////// //////////////////////////////
hành động const = function() {
  // ...
};
```

**Tốt:**

```javascript
$scope.model = {
  thực đơn: "foo",
  điều hướng: "thanh"
};

hành động const = function() {
  // ...
};
```

**[⬆ quay lại đầu trang](#table-of-contents)**

## Dịch

Điều này cũng có sẵn trong các ngôn ngữ khác:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenia**: [hanumanum/clean-code-javascript/] (https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code- javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Tiếng Bồ Đào Nha Brazil**: [fesnt/clean-code-javascript] (https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Tiếng Trung giản thể**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Tiếng Trung phồn thể**: [AllJointTW/clean-code-javascript] (https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **Tiếng Pháp**: [eugene-augier/clean-code-javascript -fr](https://github.com/eugene-augier/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Đức.png) **Tiếng Đức**: [marcbruederlin/clean-code-javascript]( https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/] (https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Tiếng Ý**: [frappacchio/clean-code-javascript/] (https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/ Japan.png) **Tiếng Nhật**: [mitsuruog/clean-code-javascript/] (https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Tiếng Hàn**: [qkraudghgh/clean-code-javascript -ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Tiếng Ba Lan**: [greg-dev/clean-code-javascript -pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Nga.png) **Tiếng Nga**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Tiếng Tây Ban Nha**: [tureey/clean-code-javascript]( https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Tiếng Tây Ban Nha**: [andersontr15/clean-code-javascript]( https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbia**: [doskovicmilos/clean-code-javascript/] (https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Tiếng Thổ Nhĩ Kỳ**: [bsonmez/clean-code-javascript]( https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Tiếng Ukraina**: [mindfr1k/clean-code-javascript-ua ](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/vietnam.png) **Tiếng Việt**: [hienvd/clean-code-javascript/] (https://github.com/hienvd/clean-code-javascript/)
- ![ir](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Iran.png) **Tiếng Ba Tư**: [hamettio/clean-code-javascript]( https://github.com/hamettio/clean-code-javascript)

**[⬆ quay lại đầu trang](#table-of-contents)**
