---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background:
# apply any windi css classes to the current slide
class: 'text-center'
position: 'center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: true
---

# Báo cáo bài tập cuối kỳ
## Nhóm 6
## Đề tài: Ứng dụng Deep Learning xây dựng phần mềm tự động sinh mô tả ảnh
Thành viên: - Nguyễn Chí Thanh
            - Đoàn Đại Thanh Long
            - Lê Thị Thắm

<style>
h1 {
  background-color: #2B90B6;
  font-size: 12px;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
  -font-size: 20px;
}
</style>

# MỤC LỤC

## 1. Tổng quan Image Captioning
## 2. Chi tiết phương pháp
## 3. Phương pháp đánh giá
## 4. Thực nghiệm và kết quả
## 5. Demo giao diện
## 6. Hướng phát triển

---

# 1. Tổng quan Image Captioning

- Định nghĩa: Image Captioning là một bài toán miêu tả nội dung của một ảnh dựa vào ngôn ngữ tự nhiên.

- Ý nghĩa bài toán: 
  - Để giúp những người già mắt kém hoặc người mù có thể biết được cảnh vật xung quanh hay hỗ trợ việc di chuyển
  - Giúp google search có thể tìm kiếm được hình ảnh dựa vào caption

- Image Captioning là một trong những vision-language model phổ biến cùng với:
  - Visual Question Answering
  - Text-Image Retrieval
  - Visual Grounding
  - Scene-Graph Generation

---

# 1. Tổng quan Image Captioning


---

#  3. Phương pháp đánh giá

- Phương pháp hay được sử dụng để đánh giá các bản dịch là BLEU Score.

- BLEU (BiLingual Evaluation Understudy) là một số nằm giữa 0 và 1 đo lường độ tương tự giữa các bản dịch máy với một tập các bản dịch mẫu có chất lượng cao.

- BLEU score cũng có thể được sử dụng cho các bài toán sinh văn bản:
  - Language Generation
  - Image Captioning
  - Text Summarization
  - Speech To Text

- BLEU score được tính dựa trên số lượng n-grams giống nhau giữa câu được do model sinh ra và các câu dịch mẫu (có xét đến yếu tố độ dài)

---

# 3. Phương pháp đánh giá

## 3.1 N-grams

- N-grams là một khái niệm được sử dụng rất nhiều trong xử lý ngôn ngữ tự nhiên. Là một tập gồm n các từ liên tiếp nhau trong một câu

- Ví dụ, trong câu "Học cao học rất khó", các n-grams có thể có:

  - 1-gram (unigram): "Học", "cao", "học", "rất", "khó"
  - 2-gram (bigram): "Học cao", "cao học", "học rất", "rất khó"
  - 3-gram (trigram): "Học cao học", "cao học rất", "học rất khó"
  - 4-gram: "Học cao học rất", "cao học rất khó"


---

# 3. Phương pháp đánh giá

## 3.2 Precision

- Độ đo đo lường số lượng từ xuất hiện trong câu được mô hình dịch cũng xuất hiện 

- Ví dụ:
  - Câu mẫu: "Tôi đang học cao học"
  - Câu dự đoán: "Tôi đang học thạc sỹ"

Precision = Số từ đúng trong câu dự đoán/Tổng số từ trong câu dự đoán

Precision = 3/5

---

# 3. Phương pháp đánh giá

## 3.3 Clipped Precision

- Cách tính Precision ở trên có thể bị gian lận bằng cách lặp một từ đúng nhiều lần để tăng Precision

- Ví dụ:
  - Câu mẫu: "Tôi đang học"
  - Câu dự đoán: "Tôi tôi tôi"

$\Rightarrow$ Precision = 1

Cách tính trên không hợp lý, để khắc phục nhược điểm này. Mỗi từ đúng ở câu dự đoán chỉ được tính 1 lần dù có xuất hiện bao nhiêu lần trong câu dự đoán.

Clipped Precision = 1/3

---

# 3. Phương pháp đánh giá

## 3.4 Độ chính xác trung bình hình học (GMP):

- Sử dụng các Precision ứng với các n-grams, ta tính độ chính xác trung bình hình học:

$$\mathrm{GAP}(N)=\exp\big( \sum_{n=1}^N w_n \log p_n \big)=\prod_{i=1}^N p_n^{w_n}$$

Với:

- N là n-grams cao nhất được xét (thường $N=4$)
- $w_n$ là trọng số với Precision của n-grams
- $p_n$ là độ chính xác ứng với n-grams

---

# 3. Phương pháp đánh giá

## 3.5. Brevity Penalty

- Khi một câu chỉ có một từ chẳng hạn "Em" hoặc "Học", 1-gram precision là 1, nhưng sự thật đây không phải là một câu dịch tốt.

- Vì vậy ở BLEU Score cần thêm một hệ số phạt về độ dài của câu dịch.

$$\mathrm{Brevity \thickspace Penalty}=\mathrm{BP}=\begin{cases} 1 \thickspace \mathrm{nếu} \thickspace c > r \\ \exp\Big(1-\dfrac{r}{c}\Big) \thickspace \mathrm{nếu} \thickspace c \leq r \end{cases}$$

- Với:

 - $c$ là độ dài của câu được dự đoán bởi mô hình
 - $r$ là độ dài của câu mẫu

---
layout: two-cols
---

# 3. Phương pháp đánh giá

## 3.6. Công thức BLEU Score

$$\mathrm{BLEU}(N)=\mathrm{BP}.\mathrm{GAP}(N)$$

::right::

### Ví dụ: Tính BLEU Score giữa hai câu:
  - Câu mẫu: "The guard arrived late because it was raining"
  - Câu dự đoán: "The guard arrived late because of the rain"

- Bước 1: Tính Precision cho 1-grams đến 4-grams

  - Precision 1-grams:

<img src="images/BLEU_1.jpg" width="500" >

Precision 1-grams = 5/8

---
layout: two-cols
---

# 3. Phương pháp đánh giá

## 3.6. Công thức BLEU Score

- Bước 1: Tính Precision cho 1-grams đến 4-grams

  - Precision 2-grams:

<img src="images/BLEU_2.jpg" width="500" >

Precision 2-grams = 4/7

::right::

  - Precision 3-grams:

<img src="images/BLEU_3.jpg" width="500" >

Precision 3-grams = 3/6

  - Precision 4-grams:

  <img src="images/BLEU_4.jpg" width="500" >

Precision 4-grams = 2/5

---

# 3. Phương pháp đánh giá

## 3.6. Công thức BLEU Score

- Bước 2: Tính Brevity Penalty:

$$\mathrm{BP}=\exp\Big(1-\dfrac{r}{c}\Big)=\exp\Big(1-\dfrac{8}{8}\Big)=1$$

- Bước 3: Tính độ chính xác trung bình hình học $\mathrm{GAP}(N)$:

$$\mathrm{GAP}(N)=\exp\big( \sum_{n=1}^N w_n \log p_n \big)=\prod_{i=1}^N p_n^{w_n}=\Big(\dfrac{5}{8}\dfrac{4}{7}\dfrac{3}{6}\dfrac{2}{5}\Big)^{\dfrac{1}{4}}\approx 0.516973$$

- Bước 4: Tính BLEU score:

$$\mathrm{BLEU}(N)=\mathrm{BP}.\mathrm{GAP}(N)\approx 0.516973$$

---

# 4. Thực nghiệm và kết quả

# 4.1. Dữ liệu

- Tập dữ liệu được sử dụng cho quá trình huấn luyện là Flikr30k

- Flickr30k bao gồm khoảng 31000 ảnh thu thập từ Flickr, mỗi ảnh gồm 5 câu miêu tả ảnh được gán nhãn từ người

- Flickr30k có một tập dữ liệu thu nhỏ là Flickr8k

- Ngoài ra còn một tập dữ liệu phổ biến khác là COCO

---
layout: two-cols
---

# 4. Thực nghiệm và kết quả

# 4.1. Dữ liệu

- Một số hình ảnh mẫu và các câu miêu tả mẫu tương ứng:

<img src="images/Flickr_Sample_1.jpg" width="500" >
<img src="images/Flickr_Sample_2.jpg" width="500" >
<img src="images/Flickr_Sample_3.jpg" width="500" >

::right::

<img src="images/Flickr_Sample_4.jpg" width="500" >
<img src="images/Flickr_Sample_5.jpg" width="500" >

---

# 4. Thực nghiệm và kết quả

# 4.1. Dữ liệu

- Tần số các từ chưa qua xử lý:

<img src="images/Flickr_frequency_raw_word.jpg" width="600" >


---
layout: two-cols
---

# 4. Thực nghiệm và kết quả

# 4.1. Dữ liệu

- Histogram độ dài của các miêu tả mẫu:

<img src="images/Flickr_Length_Distribution.jpg" width="300">

::right::

- Độ dài của các miêu tả ảnh có phân phối lệch phải

- Đa số các câu có độ dài từ 10 đến 20 từ

- Câu có độ dài lớn nhất khoảng 80

