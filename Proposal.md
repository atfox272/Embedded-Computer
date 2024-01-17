# Embedded-Computer
Implement Embedded Computer

# Tổng quan:
Thiết kế một máy tính nhúng với bộ xử lý RV64IM tích hợp bộ gia tốc phần cứng phục vụ mục đích đồ họa 

## Sơ đồ khối:

![EmbeddedComputer drawio](https://github.com/atfox272/Embedded-Computer/assets/99324602/6d1d9a0b-0629-4479-867e-9cb7987013e0)
_Lưu ý: Sơ đồ khối trên có thể chưa hoàn chỉnh vì có một số khối chức năng mà tôi chưa hoàn toàn nắm được luồng thực thi và các cơ chế của nó_

## Tổng quan các khối:
### RV64IM:
_(Chi tiết sẽ được nói rõ trong phần sau)_

### Memory Management Unit (MMU):
Memory Management Unit (MMU) là khối quản lý bằng phần trang (page table) cho Data Memory. Tối ưu vùng nhớ (tránh vấn đề segmentation trong bộ nhớ) và bảo vệ 1 số vùng nhớ bị hạn chế truy cập.
Tuy nhiên, trong dự án này để demo hoặc prototype vì chưa cần quá chú trọng vào việc **tối ưu vùng nhớ** hoặc **bảo mật** thì có thể bỏ qua khối này (nối INTERCONNECT trực tiếp vào khối DMA)

### Direct Access Memory (DMA):
Direct Access Memory (DMA) là khối truy xuất bộ nhớ (gồm Data Memory và ROM chứa Bootloader), các khối CPU hay PPU khi muốn truy xuất vùng nhớ đều phải yêu cầu trung gian qua DMA

### Interrupt:
Interrupt là khối quản lý _flag_ cuar các module ngắt (timer, peripheral, v.v)

### Peripherlas & I/O Devices
Gồm các khối ngoại vi và I/O Devices

### ROM:
Chứa Bootloader (resident monitor) (về resident)

### Pixel Processing Unit (PPU):
_Simple GPU_
### Video Interface (HDMI Interface)

## RV64IM processor:
### Sơ đồ khối
![image](https://github.com/atfox272/Embedded-Computer/assets/99324602/a888d6f7-6b0a-4b24-b0e2-04b62a66762d)

### Chi tiết các khối
#### Central Processing Unit
RV64IM - bộ xử lý các phép toán học (1 chu kỳ và đa chu kỳ), các phép toán số thực và các phép luận lý

#### Prefetch & Branch Predictor:
Hai bộ này sẽ quản lý luồn của thanh ghi PC (program counter)
- Prefect: Cập nhật PC theo địa chỉ interrupt routine khi có cờ ngắt (thông tin sẽ được truy suất từ Interrupt Controller)
- Branch predictor: lưu thông tin để đưa ra quyết định nhảy/không nhảy

#### Cache
Bộ nhớ đệm lệnh và bộ nhớ đêm dữ liệu (2-way associative cache)

#### Debug Interface

### Mô hình Pipeline:
7-stage pipeline
(_Pre-fetch + Instruction-fetch + Instruction-decode + Issue + Execution + MEM + WB_)

![image](https://github.com/atfox272/Embedded-Computer/assets/99324602/7a745be8-dc26-4b16-bbfd-d8b5eff5f249)

