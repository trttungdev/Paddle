# Hướng dẫn sử dụng PaddleOCR

## 0. Cài đặt môi trường
- Load Docker :
```
docker pull trttung1610/bkai_paddle:latest
  
```
```
cd Paddle
pip install -r requirements.txt

```

## 1. Chuẩn bị data
Chuẩn bị data như sau vào thư mục Paddle: 

train.txt, val.txt có dạng sau: `img_path <tab> text_label`
```
img/im3671.jpg_box0.jpg	Trà
img/im3671.jpg_box1.jpg	m
img/im3671.jpg_box2.jpg	ĐC:
img/im3671.jpg_box3.jpg	K
```
Tại đây , đội sẽ chia bộ new_train được BTC cung cấp với tỉ lệ là train : 0.95 , val : 0.05

## 2. Train model
Đội đã điều chỉnh file config để phù hợp và tối ưu với cuộc thi .

### 2.1 Train 
- ABInet : 
```
sh scripts/train_ABInet.sh

```
- SVTR :
```
sh scripts/train_SVTR.sh
```

### 2.2 Export model 

Weight được đội cung cấp đã được chọn là ckpt để dự đoán : 
- ABInet : epoch thứ 10
- SVTR : epoch thứ 82  

Vì vậy ckpt của của 2 mô hình trên sẽ được đội chọn để xuất ra  phục vụ việc dự đoán :

  - ABInet :
  ```
  sh scripts/export_ABInet.sh
  ```
  - SVTR :
  ```
  sh scripts/export_SVTR.sh
  ```

Kết quả được xuất ra tại thư mục {tên mô hình} / Inference

## 3. Predict model
### 3.1. Predict with CLI
- ABInet :
```
sh scripts/pred_ABInet.sh

```
- SVTR :
```
sh scripts/pred_SVTR.sh

```

### 3.2. Format 
Sau khi có file 2 prediction_log.txt từ 2 mô hình , file dự đoán sẽ được đưa qua mytools/paddle2txt.py để lấy kết quả dự đoán và sau đó file prediction_log.txt của mỗi mô hình sẽ được đưa qua format_pred.py để kiểm tra và chỉnh sửa lại phù hợp với file submit:

```
python mytools/paddle2txt.py --input prediction_log.txt --ouput prediction.txt

python mytools/format_pred.py prediction.txt predictions/{tên mô hình}/prediction.txt

```
