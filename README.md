# Vietnamese Semantic Role Labeling (VSRL) qua phương pháp Label Projection

Đây là đề tài nghiên cứu xây dựng bộ gán nhãn vai nghĩa (Semantic Role Labeling) cho tiếng Việt. Đề tài được thực hiện bởi nhóm sinh viên khoa Toán – Cơ – Tin học, Trường Đại học Khoa học Tự nhiên (VNU-HUS) gồm: Nguyễn Mai Hoàng Anh-23001824, Vũ Thị Hà-23001866, Nguyễn Thị Huyền Linh-23001866

## Tổng quan phương pháp (Pipeline)
Do hạn chế về tài nguyên gán nhãn cho tiếng Việt, dự án sử dụng phương pháp phóng chiếu nhãn (Cross-lingual Label Projection) từ tiếng Anh sang tiếng Việt thông qua các bước:
1. **Data Extraction**: Trích xuất dữ liệu câu từ tập corpus `vtb-*.conll`.
2. **Translation**: Dịch các câu tiếng Việt sang tiếng Anh sử dụng Google Translate API.
3. **English SRL Tagging**: Sử dụng `allennlp` để gán nhãn SRL cho các câu tiếng Anh.
4. **Word Alignment**: Dùng `SimAlign` (dựa trên mô hình mBERT) để gióng hàng từ vựng giữa câu Việt - Anh.
5. **Label Projection**: Phóng chiếu nhãn từ token tiếng Anh sang token tiếng Việt tương ứng.
6. **POS Filtering**: Lọc nhiễu bằng từ loại (POS Tagging) thông qua thư viện `underthesea` để loại bỏ các vị từ không hợp lệ.
7. **Evaluation**: Đánh giá F1 Score dựa trên tập Gold data.

## Cấu trúc thư mục (Project Structure)

```text
VSRL/
│   .gitignore
│   README.md
│   requirements.txt
│
├───.idea
└───src
    │   english_srl.ipynb
    │   VSRL.ipynb
    │
    ├───.ipynb_checkpoints
    │       VSRL-checkpoint.ipynb
    ├───.vscode
    ├───data
    │   ├───gold
    │   │       1000_sentences.txt
    │   │       gold_labels.json
    │   ├───silver
    │   │       english_labels_2.json
    │   │       silver_filtered_2.json
    │   │       silver_raw_2.json
    │   ├───translation
    │   │       en_sentences.txt
    │   │       translation_pair.json
    │   └───VTB-SRL
    │       │   vtb-00.conll
    │       │   vtb-01.conll
    │       │   vtb-02.conll
    │       │   vtb-03.conll
    │       │   vtb-10.conll
    │       │   vtb-11.conll
    │       │   vtb-12.conll
    │       │   vtb-13.conll
    │       │   vtb-14.conll
    │       │   vtb-15.conll
    │       └───.ipynb_checkpoints
    │               vtb-00-checkpoint.conll
    │
    └───reports
            alignment_2.json
            evaluation_report_v2.json
            translation_log.json
```

## Hướng dẫn cài đặt

**Bước 1:** Clone repository
```bash
git clone https://github.com/Vha410/VietNamese-SRL-Projection.git

cd VietNamese-SRL-Projection
```

**Bước 2:** Cài đặt các thư viện cần thiết
```bash
pip install -r requirements.txt
```
*(Lưu ý: Khuyến nghị sử dụng môi trường ảo Python 3.8 cho module `english_srl` vì `allennlp` có các dependency đặc thù).*

## Kết quả đánh giá
Tham khảo chi tiết tại `src/reports/evaluation_report_v2.json`. Pipeline hỗ trợ phân tích chi tiết F1 Score cho khâu phát hiện vị từ (Predicate Detection) và gán nhãn tham số (Argument Labeling).