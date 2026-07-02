# Slide Outline — Đề tài 24
## Kiến trúc bộ nhớ phi khả biến dựa trên ReRAM/PCM và ứng dụng trong Storage-Class Memory (SCM)
### IT2004.CH201 — Công nghệ máy tính hiện đại

---

## SLIDE 1 — Trang bìa

**Tiêu đề:**
KIẾN TRÚC BỘ NHỚ PHI KHẢ BIẾN DỰA TRÊN ReRAM/PCM
VÀ ỨNG DỤNG TRONG STORAGE-CLASS MEMORY (SCM)

**Thông tin:**
- Môn học: Công nghệ máy tính hiện đại — IT2004.CH201
- GVHD: TS. Nguyễn Mạnh Thảo
- Nhóm thực hiện:
  - 250202039 — Bùi Tấn Hưng
  - 250202033 — Nguyễn Tấn Đạt
  - 250202011 — Đinh Hoàng Lộc
  - 250202054 — Dương Hiển Thế
  - 250202014 — Phan Văn Minh
- Logo UIT góc trên trái

---

## SLIDE 2 — Nội dung trình bày

1. Đặt vấn đề: giới hạn của bộ nhớ truyền thống
2. Công nghệ PCM: nguyên lý và thiết kế
3. Công nghệ ReRAM: cơ chế vật lý
4. ReRAM trong tính toán (Processing-In-Memory)
5. Khái niệm Storage-Class Memory (SCM)
6. Các hướng nghiên cứu cải thiện SCM
7. Case study: Intel Optane DC PMM
8. Kết luận

---

## SLIDE 3 — Đặt vấn đề: phân cấp bộ nhớ hiện tại

**Tiêu đề:** Vấn đề của phân cấp bộ nhớ truyền thống

**Hình minh họa:** Sơ đồ tháp memory hierarchy (SRAM > DRAM > Flash/SSD > HDD)
- Trục X: Dung lượng tăng dần
- Trục Y: Tốc độ giảm dần
- Màu sắc phân biệt volatile vs non-volatile

**Nội dung chính:**
- SRAM/DRAM: nhanh, nhưng volatile (mất điện = mất dữ liệu) và đắt
- Flash/SSD: rẻ, dung lượng lớn, nhưng truy cập theo block (không phải byte), chậm hơn DRAM 1.000x
- Khoảng trống lớn giữa DRAM và Flash: chưa có công nghệ nào lấp đầy

**Câu chốt:** "Ứng dụng hiện đại cần bộ nhớ vừa nhanh như DRAM, vừa phi khả biến như Flash — không công nghệ nào hiện tại đáp ứng cả hai."

---

## SLIDE 4 — Đặt vấn đề: giới hạn của Flash

**Tiêu đề:** Tại sao Flash không thể thay thế DRAM?

**So sánh ngắn gọn:**

| Đặc tính | DRAM | Flash NAND |
|---|---|---|
| Giao diện truy cập | Load/Store (byte) | Block I/O (512B+) |
| Latency | ~81 ns | ~100 µs |
| Endurance | Thực tế vô hạn | 1.000 – 100.000 chu kỳ |
| Volatile | Có | Không |

**Kết luận:** Flash không thể làm bộ nhớ chính vì giao diện sai, quá chậm, và hỏng sau một số lần ghi nhất định.

---

## SLIDE 5 — PCM: Nguyên lý hoạt động

**Tiêu đề:** Phase Change Memory — lưu dữ liệu bằng pha vật liệu

**Hình minh họa:** Cấu trúc mushroom cell — heater + lớp GST + 2 điện cực
- Trạng thái AMORPHOUS (điện trở cao = bit 1) màu đỏ/cam
- Trạng thái CRYSTALLINE (điện trở thấp = bit 0) màu xanh

**Nội dung:**
- Vật liệu: GST (Ge₂Sb₂Te₅) — chalcogenide
- **RESET** (ghi 1): xung dòng lớn ngắn → nung chảy → làm nguội nhanh → amorphous
- **SET** (ghi 0): xung dòng vừa dài hơn → ủ nhiệt giữa T_c và T_m → tinh thể
- **Đọc:** dòng nhỏ đo điện trở, không làm nhiễu trạng thái
- Tương phản điện trở: có thể chênh lên đến 5 bậc độ lớn

---

## SLIDE 6 — PCM: Thách thức vật liệu và thiết kế ô nhớ

**Tiêu đề:** Hai thách thức lớn nhất của PCM

**Cột trái — Resistance Drift:**
- Điện trở vùng amorphous tự tăng theo thời gian: R(t) = R₀(t/t₀)^ν, ν ≈ 0.1
- Giới hạn chính của MLC: các mức trung gian trôi lẫn vào nhau

**Cột phải — Mâu thuẫn vật liệu:**
- Vật liệu phải vừa ổn định ở 85-150°C (không tự ghi lại)
- Vừa kết tinh cực nhanh (cấp ns) khi có lệnh ghi
- Hai yêu cầu mâu thuẫn nhau — bài toán tối ưu trung tâm của PCM

**Phần dưới — Dòng RESET:**
- Tỉ lệ thuận với diện tích tiếp xúc; cần 10-40 MA/cm²
- Lịch sử cải tiến: mushroom → edge-contact → μTrench → pore → confined cell (giảm 65%)

---

## SLIDE 7 — PCM: Selector và giới hạn mật độ

**Tiêu đề:** Nút thắt mật độ: bộ chọn ô nhớ

**Sơ đồ:** Ô PCM + selector kết nối với wordline/bitline

**Nội dung:**
- Mật độ mảng PCM bị giới hạn bởi kích thước **selector**, không phải ô PCM
- Các loại selector: MOSFET, BJT, Diode, MIT, OTS
- Yêu cầu lý tưởng: ON/OFF ratio > 10⁶, dòng ON cao, tương thích CMOS < 450°C
- Chưa có selector nào thỏa mãn đồng thời tất cả → vẫn là thách thức mở

**Box highlight:** MLC (Multi-Level Cell) — lưu 2-4 bit/ô, cần thuật toán read-verify-write nhiều vòng lặp; đã thương mại hóa ở 2 bit/ô.

---

## SLIDE 8 — PCM: Độ tin cậy

**Tiêu đề:** Cơ chế hỏng của ô PCM

**3 hình nhỏ minh họa:**

1. **Stuck-open** — lỗ rỗng (void) hình thành tại điện cực dưới sau nhiều chu kỳ ghi → ô bị kẹt cách điện
2. **Stuck-close** — antimon (Sb) tích tụ gần điện cực, giảm nhiệt độ kết tinh cục bộ → ô bị kẹt dẫn điện
3. **Thermal disturbance** — nhiệt RESET lan sang ô lân cận → kết tinh ngoài ý muốn

**Khả năng thu nhỏ:**
- Vật liệu GST giữ tính chất chuyển pha đến ~1.8 nm
- Thương mại hiện tại: 90 nm; trình diễn công nghệ: 45 nm
- Rào cản: selector, không phải vật liệu PCM

---

## SLIDE 9 — ReRAM: Cấu trúc MIM và cơ chế filament

**Tiêu đề:** Resistive RAM — lưu dữ liệu bằng sợi dẫn điện

**Hình minh họa (hai trạng thái):**
- Trái: HRS — lớp oxide nguyên vẹn, không có filament
- Phải: LRS — filament oxygen vacancy xuyên qua oxide từ điện cực dưới lên trên

**Nội dung:**
- Cấu trúc MIM: Kim loại / Oxide / Kim loại (đơn giản hơn PCM)
- Cơ chế: oxygen vacancy di chuyển dưới điện trường → tập trung thành sợi dẫn
- **SET** (HRS → LRS): áp điện áp âm → filament hình thành
- **RESET** (LRS → HRS): áp điện áp dương → filament đứt gãy
- Vật liệu oxide phổ biến: NiO, HfO₂, ZrO₂, TiO₂

---

## SLIDE 10 — ReRAM: Crossbar và vấn đề Sneak-path

**Tiêu đề:** Crossbar array — mật độ cao nhưng có bẫy

**Hình minh họa (2 cột):**

**Cột trái — Crossbar thuần:**
- Lưới hàng x cột, ô ReRAM tại mỗi giao điểm
- Không cần selector → mật độ tối đa (4F²)
- Vấn đề: sneak-path — dòng ký sinh qua các ô không mục tiêu

**Cột phải — 1T1R:**
- Thêm 1 transistor mỗi ô → chặn sneak-path
- Tương tự cấu trúc DRAM (1T1C) nhưng thay tụ bằng điện trở
- Đánh đổi: mật độ giảm, nhưng đọc/ghi chính xác

**Độ tin cậy:** Retention > 10 năm (ngoại suy); Endurance > 10⁶ chu kỳ (TL4)

---

## SLIDE 11 — ReRAM trong tính toán: Dot-product analog

**Tiêu đề:** Memristor Crossbar Array — tính toán trong bộ nhớ

**Hình minh họa:**
- Ma trận conductance G (mỗi ô = 1 giá trị)
- Vector điện áp đầu vào V đặt vào các hàng
- Dòng điện ra tại mỗi cột = dot-product theo định luật Ohm + Kirchhoff

**Công thức:** I_j = Σᵢ Vᵢ × Gᵢⱼ

**Ý nghĩa:**
- Phép nhân ma trận-vector thực hiện song song trong 1 bước
- Không cần di chuyển dữ liệu giữa CPU và RAM → phá vỡ memory wall
- Phù hợp đặc biệt với lớp fully-connected và convolution của neural network

---

## SLIDE 12 — Kiến trúc tăng tốc mạng nơ-ron

**Tiêu đề:** Từ crossbar đến chip AI: ISAAC và PipeLayer

**Sơ đồ pipeline:**

```
Input → [Tile ReRAM] → ADC → Digital → [Tile ReRAM] → ADC → Output
            Layer 1                         Layer 2
```

**ISAAC (In-Situ Analog Arithmetic in Crossbars):**
- Chia mạng thành các tile crossbar, xử lý song song theo pipeline
- Mỗi tile = 1 lớp hoặc 1 phần lớp của mạng nơ-ron

**PipeLayer:**
- Cải thiện ISAAC bằng pipeline giữa các tile
- Xử lý nhiều ảnh/batch đồng thời qua các giai đoạn

**So với GPU:** Tốc độ suy diễn cao hơn, năng lượng thấp hơn trên lý thuyết

---

## SLIDE 13 — Thách thức ADC/DAC và độ tin cậy kiến trúc

**Tiêu đề:** Nút thắt thực tế của ReRAM accelerator

**Hình biểu đồ tỉ lệ diện tích:**
- ADC/DAC chiếm 85-98% diện tích chip trong hệ thống neuromorphic ReRAM
- Phần tính toán thực (crossbar) chỉ chiếm 2-15%

**Các thách thức khác:**
- Sneak-path trong crossbar không selector
- Process variation: điện trở mỗi ô sai lệch so với thiết kế
- Lỗi cứng stuck-at (SA0/SA1): xử lý bằng fault-aware training
- Trôi điện trở (resistance drift): dùng guardband nhưng tốn thêm latency ghi
- Thiếu công cụ mô phỏng mở và prototype quy mô lớn

---

## SLIDE 14 — Khái niệm Storage-Class Memory

**Tiêu đề:** Storage-Class Memory — lấp khoảng trống trong phân cấp bộ nhớ

**Hình minh họa:** Sơ đồ phân cấp mở rộng
```
SRAM (cache) → DRAM → [SCM] → Flash/SSD → HDD
     nhanh, đắt          ↑              chậm, rẻ
                   byte-addressable
                   non-volatile
                   vị trí mới
```

**Định nghĩa SCM:**
- Bộ nhớ phi khả biến, truy cập theo byte (không phải block)
- Đặt giữa DRAM và Flash trong phân cấp
- Ứng viên công nghệ: PCM và ReRAM

**Lợi thế chung:**
- Không cần refresh (không rò rỉ điện tích như DRAM)
- Mật độ cao hơn SRAM nhiều lần
- Hỗ trợ MLC

---

## SLIDE 15 — So sánh định lượng SCM với các tầng bộ nhớ khác

**Tiêu đề:** Vị trí của SCM trong phổ bộ nhớ

**Bảng (từ TL5, Table 1):**

| Công nghệ | Cell size (F²) | Read latency | Write latency | Endurance | Standby power |
|---|---|---|---|---|---|
| HDD | N/A | ms | ms | Thực tế vô hạn | Cao |
| Flash NAND | 4 | ~100 µs | ~1 ms | 10³-10⁵ | 0 |
| DRAM | 6 | ~10 ns | ~10 ns | Thực tế vô hạn | Cao (refresh) |
| PCM | 8 | 20-50 ns | 50 ns | 10¹² | 0 |
| ReRAM | 4-10 | 10 ns | 50 ns | 10¹¹ | 0 |

**Ghi chú:** Latency Optane thực tế (TL3): ~305 ns random read — cao hơn lý thuyết PCM, phản ánh overhead hệ thống.

---

## SLIDE 16 — Các hướng nghiên cứu cải thiện SCM

**Tiêu đề:** Khắc phục nhược điểm SCM — các hướng nghiên cứu chính

**4 hướng chính:**

**1. Wear-leveling (kéo dài tuổi thọ)**
- ECP (Error Correcting Pointers): ghi dữ liệu bù xen kẽ với dữ liệu thực
- EPAC: phân phối đều số lần ghi trên toàn mảng, tránh một vùng bị ghi quá nhiều

**2. Error Correction (ECC)**
- NAND Flash dùng ECC đơn bit; SCM cần ECC đa bit vì lỗi phức tạp hơn
- Multi-bit error correction cho MLC

**3. Hiệu năng ghi — giảm độ trễ**
- PreSET/Two-Stage-Write: chuẩn bị ô trước khi ghi để rút ngắn thời gian SET
- MaxPB: song song hóa tối đa các ô trong cùng page

**4. MLC — Program-and-Verify**
- Lặp ghi-đọc-xác minh đến khi đạt mức điện trở đúng
- Guardband: đệm chống drift, đánh đổi trực tiếp với tốc độ ghi

---

## SLIDE 17 — Case study: Intel Optane DC PMM

**Tiêu đề:** Intel Optane DC Persistent Memory — SCM thương mại đầu tiên

**Thông tin sản phẩm:**
- Công nghệ: 3D XPoint (họ hàng PCM, cấu trúc cross-point)
- Giao diện: DDR-T (tương thích khe DIMM nhưng giao thức khác DRAM)
- Dung lượng: 128 GB / 256 GB / 512 GB per DIMM

**Hai chế độ hoạt động:**

| Chế độ | Mô tả | Phù hợp |
|---|---|---|
| Memory Mode | DRAM làm cache cho Optane, OS thấy 1 vùng bộ nhớ lớn | Tăng dung lượng RAM |
| App Direct Mode | Ứng dụng truy cập trực tiếp qua PMDK | Persistent data structures |

**Điểm đặc biệt:** Granularity nội bộ 256B, khác với cache line 64B của CPU → cần thiết kế hệ thống cẩn thận.

---

## SLIDE 18 — Kết quả đo Optane (TL3)

**Tiêu đề:** Hiệu năng thực tế vs kỳ vọng

**Số liệu đo thực (Izraelevitz et al. 2019 — ~330 giờ đo):**

| Chỉ số | DRAM | Optane (App Direct) |
|---|---|---|
| Random read latency | ~81 ns | ~305 ns |
| Sequential read bandwidth | Cao | Thấp hơn DRAM ~2-3x |
| Random write latency | ~81 ns | ~94 ns |
| Asymmetry đọc/ghi | Không | Có (ghi nhanh hơn đọc) |

**Phát hiện quan trọng:**
- Giả lập NVM bằng DRAM **không tái hiện được** hành vi thực của Optane
- Asymmetry đọc/ghi ngược với Flash (Flash ghi chậm, đọc nhanh)
- Granularity nội bộ 256B ảnh hưởng rõ đến hiệu năng truy cập nhỏ

---

## SLIDE 19 — Bảng so sánh tổng hợp

**Tiêu đề:** Tổng kết: PCM, ReRAM, DRAM, Flash và Optane

*(Dùng bảng Section 8 của báo cáo)*

**Bảng:**

| Đặc tính | DRAM | PCM | ReRAM | Optane/3D XPoint | NAND Flash |
|---|---|---|---|---|---|
| Volatile | Có | Không | Không | Không | Không |
| Byte-addressable | Có | Có | Có | Có | Không |
| Cell size (F²) | 6 | 8 | 4-10 | ~4 (3D) | 4 |
| Cần selector | 1T1C | Bắt buộc | Tùy chọn | Có | Có |
| Endurance | Thực tế vô hạn | 10¹² | 10¹¹ | >10³ (TL3 đo) | 10³-10⁵ |

**Highlight:** Không công nghệ nào hoàn hảo — SCM là sự đánh đổi có kiểm soát.

---

## SLIDE 20 — Kết luận

**Tiêu đề:** Kết luận và xu hướng tương lai

**3 điểm kết luận:**

1. **PCM và ReRAM** đều khả thi làm SCM, mỗi công nghệ có ưu thế riêng: PCM chín muồi hơn về chế tạo, ReRAM đơn giản hơn về cấu trúc và tiềm năng mật độ cao hơn.

2. **Intel Optane** cho thấy SCM thương mại là thực tế, nhưng latency thực tế (~305 ns) cao hơn đáng kể so với lý thuyết, đặt ra yêu cầu thiết kế hệ thống phần mềm cẩn thận.

3. **Tương lai:** Hệ bộ nhớ lai (hybrid memory) kết hợp DRAM + SCM, MLC mật độ cao, và tính toán trong bộ nhớ (ReRAM PIM) là 3 hướng phát triển đang được đầu tư mạnh nhất.

**Câu kết:** NVM đang chuyển phân cấp bộ nhớ từ các tầng rời rạc thành một phổ liên tục — người thiết kế hệ thống có thêm lựa chọn nhưng cũng thêm phức tạp.

---

## SLIDE 21 — Tài liệu tham khảo

**Tiêu đề:** Tài liệu tham khảo

1. H.-S. P. Wong et al., "Phase Change Memory," *Proc. IEEE*, vol. 98, no. 12, pp. 2201-2227, 2010.

2. S. Mittal, "A Survey of ReRAM-Based Architectures for Processing-In-Memory and Neural Networks," *Machine Learning and Knowledge Extraction*, vol. 1, no. 1, pp. 75-114, 2018.

3. J. Izraelevitz et al., "Basic Performance Measurements of the Intel Optane DC Persistent Memory Module," *arXiv:1903.05714*, 2019.

4. J. S. Meena et al., "Overview of Emerging Nonvolatile Memory Technologies," *Nanoscale Research Letters*, vol. 9, no. 526, 2014.

5. H. Kamath et al., "A Survey of Storage Class Memory," *IETE Technical Review*, 2019.

---

## GHI CHÚ THIẾT KẾ

### Phân công slide theo thành viên (gợi ý):
- Slides 3-4: Đặt vấn đề
- Slides 5-8: PCM (phần 2)
- Slides 9-10: ReRAM vật lý (phần 3)
- Slides 11-13: ReRAM kiến trúc (phần 4)
- Slides 14-16: SCM (phần 5-6)
- Slides 17-18: Optane (phần 7)
- Slides 19-21: Tổng hợp và kết luận

### Gợi ý thiết kế visual:
- Dùng màu thống nhất: xanh dương cho DRAM, cam cho PCM, xanh lá cho ReRAM, tím cho Optane
- Các bảng số liệu: highlight ô tốt nhất bằng màu nền nhạt
- Sơ đồ cấu trúc ô nhớ (Slide 5, 9): vẽ đơn giản bằng hình chữ nhật + mũi tên, không cần chi tiết kỹ thuật
- Font chữ: tối thiểu 24pt cho nội dung, 32pt+ cho tiêu đề
- Mỗi slide không quá 5-6 dòng bullet; ưu tiên hình/bảng thay vì text

### Thời lượng gợi ý (20-25 phút):
- Slides 1-2: 1 phút
- Slides 3-4: 2 phút (đặt vấn đề)
- Slides 5-8: 5 phút (PCM)
- Slides 9-10: 3 phút (ReRAM vật lý)
- Slides 11-13: 3 phút (ReRAM kiến trúc)
- Slides 14-16: 3 phút (SCM)
- Slides 17-18: 3 phút (Optane)
- Slides 19-21: 2 phút (tổng hợp + Q&A dẫn vào)
