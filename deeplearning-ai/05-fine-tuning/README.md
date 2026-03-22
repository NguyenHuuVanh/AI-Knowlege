# 🔧 Fine-tuning & Training

## Tổng quan
Kỹ thuật tinh chỉnh và huấn luyện mô hình AI cho các use case cụ thể.

## Các phương pháp Fine-tuning

### 1. Full Fine-tuning
- Train toàn bộ parameters
- Tốn nhiều tài nguyên (GPU, RAM)
- Kết quả tốt nhất nhưng chi phí cao

### 2. LoRA (Low-Rank Adaptation)
- Chỉ train một phần nhỏ parameters
- Giảm đáng kể tài nguyên cần thiết
- Kết quả gần bằng full fine-tuning

### 3. QLoRA
- LoRA + Quantization (4-bit, 8-bit)
- Có thể fine-tune model lớn trên consumer GPU
- Phổ biến nhất hiện tại

### 4. RLHF / DPO
- **RLHF** - Reinforcement Learning from Human Feedback
- **DPO** - Direct Preference Optimization
- Align model theo preference con người

## Tools & Platforms
| Tool | Mô tả |
|------|--------|
| Hugging Face Transformers | Thư viện chuẩn cho NLP |
| Unsloth | Fine-tuning nhanh, ít RAM |
| Axolotl | Config-based fine-tuning |
| OpenAI Fine-tuning API | Cloud fine-tuning |
| Google Vertex AI | Enterprise fine-tuning |

## 📎 Tài liệu
- [Hugging Face - Fine-tuning](https://huggingface.co/docs/transformers/training)
- [Unsloth](https://github.com/unslothai/unsloth)
- [LoRA Paper](https://arxiv.org/abs/2106.09685)

---
*Thêm ghi chú vào thư mục `notes/` bên dưới.*
