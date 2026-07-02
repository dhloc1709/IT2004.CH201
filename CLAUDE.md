# IT2004.CH201 — Đề tài 24

## Thông tin chung

- **Môn học:** IT2004.CH201 — Công nghệ máy tính hiện đại
- **GVHD:** TS. Nguyễn Mạnh Thảo
- **Hình thức:** Báo cáo seminar lý thuyết (không có demo)
- **Nhóm:**
  - 250202039 — Bùi Tấn Hưng
  - 250202033 — Nguyễn Tấn Đạt
  - 250202011 — Đinh Hoàng Lộc
  - 250202054 — Dương Hiển Thế
  - 250202014 — Phan Văn Minh

## Đề tài

**Kiến trúc bộ nhớ phi khả biến dựa trên ReRAM/PCM và ứng dụng trong Storage-Class Memory (SCM)**

Phạm vi đề tài gồm 3 phần chính:
1. Kiến trúc vật lý của PCM và ReRAM
2. Ứng dụng ReRAM trong tính toán (Processing-In-Memory, neural network accelerator)
3. Ứng dụng PCM/ReRAM làm Storage-Class Memory (SCM)

**Không thuộc phạm vi:** STT-RAM, MRAM, FeRAM — các công nghệ này không có trong tên đề tài và đã được loại bỏ hoàn toàn khỏi báo cáo.

## File chính

| File | Mô tả |
|------|-------|
| `topic_24_baocao.tex` | Báo cáo LaTeX chính — nguồn sự thật duy nhất |
| `topic_24_baocao.pdf` | PDF được compile từ file trên (tectonic) |
| `topic_24_slide_outline.md` | Outline 21 slide cho phần trình bày |
| `logo-uit.png` | Logo UIT dùng trong trang bìa báo cáo |
| `ref/` | 5 tài liệu tham khảo gốc (PDF) |

## Cấu trúc báo cáo

```
1. Đặt vấn đề
2. Công nghệ PCM (2.1–2.5)
3. Công nghệ ReRAM: cơ chế vật lý (3.1–3.2)
4. Kiến trúc ứng dụng ReRAM trong tính toán (4.1–4.5 + Thách thức tổng hợp)
5. Khái niệm Storage-Class Memory (5.1–5.2)
6. Các hướng nghiên cứu cải thiện SCM (6.1–6.4)
7. Case study: Intel Optane DC PMM (7.1–7.3)
8. So sánh tổng hợp
9. Kết luận và xu hướng tương lai
```

## Tài liệu tham khảo

| Mã | File | Nội dung |
|----|------|----------|
| TL1 | `ref/topic_24_ref_01_PhaseChangeMemory.pdf` | Wong et al. 2010 — Phase Change Memory (PCM) |
| TL2 | `ref/topic_24_ref_02_make-01-00005.pdf` | Mittal 2018 — Survey ReRAM architectures (PIM, neural networks) |
| TL3 | `ref/topic_24_ref_03_1903.05714v3.pdf` | Izraelevitz et al. 2019 — Intel Optane DC PMM benchmark |
| TL4 | `ref/topic_24_ref_04_OverviewEmergingNVM.pdf` | Meena et al. 2014 — Overview emerging NVM technologies |
| TL5 | `ref/topic_24_ref_05_SurveySCM.pdf` | Kamath et al. 2019 — Survey of Storage Class Memory |

## Compile báo cáo

```bash
/opt/homebrew/bin/tectonic topic_24_baocao.tex
```

Chỉ có warning `Underfull \hbox` (bình thường), không có lỗi.

## Lưu ý quan trọng khi chỉnh sửa

- **STT-RAM đã bị xóa hoàn toàn** — không thêm lại vào bất kỳ đâu
- **Slide outline khớp với báo cáo** — sửa một bên thì sửa bên kia theo
- Deadline seminar: khoảng đầu tháng 7/2026
