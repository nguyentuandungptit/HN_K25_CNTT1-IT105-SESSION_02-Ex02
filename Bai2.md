# Báo cáo Phân tích & Thiết kế Hệ thống Thông tin RikkeiExpress
**Môn học:** Phân tích & Thiết kế Hệ thống Thông tin (IT105)  
**Bài tập:** Session 02 - Bài 2 (Vận dụng cơ bản)
 
---

## PHẦN 1: BÁO CÁO PHÂN TÍCH VÀ CHỮA LỖI PHÂN LOẠI HTTT

### 1. Phân tích các điểm sai trong bản phân loại của Thực tập sinh BA

Trong bản phân loại ban đầu, Thực tập sinh BA đã mắc một số sai sót nghiêm trọng do chưa nắm vững bản chất, mục tiêu và đối tượng người dùng của 3 cấp độ Hệ thống Thông tin (HTTT):

1. **Sai sót 1 (Ghi nhận ở nhóm TPS):**
   - *Tính năng bị xếp sai:* "Báo cáo tổng hợp doanh thu tháng cho Trưởng bưu cục" và "Công cụ phân tích dự báo điểm nóng quá tải đơn hàng mùa Tết".
   - *Nguyên nhân sai:* 
     - **Báo cáo tổng hợp doanh thu tháng** là dữ liệu gom nhóm, định kỳ phục vụ cấp quản lý (Trưởng bưu cục) đánh giá hiệu quả hoạt động, đây là đặc trưng của **MIS (Management Information System)**, không phải giao dịch tác nghiệp hàng ngày.
     - **Công cụ phân tích dự báo điểm nóng quá tải** sử dụng dữ liệu lịch sử và các mô hình dự báo để hỗ trợ ra quyết định chiến lược/điều phối tài nguyên, thuộc nhóm **DSS (Decision Support System)**.
   
2. **Sai sót 2 (Ghi nhận ở nhóm MIS):**
   - *Tính năng bị xếp sai:* "Tài xế bấm nút 'Đã lấy hàng' trên App Mobile" và "Nhân viên kho quét mã vạch nhập kho".
   - *Nguyên nhân sai:* Đây là các hành động cập nhật trạng thái đơn hàng trực tiếp theo thời gian thực (real-time transaction execution) của người lao động tác nghiệp (Tài xế, Nhân viên kho). Mục tiêu là cập nhật dữ liệu giao dịch chính xác và nhanh chóng. Do đó, đây là các tính năng cốt lõi của **TPS (Transaction Processing System)**, không phải MIS.

3. **Sai sót 3 (Ghi nhận ở nhóm DSS):**
   - *Tính năng bị xếp sai:* "In phiếu cước giao hàng cho khách tại bưu cục".
   - *Nguyên nhân sai:* Việc tính cước và in phiếu là hoạt động giao dịch trực tiếp với khách hàng tại bưu cục do nhân viên thu ngân/tác nghiệp thực hiện. Đây là quy trình xử lý giao dịch cơ bản của **TPS (Transaction Processing System)**, hoàn toàn không chứa yếu tố phân tích hay hỗ trợ quyết định (DSS).

---

### 2. Bảng phân định chuẩn xác 3 nhóm TPS, MIS, DSS cho RikkeiExpress

| Nhóm HTTT | Đối tượng người dùng | Mục tiêu chính | Tính năng chuẩn xác cho RikkeiExpress |
| :--- | :--- | :--- | :--- |
| **TPS** *(Transaction Processing System)* | Nhân viên thu ngân, Nhân viên kho, Tài xế giao hàng | Xử lý các giao dịch phát sinh hàng ngày với tốc độ cao, độ chính xác tuyệt đối và cập nhật thời gian thực. | 1. Tài xế bấm nút "Đã lấy hàng" trên App Mobile.<br>2. Nhân viên kho quét mã vạch nhập kho.<br>3. In phiếu cước giao hàng cho khách tại bưu cục. |
| **MIS** *(Management Information System)* | Quản lý bưu cục, Trưởng vùng | Tổng hợp, thống kê dữ liệu từ TPS thành các báo cáo định kỳ/đột xuất hỗ trợ công tác giám sát, kiểm soát hoạt động. | 1. Báo cáo tổng hợp doanh thu tháng cho Trưởng bưu cục.<br>2. Báo cáo thống kê số lượng đơn giao thành công/thất bại tuần trước. |
| **DSS** *(Decision Support System)* | Ban Giám đốc, Hội đồng Chiến lược | Phân tích dữ liệu lịch sử, dự báo xu hướng và chạy mô hình giả định (What-If) hỗ trợ ra quyết định chiến lược. | 1. Công cụ phân tích dự báo điểm nóng quá tải đơn hàng mùa Tết.<br>2. Hệ thống gợi ý tối ưu hóa mạng lưới mở thêm bưu cục mới dựa trên dữ liệu 3 năm. |

---

## PHẦN 2: GIẢI THÍCH NGHIỆP VỤ & MÃ NGUỒN KIỂM TRA (PYTHON)

### 1. Giải thích lý do tính năng TPS đòi hỏi tốc độ xử lý nhanh và độ chính xác tuyệt đối

1. **Khối lượng giao dịch cực lớn và liên tục (High Volume & Real-time):**
   - Chuỗi giao vận RikkeiExpress xử lý hàng triệu đơn hàng mỗi ngày. Mỗi thao tác quét mã vạch, bấm "Đã lấy hàng", hay in phiếu cước đều phát sinh dữ liệu ngay lập tức. Tốc độ xử lý nhanh (tính bằng milisecond) giúp giảm thiểu thời gian chờ đợi tại kho/bưu cục, tránh ùn tắc chuỗi cung ứng.

2. **Dữ liệu nền tảng cho các hệ thống cấp trên (Foundation for MIS & DSS):**
   - TPS là "nguồn cấp dữ liệu đầu vào" (Data Source) duy nhất cho MIS và DSS. Nếu dữ liệu TPS bị sai lệch (như sai trọng lượng, sai số tiền cước, nhầm trạng thái đơn), toàn bộ báo cáo doanh thu của MIS và dự báo của DSS sẽ bị sai dây chuyền ("Garbage in, Garbage out").

3. **Tính toàn vẹn dữ liệu và trải nghiệm khách hàng:**
   - Xử lý giao dịch đòi hỏi tuân thủ nghiêm ngặt tính chất ACID (Atomicity, Consistency, Isolation, Durability) trong cơ sở dữ liệu. Bất kỳ lỗi mất dữ liệu hay sai sót tính cước nào cũng làm mất uy tín thương hiệu RikkeiExpress và gây thất thoát tài chính trực tiếp.

---

### 2. Chương trình Python kiểm tra và phân loại tính năng tự động

Dưới đây là mã nguồn Python thực thi hàm `classify_logistics_feature(feature_name)` sử dụng kỹ thuật phân tích từ khóa nghiệp vụ (Keyword Analysis & Heuristics) kết hợp chuẩn hóa văn bản.

```python
import re

def classify_logistics_feature(feature_name: str) -> str:
    """
    Tự động phân loại tính năng phần mềm Logistics vào nhóm TPS, MIS hoặc DSS.
    
    Parameters:
        feature_name (str): Tên hoặc mô tả tính năng cần phân loại.
        
    Returns:
        str: Kết quả phân loại kèm giải thích ngắn gọn.
    """
    if not feature_name or not isinstance(feature_name, str):
        return "Lỗi: Tên tính năng không hợp lệ."

    text = feature_name.lower().strip()

    # Từ khóa đặc trưng nhóm DSS (Hỗ trợ quyết định / Phân tích dự báo / Chiến lược)
    dss_keywords = [
        "dự báo", "phân tích dự báo", "mô hình", "điểm nóng quá tải", 
        "tối ưu hóa mạng lưới", "mở bưu cục", "chiến lược", "what-if", 
        "gợi ý chiến lược", "xu hướng"
    ]

    # Từ khóa đặc trưng nhóm MIS (Báo cáo quản lý / Thống kê tổng hợp / Định kỳ)
    mis_keywords = [
        "báo cáo", "tổng hợp", "thống kê", "doanh thu tháng", 
        "định kỳ", "báo cáo tuần", "giám sát", "bảng điều khiển"
    ]

    # Từ khóa đặc trưng nhóm TPS (Xử lý giao dịch tác nghiệp hàng ngày)
    tps_keywords = [
        "quét mã", "bấm nút", "đã lấy hàng", "nhập kho", "xuất kho", 
        "in phiếu", "tính cước", "giao hàng", "thu tiền", "cập nhật trạng thái",
        "tạo đơn", "thanh toán"
    ]

    # Ưu tiên kiểm tra DSS (Tính chất phân tích cao nhất)
    for kw in dss_keywords:
        if kw in text:
            return (f"Feature: '{feature_name}'\n"
                    f"-> Nhóm: DSS (Decision Support System - Hỗ trợ quyết định)\n"
                    f"-> Mục tiêu: Hỗ trợ Ban Giám đốc phân tích dự báo và ra quyết định chiến lược.\n")

    # Kiểm tra MIS (Báo cáo tổng hợp/thống kê)
    for kw in mis_keywords:
        if kw in text:
            return (f"Feature: '{feature_name}'\n"
                    f"-> Nhóm: MIS (Management Information System - Thông tin quản lý)\n"
                    f"-> Mục tiêu: Cung cấp báo cáo thống kê định kỳ cho Quản lý bưu cục / Trưởng vùng.\n")

    # Kiểm tra TPS (Giao dịch tác nghiệp)
    for kw in tps_keywords:
        if kw in text:
            return (f"Feature: '{feature_name}'\n"
                    f"-> Nhóm: TPS (Transaction Processing System - Xử lý giao dịch tác nghiệp)\n"
                    f"-> Mục tiêu: Hỗ trợ nhân viên tác nghiệp xử lý giao dịch hàng ngày nhanh chóng, chính xác.\n")

    return (f"Feature: '{feature_name}'\n"
            f"-> Nhóm: KHÔNG XÁC ĐỊNH (Cần cung cấp thêm chi tiết nghiệp vụ).\n")


# --- TEST CASES VỚI CÁC TÍNH NĂNG TRONG BÀI TOÁN ---
if __name__ == "__main__":
    test_features = [
        "Báo cáo tổng hợp doanh thu tháng cho Trưởng bưu cục",
        "Công cụ phân tích dự báo điểm nóng quá tải đơn hàng mùa Tết",
        "Tài xế bấm nút 'Đã lấy hàng' trên App Mobile",
        "Nhân viên kho quét mã vạch nhập kho",
        "In phiếu cước giao hàng cho khách tại bưu cục",
        "Báo cáo thống kê số lượng đơn giao tuần trước",
        "Hệ thống gợi ý tối ưu hóa mạng lưới mở bưu cục mới"
    ]

    print("=" * 70)
    print("KẾT QUẢ KIỂM TRA TỰ ĐỘNG PHÂN LOẠI TÍNH NĂNG RIKKEIEXPRESS")
    print("=" * 70 + "\n")

    for feature in test_features:
        print(classify_logistics_feature(feature))
        print("-" * 70)
