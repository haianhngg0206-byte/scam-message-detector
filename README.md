# Dịch vụ phát hiện tin nhắn lừa đảo

Dự án mẫu cho bài tập MLOps: giao diện web, API FastAPI, thử nghiệm A/B giữa một mô hình phân loại văn bản từ Hugging Face (Model A) và một baseline từ khóa (Model B), chạy bằng Docker Compose và có CI cơ bản trên GitHub Actions.

## 1. Chức năng
- Nhập tin nhắn và dự đoán nguy cơ lừa đảo/spam.
- Chạy Model A, Model B hoặc so sánh cả hai.
- Hiển thị nhãn, điểm dự đoán và thời gian chạy.
- API kiểm tra trạng thái và endpoint dự đoán.
- Kiểm thử API tự động và workflow CI trên GitHub Actions.

## 2. Chạy bằng Docker
Cài Docker Desktop, mở terminal tại thư mục dự án rồi chạy:

```bash
docker compose up --build
```

Mở trình duyệt tại http://localhost:8000

Dừng hệ thống:
```bash
docker compose down
```

Lần đầu chạy, Model A cần tải model từ Hugging Face nên có thể mất thời gian và cần kết nối Internet.

## 3. Chạy trực tiếp bằng Python
Khuyến nghị Python 3.11. Tại thư mục dự án:

```bash
python -m venv .venv
```

Windows:
```bash
.venv\\Scripts\\activate
```

macOS/Linux:
```bash
source .venv/bin/activate
```

Cài thư viện và chạy:
```bash
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Giao diện: http://localhost:8000  
Tài liệu API: http://localhost:8000/docs  
Health check: http://localhost:8000/health

## 4. API
`POST /api/predict`

Ví dụ JSON:
```json
{
  "text": "Ban da trung thuong, bam vao link de nhan qua",
  "model": "both"
}
```

`model` nhận `A`, `B` hoặc `both`.

## 5. A/B testing và đánh giá
- Model A: mô hình phân loại tin nhắn tải từ Hugging Face.
- Model B: baseline dựa trên từ khóa, dùng làm mốc so sánh ban đầu.
- API trả về nhãn và điểm dự đoán; thời gian phản hồi được ghi cho từng model.
- Để báo cáo chính xác, nhóm cần chuẩn bị bộ dữ liệu kiểm thử có nhãn thật, chạy cả hai model và tính Accuracy, Precision, Recall, F1-score, cùng thời gian phản hồi trung bình. Không dùng vài ví dụ thủ công để kết luận model nào tốt hơn.

**Lưu ý:** Model B hiện là baseline theo từ khóa, không phải mô hình học máy thứ hai. Nếu giảng viên yêu cầu hai mô hình Hugging Face, nhóm có thể thay hàm `predict_baseline` bằng model HF thứ hai và cập nhật phần mô tả này.

## 6. Chạy kiểm thử
```bash
pytest -q
```

## 7. Đưa lên GitHub
1. Tạo repository mới trên GitHub.
2. Mở terminal tại thư mục dự án.
3. Chạy:

```bash
git init
git add .
git commit -m "Initial MLOps project"
git branch -M main
git remote add origin https://github.com/<ten-tai-khoan>/<ten-repository>.git
git push -u origin main
```

Thay URL bằng URL repository thật của nhóm.

## 8. Cấu trúc thư mục
```text
app/
  main.py
  models.py
  static/index.html
tests/test_api.py
.github/workflows/ci.yml
Dockerfile
docker-compose.yml
requirements.txt
README.md
```

## Lưu ý về kết quả
Đây là mã nguồn mẫu học tập, không nên sử dụng để tự động kết luận tin nhắn là lừa đảo. Model A có thể trả nhãn khác nhau tùy cách model được huấn luyện; hãy kiểm tra nhãn gốc và đánh giá bằng bộ dữ liệu có nhãn trước khi viết kết luận.
