# Agent Configuration Guide / Hướng Dẫn Cấu Hình Agent

(English below)

<details open>
<summary><b>Hướng Dẫn Sử Dụng Nhóm Lệnh (Workflows & Scripts)</b></summary>

Hệ thống được trang bị các luồng quy trình (workflows) và các công cụ giúp tự động xác thực:

- **Workflows (Slash Commands):** Sử dụng các lệnh này trực tiếp trong chat:
  - `/brainstorm` - Nghiên cứu định hướng giải pháp đa góc độ trước khi code.
  - `/plan` - Phân tích hạng mục công việc và lên kế hoạch chi tiết.
  - `/create` - Thực thi và lập trình file cụ thể.

- **Scripts Chạy Xác Thực (Validation Scripts):**
  - `python .agent/scripts/checklist.py .` - Kiểm tra và xác thực lỗi nhanh trong quá trình dev.
  - `python .agent/scripts/verify_all.py . --url <URL>` - Chạy toàn bộ các quá trình kiểm tra (bao gồm e2e, security, SEO, hiệu năng...) trước khi triển khai (deploy).

</details>

<details>
<summary><b>Hướng Dẫn Triển Khai (Dự án mới & Dự án cũ)</b></summary>

**A. Triển khai vào dự án mới (New Project)**
1. Sao chép các thư mục cấu hình (`.agent/`, `.claude/`, `.continue/`, `.qwen/`) từ kho lưu trữ (repository) này sang thư mục gốc của dự án mới.
2. Sao chép tệp `GEMINI.md` vào thư mục gốc để làm file định hướng gốc của hệ thống.
3. Kích hoạt dự án bằng cách thiết lập các biến môi trường cần thiết (VD: cài đặt `DASHSCOPE_API_KEY` cho Qwen).
4. Mở IDE của dự án và cài bộ code extensions tương ứng (như Continue) để bắt đầu sử dụng chuẩn chung.

**B. Triển khai vào dự án đã có (Existing Project)**
1. Tải phần core agent (`.agent/` và `GEMINI.md`) copy vào thư mục hiện tại để có ngay nhóm luật chuẩn (clean code, rule logic) và các bước scripts validation.
2. Hợp nhất cấu hình nếu có trùng lặp. Ví dụ, nếu bạn đang dùng Continue cấu hình cục bộ, hãy merge tập tin `.continue/.continuerc.json` hiện tại thay vì ghi đè hoàn toàn.
3. Cập nhật và tinh chỉnh `.gitignore` cho hợp lý: Đảm bảo ignore các thư mục bộ nhớ rác như `*/memory/` để không đẩy rác lên upstream.

</details>

<details>
<summary><b>Cấu Hình Chi Tiết Từng Agent (Chi tiết lưu trữ)</b></summary>

Dự án này sử dụng nhiều công cụ AI agent khác nhau bao gồm **Antigravity**, **Claude Code**, **Continue**, và **Qwen**. Dưới đây là cách cấu hình và vị trí thư mục của từng công cụ trong source base này.

#### 1. Antigravity Agent
Antigravity sử dụng thư mục `.agent` (chứa các rules/skills) và dựa trên file cấu hình lõi `GEMINI.md`.
- **Tập tin cấu hình chính**: `GEMINI.md` chứa quy định hoạt động của AI (Agent Routing, Rules) trong workspace.
- **Rules/Skills**: Được lưu tại `.agent/skills/` và tự động load theo quy trình.
- **Workflow / Commands**: Hỗ trợ quy trình tuần tự chuẩn qua các slash commands `/brainstorm`, `/plan`, `/create`.
- **Memory**: Trạng thái bộ nhớ và quá trình phân tích đều được ghi lại vào thư mục memory của Antigravity để duy trì context qua các phiên làm việc (nằm trong `.antigravity/memory/`).

#### 2. Claude Code
Claude Code sử dụng thư mục `.claude/` để quản lý logic và thiết lập phân quyền trực tiếp qua CLI.
- **Cấu hình chính**: Thiết lập quyền đọc/ghi định nghĩa ở `.claude/settings.json`.
- **Kiến trúc hệ thống**: Chi tiết file `.claude/ARCHITECTURE.md`.
- **MCP Server**: File `.claude/.mcp.json` giúp cấu hình máy chủ MCP dành riêng cho Claude tương tác với tài nguyên.
- **Tuỳ chỉnh Rules/Agents**: Đặt trong file markdown tại `.claude/rules/` và `.claude/agents/`.

#### 3. Continue (VS Code / JetBrains)
Công cụ Continue lưu các rule hỗ trợ IDE vào thư mục `.continue/` để load bối cảnh khi xử lý nhắc lệnh.
- **Cấu hình trỏ file**: Bằng file `.continue/.continuerc.json` quản lý rule và path.
- **Cơ sở quy tắc (Rules)**: Tải tự động các file markdown trong `.continue/rules/*.md`.
- **Prompts & Agents**: Tùy chỉnh cá nhân hóa lệnh prompts tại `.continue/prompts/*.md` và `.continue/agents/*.md`.

#### 4. Qwen
Agent Qwen quản lý theo thư mục `.qwen/`, sử dụng cổng API tương thích của DashScope.
- **Cấu hình chính**: `.qwen/settings.json` định nghĩa model, fallback, và quyền truy cập môi trường.
- **Mô hình**: Thiết lập dùng mặc định là `qwen-coder-plus-latest` (dự phòng là `qwen-plus-latest`).
- **Phím API / Môi trường**: Cần đảm bảo biến môi trường `$DASHSCOPE_API_KEY` được config bên trong hệ thống.
- **Workflow / Skills**: Đường dẫn context mặc định của Qwen bao gồm `.qwen/skills`, `.qwen/agents`, `.qwen/commands`.

</details>

---

## English Guide

<details open>
<summary><b>Usage Guide (Workflows & Scripts)</b></summary>

There are specific workflows and validation scripts you can execute:

- **Workflows (Slash Commands):** Utilize these right within the chat context:
  - `/brainstorm` - Multi-perspective structured brainstorming with expert lenses before any implementation.
  - `/plan` - Task breakdown and actionable implementation planning.
  - `/create` - Execute the actual coding feature implementations.

- **Validation Scripts (Terminal executions):**
  - `python .agent/scripts/checklist.py .` - Quick priority-based validation during early development.
  - `python .agent/scripts/verify_all.py . --url <URL>` - Full comprehensive suite verification, including UI/UX, accessibility, security, mapping to an endpoint before deployment.

</details>

<details>
<summary><b>Implementation Guide (New & Existing Projects)</b></summary>

**A. Integrating into a New Project**
1. Copy the necessary agent configuration directories (`.agent/`, `.claude/`, `.continue/`, `.qwen/`) from this central settings repository into your new project's root folder.
2. Make sure to copy the `GEMINI.md` logic file into the root path.
3. Configure API Keys in your local environment files (e.g., export `DASHSCOPE_API_KEY` for Qwen functionality).
4. Launch your IDE, install necessary extensions (such as Continue or Claude CLI), and start building!

**B. Integrating into an Existing Project**
1. Start by migrating the core structures (`.agent/` and `GEMINI.md`) carefully to establish standardized clean-code patterns.
2. Merge tool configurations. If you already have `.continue/` locally, blend its properties with the global configuration `.continue/.continuerc.json` carefully to prevent conflicting behaviors.
3. Keep your `.gitignore` updated intentionally. Make sure local/temporary memory files such as `*/memory/` are strictly ignored to prevent repo clutter.

</details>

<details>
<summary><b>Detailed Agent Configuration Setup</b></summary>

This project uses various AI coding agents including **Antigravity**, **Claude Code**, **Continue**, and **Qwen**. Below is a summary of how each tool is configured and where to find their setup files.

#### 1. Antigravity Agent
Antigravity utilizes the `.agent` specific structure alongside the global behavior file `GEMINI.md`.
- **Core Setting File**: `GEMINI.md` dictates the routing, logic, and operational phases (Rules) of the AI in this workspace.
- **Rules & Skills**: Defined in `.agent/skills/` directory and loaded selectively.
- **Workflow Commands**: Enforces strict phase gateways, such as `/brainstorm`, `/plan`, and `/create`.
- **Memory**: Context and logic traceability are preserved locally (typically in `.antigravity/memory/`).

#### 2. Claude Code
Claude Code leverages a strict configuration through the `.claude/` directory to manage permissions and capabilities.
- **Main settings**: Security and file access are specified in `.claude/settings.json`.
- **Architecture Base**: Detailed operations rules can be read in `.claude/ARCHITECTURE.md`.
- **MCP Services**: MCP integration files are specified inside `.claude/.mcp.json`.
- **Customization**: Agents and rules are stored as markdown files inside `.claude/rules/` and `.claude/agents/`.

#### 3. Continue (VS Code / JetBrains extension)
The Continue setup configures code context indexing and dynamic completions strictly inside `.continue/`.
- **Base Root Settings**: `.continue/.continuerc.json` connects markdown files dynamically into the context tree.
- **Rules**: Explicit custom instructions live inside `.continue/rules/*.md`.
- **Prompts & Specific Agents**: Custom UI workflow instructions live inside `.continue/prompts/` and `.continue/agents/`.

#### 4. Qwen
The Qwen configurations exist mostly locally using DashScope APIs inside the `.qwen/` folder.
- **Main Configuration**: Uses `.qwen/settings.json` for base permissions and defaults.
- **Model Details**: The `model.default` relies on `qwen-coder-plus-latest`.
- **Authentication**: Requires the system environment variable `$DASHSCOPE_API_KEY` to function.
- **Paths Setup**: The system strictly maps environmental context to directories like `.qwen/skills/` and `.qwen/agents/`.

</details>
