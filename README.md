# 🛒 Mini E-Commerce tích hợp thanh toán Smart Contract (Hybrid DApp)

Dự án xây dựng hệ thống thương mại điện tử mini áp dụng mô hình ứng dụng phân tán lai (Hybrid DApp). Hệ thống kết hợp giữa kiến trúc Web truyền thống (Off-chain) để quản lý dữ liệu sản phẩm, người dùng và Blockchain (On-chain) để thực hiện giao dịch thanh toán minh bạch, tự động thông qua Smart Contract.

---

##  Giới thiệu & Bài toán giải quyết

* **Bài toán:** Các hệ thống thương mại điện tử truyền thống phụ thuộc hoàn toàn vào cổng thanh toán trung gian tập trung. Ngược lại, nếu đưa toàn bộ dữ liệu (hình ảnh, danh mục, thông tin người dùng) lên Blockchain sẽ tốn chi phí gas rất lớn và tốc độ xử lý chậm.
* **Giải pháp Hybrid DApp:** 
  * **Off-chain (Cơ sở dữ liệu truyền thống):** Lưu trữ danh mục sản phẩm, người dùng, chi tiết đơn hàng nhằm tối ưu hiệu năng và chi phí.
  * **On-chain (Blockchain & Smart Contract):** Xử lý luồng nhận tiền thanh toán, phát sự kiện và đảm bảo tính bất biến, minh bạch của giao dịch tài chính.

---

## 🏛️ Kiến trúc hệ thống (Hybrid Architecture)
[ Frontend: ReactJS + Web3/MetaMask ]
│                       │
│ (REST API)            │ (Gửi giao dịch thanh toán)
▼                       ▼
[ Backend: Node.js / ASP.NET ]  [ Smart Contract (Ethereum Sepolia Testnet) ]
│                       │
│ (Lưu/Đọc dữ liệu)     │ (Phát Event PaymentCompleted)
▼                       ▼
[ MySQL Database ] <──────────┘ (Backend Listener bắt sự kiện và cập nhật đơn hàng)
### Luồng hoạt động chính:
1. **Duyệt hàng & Đặt hàng:** Người mua xem danh mục hàng hóa, thêm vào giỏ và nhấn tạo đơn hàng. Backend tạo đơn hàng trong MySQL với trạng thái mặc định: `Chờ thanh toán` (`Pending`).
2. **Thanh toán Web3:** Frontend lấy `OrderID` và tổng số tiền từ Backend, yêu cầu người dùng xác nhận giao dịch qua ví MetaMask tới Smart Contract.
3. **Thực thi On-chain:** Smart Contract nhận tiền điện tử (ETH Sepolia testnet) và phát ra sự kiện (`Event`) `PaymentCompleted(orderId, buyerAddress, amount, timestamp)`.
4. **Đồng bộ Off-chain:** Backend (sử dụng `ethers.js`/`web3.js`) lắng nghe sự kiện từ Contract, tự động cập nhật trạng thái đơn hàng trong MySQL sang `Đã thanh toán` (`Paid`), đồng thời khóa tính năng hủy đơn của khách hàng.

---

## 🛠️ Công nghệ sử dụng

* **Blockchain & Smart Contract:** Solidity, Remix IDE / Hardhat, Sepolia Testnet, MetaMask.
* **Web3 Integration:** Ethers.js / Web3.js.
* **Backend:** Node.js (Express) hoặc C# ASP.NET Core MVC.
* **Database:** MySQL.
* **Frontend:** ReactJS, HTML5/CSS3, Axios.

---

## 👥 Phân công nhiệm vụ thành viên

| Vai trò | Thành viên | Nhiệm vụ chính |
| :--- | :--- | :--- |
| **Thành viên 1** | *[Điền Họ và Tên]* | **Database & Smart Contract (Data Layer)**<br>- Thiết kế ERD và viết script tạo bảng MySQL (`Users`, `Products`, `Orders` có áp dụng soft delete).<br>- Viết Smart Contract bằng Solidity nhận tiền và phát event `PaymentCompleted`.<br>- Deploy Smart Contract lên mạng thử nghiệm Sepolia, trích xuất địa chỉ Contract và ABI. |
| **Thành viên 2** | *[Điền Họ và Tên]* | **Backend Developer (Off-chain Logic)**<br>- Xây dựng RESTful API CRUD cho sản phẩm, giỏ hàng, tạo đơn hàng.<br>- Xây dựng module `Blockchain Listener` liên tục lắng nghe event từ Smart Contract.<br>- Cập nhật trạng thái đơn hàng sang `Đã thanh toán` và chặn hủy đơn sau thanh toán. |
| **Thành viên 3** | *[Điền Họ và Tên]* | **Frontend Developer (UI/UX & Web3)**<br>- Thiết kế giao diện ReactJS cho trang sản phẩm, chi tiết giỏ hàng và thanh toán.<br>- Tích hợp kết nối ví MetaMask cho khách hàng.<br>- Gọi hàm thanh toán trên Smart Contract, hiển thị trạng thái giao dịch (chờ xác nhận, hoàn tất). |

---

## 📂 Cấu trúc thư mục mã nguồn

```text
├── database/            # Script tạo bảng MySQL, lược đồ quan hệ ERD
├── smart-contract/      # Mã nguồn Solidity (.sol), file ABI, địa chỉ Contract
├── backend/             # RESTful API, bộ lắng nghe sự kiện Blockchain
├── frontend/            # Giao diện ứng dụng ReactJS & tích hợp ví MetaMask
└── README.md            # Tài liệu tổng quan dự án
