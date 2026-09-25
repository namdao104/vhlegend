# Nối form Liên hệ về Google Sheet

Form trên web hiện chưa có nơi nhận dữ liệu. Trong lúc chờ, khi khách bấm gửi
thì trình duyệt của họ mở ứng dụng email với thông tin đã điền sẵn — không mất
dữ liệu, nhưng khách phải bấm gửi lần nữa, và ai không cài ứng dụng email sẽ
gặp trục trặc.

Làm 6 bước dưới đây để mọi yêu cầu tự chảy vào một Google Sheet của anh. Không
cần dịch vụ bên thứ ba, không mất phí.

---

## 1. Tạo Google Sheet

Vào `sheets.new`, đặt tên ví dụ **VH Legend — Website enquiries**.

Dòng đầu tiên gõ đúng sáu tiêu đề này, mỗi chữ một ô:

```
timestamp | name | company | country | email | phone | page
```

## 2. Mở trình soạn mã

Trong Sheet: menu **Tiện ích mở rộng → Apps Script**.

## 3. Dán đoạn mã

Xoá hết nội dung có sẵn, dán đoạn này vào:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
  var d = JSON.parse(e.postData.contents);
  sheet.appendRow([
    new Date(), d.name || '', d.company || '', d.country || '',
    d.email || '', d.phone || '', d.page || ''
  ]);

  // báo email cho đội bán hàng
  MailApp.sendEmail({
    to: 'RLee@vhlegend.com, RAng@vhlegend.com',
    subject: 'Website enquiry — ' + (d.company || d.name),
    body: 'Name: ' + d.name + '\nCompany: ' + d.company +
          '\nCountry: ' + d.country + '\nEmail: ' + d.email +
          '\nPhone: ' + d.phone + '\nPage: ' + d.page
  });

  return ContentService.createTextOutput('ok');
}
```

## 4. Triển khai

Bấm **Deploy → New deployment**.

- Select type: chọn **Web app**
- Execute as: **Me**
- Who has access: **Anyone**  ← bắt buộc, nếu để "Anyone with Google account"
  thì khách không gửi được

Bấm Deploy. Google sẽ hỏi cấp quyền — đồng ý. Lần đầu có màn hình cảnh báo
"Google hasn't verified this app", bấm **Advanced → Go to ...** rồi Allow. Đây
là script của chính anh nên an toàn.

## 5. Copy đường dẫn

Sau khi deploy, Google cho một đường dẫn dạng:

```
https://script.google.com/macros/s/AKfycb.../exec
```

## 6. Dán vào website

Mở `index.html`, tìm dòng này (có ở cả ba bản `index.html`, `vi/index.html`,
`ko/index.html`):

```javascript
var ENQUIRY_ENDPOINT = '';           // <-- paste the web-app URL here
```

Dán đường dẫn vào giữa hai dấu nháy:

```javascript
var ENQUIRY_ENDPOINT = 'https://script.google.com/macros/s/AKfycb.../exec';
```

Upload lại ba file. Xong.

---

## Kiểm tra

Mở web, bấm Contact us, điền thử và gửi. Trong vài giây:

- Google Sheet có thêm một dòng
- Hộp thư RLee@ và RAng@ nhận được email báo

Nếu Sheet không có dòng nào, kiểm lại bước 4 — hầu hết lỗi nằm ở mục
"Who has access".

## Lưu ý

- Mỗi lần sửa đoạn mã phải **Deploy → Manage deployments → Edit → New version**,
  không thì bản cũ vẫn chạy.
- Google Workspace cho phép gửi 1.500 email/ngày qua Apps Script, thừa sức cho
  lượng yêu cầu của một trang B2B.
- Sheet này chứa thông tin liên hệ của khách. Chỉ chia sẻ cho người cần dùng.
