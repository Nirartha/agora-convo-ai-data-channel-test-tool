# Agora AI Agent Console (Voice & Text)

A lightweight, purely front-end web console designed for testing and interacting with Agora's Conversational AI Agent. It supports real-time voice and text interaction, and features a "Smart Dual-Channel" mode that allows seamless switching between legacy `DataStream` and the high-speed `RTM` (Real-Time Messaging) channels.

這是一個輕量級的純前端網頁控制台，專為測試與操作 Agora Conversational AI Agent 所設計。支援即時語音與文字互動，並具備「智慧雙通道」模式，可無縫切換傳統 `DataStream` 與高速 `RTM` 通道。

## Features / 核心功能

* **Dual-Channel Text Support (雙通道文字支援)**:
    * **DataStream Mode (Default)**: Uses the `/think` API for sending messages and `stream-message` for receiving transcripts. Perfect for environments without RTM enabled.
    * **RTM Mode (Toggle)**: Uses high-speed RTM for both sending and receiving text. Requires an RTM-enabled App ID and a dual-privilege Token.
    * *(DataStream 模式：使用 `/think` API 發送，`stream-message` 接收，適合未開啟 RTM 的環境。)*
    * *(RTM 模式：使用高速 RTM 收發文字，需具備開啟 RTM 的 App ID 與雙權限 Token。)*
* **Fail-Fast Token Validation (前端 Token 權限透視)**: Automatically parses the Agent's token (zlib/Base64) to verify if it contains the necessary RTM privileges (`ServiceRtm` / `0200`) before allowing RTM mode.
    * *(自動解析 Token 壓縮檔，提前攔截並警告不具備 RTM 權限的錯誤操作。)*
* **Echo Cancellation (防回音機制)**: Prevents the UI from duplicating messages when the Agent broadcasts the user's text back.
    * *(過濾 Agent 複誦的廣播，確保對話視窗乾淨。)*
* **Safety Locks (防呆鎖定)**: Disables protocol switching while the Agent is running, and prevents duplicate channel joins.
    * *(Agent 運行中禁止切換通道，避免狀態脫鉤；加入頻道後自動鎖定按鈕。)*

## How to Use / 使用說明

### 1. Preparation / 準備工作
1.  **Agora App ID**: Get your App ID from the [Agora Console](https://console.agora.io/).
2.  **Tokens**: Generate an RTC Token for yourself (`Human Token`) and one for the AI (`Agent Token`).
    * *Note: If you plan to use RTM mode, ensure your tokens include both RTC and RTM privileges.*
3.  **Customer ID & Secret**: Required for authenticating with the Agora AI Agent REST API.

### 2. Connect as Human / 連線控制端
1.  Enter your `App ID`, `Channel Name`, `Human UID` (must be a string, e.g., "1111"), and `Human Token`.
2.  (Optional) Check **"Switch to RTM Message Channel"** if your project has RTM enabled.
3.  Click **"Join Channel"**. Make sure to grant microphone permissions.
    * *(輸入連線資訊後點擊「加入頻道」，並允許麥克風權限。)*

### 3. Start the AI Agent / 啟動 AI Agent
1.  Enter your `Customer ID`, `Secret`, and `Agent Token`.
2.  The JSON Payload will automatically sync based on your input and channel mode.
3.  Click **"Start AI Agent"**.
    * *(輸入 API 憑證與 Token 後，點擊「啟動 AI Agent」，等待 Agent 加入頻道。)*

### 4. Interact / 開始互動
* **Voice**: Speak into your microphone. The AI will listen and respond with voice. Click "Manual Subscribe" if you cannot hear the AI.
* **Text**: Type in the input box at the bottom and press Enter. The message will be sent via `/think` API or RTM based on your mode.
    * *(語音：直接對麥克風說話；若聽不到 AI，可透過下拉選單手動訂閱。)*
    * *(文字：在下方輸入框打字發送，系統會自動根據當前模式切換發送 API。)*

## Important Notes / 注意事項
* **RTM Mode Requires Backend Support**: The RTM mode strictly requires your App ID to have "Presence Configuration" enabled in the Agora Console. Your Token generator must also pack `ServiceRtm` into the token.
    * *(RTM 模式必須在 Agora 後台開啟 Presence 功能，且 Token 生成器必須打包雙權限。)*
