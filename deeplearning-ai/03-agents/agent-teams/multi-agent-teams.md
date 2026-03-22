# 👥 Multi-Agent Teams

## Tổng quan
Kiến thức về hệ thống nhiều AI agents phối hợp làm việc cùng nhau để giải quyết các bài toán phức tạp.

## Tại sao cần Agent Teams?
- Một agent đơn lẻ có giới hạn về khả năng
- Chia nhỏ vấn đề cho các agent chuyên biệt → hiệu quả hơn
- Mô phỏng cách con người làm việc nhóm

## Các mô hình Agent Team

### 1. Sequential (Tuần tự)
```
Agent A → Agent B → Agent C → Output
```
- Mỗi agent xử lý một bước, truyền kết quả cho agent tiếp theo
- Ví dụ: Researcher → Writer → Editor

### 2. Hierarchical (Phân cấp)
```
        Manager Agent
       /      |       \
  Agent A  Agent B  Agent C
```
- Một agent "manager" phân phối và điều phối công việc
- Ví dụ: Project Manager → Developer, Designer, Tester

### 3. Collaborative (Cộng tác)
```
  Agent A ←→ Agent B
     ↕           ↕
  Agent C ←→ Agent D
```
- Các agent giao tiếp tự do với nhau
- Phù hợp cho brainstorming, debate

### 4. Supervisor Pattern
- Một supervisor agent giám sát và đánh giá output
- Có thể yêu cầu agent làm lại nếu chưa đạt

## Frameworks hỗ trợ Agent Teams

| Framework | Đặc điểm | Ngôn ngữ |
|-----------|----------|----------|
| **CrewAI** | Dễ dùng, role-based agents | Python |
| **AutoGen** (Microsoft) | Multi-agent conversation | Python |
| **LangGraph** | Graph-based workflows | Python |
| **Swarm** (OpenAI) | Lightweight, handoff-based | Python |
| **Agency Swarm** | Production-ready, tool-based | Python |

## Ví dụ Use Cases
1. **Content Creation Team**: Researcher + Writer + Editor + SEO Specialist
2. **Software Development**: Architect + Developer + QA + DevOps
3. **Customer Support**: Router + Specialist Agents + Escalation Agent
4. **Data Analysis**: Collector + Analyst + Visualizer + Reporter

## 📎 Tài liệu tham khảo
- [CrewAI Docs](https://docs.crewai.com/)
- [AutoGen Docs](https://microsoft.github.io/autogen/)
- [LangGraph Docs](https://langchain-ai.github.io/langgraph/)

---
*Thêm ghi chú vào thư mục `notes/` bên dưới.*
