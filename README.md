```mermaid
graph TD
    A[程序启动] -->|用户输入账号| B[用户登录 BiliFavGUI.login()]
    B -->|API 验证 Cookie| C[验证成功 Bili_fav.login()]
    B -->|验证失败| D[提示错误]

    C -->|输入收藏夹ID| E[加载收藏夹 BiliFavGUI.list_favorites()]
    E -->|调用 API 获取视频列表| F[解析视频列表 Bili_fav.get_fav_list()]
    F --> G[显示收藏夹内容 BiliFavGUI.list_favorites_thread()]

    G -->|获取视频格式| H[请求可用格式 BiliFavGUI.get_video_formats_task()]
    H -->|成功| I[显示可用格式]
    H -->|失败| J[提示格式获取失败]

    I -->|选择格式| K[开始下载 BiliFavGUI.quick_download()]
    K -->|执行 you-get| L[下载视频 Bili_fav.download_video()]
    L --> M[监控下载进度 Bili_fav._monitor_download_progress()]
    M -->|下载完成| N[更新 GUI 状态]
    M -->|下载失败| O[记录错误并重试]

    N --> P[下载完成，等待用户操作]

    subgraph "控制下载"
      K -.-> Q[暂停下载 BiliFavGUI.pause_all_downloads()]
      Q -.-> M
      K -.-> R[恢复下载 BiliFavGUI.resume_all_downloads()]
      R -.-> M
      K -.-> S[取消下载 BiliFavGUI.cancel_all_downloads()]
      S -.-> T[终止进程]
    end
