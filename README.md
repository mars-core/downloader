MarsCore（火星代码）——由中国开发者Mars（原名MA YU CHAO ）创立开发.
marscore.downloader是一个高性能的多进程、多线程下载库，支持批量URL下载、断点续传、进度监控和自动重试等功能。

**完整参数详解与调用示例：**

**安装：**

pip install marscore
import marscore

*1. 单URL下载*

result = marscore.downloader(
    url="https://example.com/large-file.zip"
)
*2. 批量URL下载*
# 批量URL下载 - 自动并行处理
urls = [
    "https://example.com/file1.zip",
    "https://example.com/file2.zip", 
    "https://example.com/file3.zip"
]
results = marscore.downloader(urls=urls)


3. process_num - 进程并发数
# 场景：批量下载大量小文件（图片、文档）下载100个文件，开启100个进程完全并行
urls = [f"https://cdn.com/video_{i}.mp4" for i in range(10)]
results = marscore.downloader(
    urls=urls,
    process_num=100  # 100个进程并行下载
)

4. threads - 线程并发数
# 场景：大文件高速下载，单文件20线程分块
result = marscore.downloader(
    url="https://example.com/4k-movie.mp4",  # 2GB大文件
    threads=20  # 20个线程并发下载分块
)

5. 进程+线程组合优化
# 场景：同时下载10个大视频文件，每个文件多线程分块
urls = [f"https://video-cdn.com/movie_{i}.mp4" for i in range(10)]
results = marscore.downloader(
    urls=urls,
    process_num=10,  # 10进程并行
    threads=16       # 每个文件16线程分块
)

6. retry_num - 最大重试次数
# 场景：不稳定网络环境下的可靠下载
results = marscore.downloader(
    urls=urls,
    process_num=8,
    threads=12,
    retry_num=5  # 最多重试5次
)

7. buffer_size_mb - 内存缓冲区大小

# 场景：高速网络下的大文件下载，减少磁盘IO
result = marscore.downloader(
    url="https://example.com/database-backup.tar.gz",
    threads=16,
    buffer_size_mb=100  # 100MB内存缓冲区
)
8. chunk_size - 数据块大小

# 场景：优化网络包大小，提升传输效率
result = marscore.downloader(
    url="https://example.com/large-file.iso",
    chunk_size=32768  # 32KB数据块
)
价值亮点：

📦 网络优化：匹配网络MTU，减少分包
🔄 流式处理：支持真正的流式下载
📊 进度精准：小块数据实现更细粒度进度跟踪


9. output_dir - 输出目录
# 场景：组织化文件存储
results = marscore.downloader(
    urls=urls,
    process_num=4,
    output_dir="downloads/videos"  # 指定下载目录
)

价值亮点：
🗂️ 文件组织：自动创建目录结构
🔒 冲突避免：每个URL独立子目录
📁 指纹识别：相同URL不同参数自动区分

10. output_filename - 自定义文件名
# 场景：批量下载统一命名
urls = [
    "https://api.com/download?file=report2024",
    "https://api.com/download?file=data2024"
]
results = marscore.downloader(
    urls=urls,
    output_filename="enterprise_data"  # 统一文件名
)
价值亮点：
🏷️ 命名控制：覆盖自动提取的文件名
🔄 批量一致性：批量下载统一命名规则
📝 特殊字符处理：自动处理URL中的异常字符

11. timeout - 请求超时
# 场景：慢速网络或大文件下载
result = marscore.downloader(
    url="https://slow-server.com/large-file.zip",
    timeout=60,  # 60秒超时
    retry_num=3
)
价值亮点：
⏱️ 连接控制：防止慢连接阻塞线程
🔄 快速恢复：及时超时触发重试机制
🌐 环境适应：适应不同网络质量

12. headers - 自定义请求头
# 场景：需要认证或特殊头的API下载
result = marscore.downloader(
    url="https://api.company.com/secure-download",
    headers={
        'User-Agent': 'EnterpriseDownloader/1.0',
        'Authorization': 'Bearer xyz123',
        'X-Client-ID': 'your-client-id'
    },
    threads=8
)
价值亮点：
🔐 认证支持：支持各种认证方式
🌍 UA模拟：绕过下载限制
🎯 API集成：直接调用需要认证的下载接口

13. proxies - 代理配置
# 场景：企业网络环境或跨境下载
results = marscore.downloader(
    urls=urls,
    process_num=6,
    proxies={
        'http': 'http://corporate-proxy:8080',
        'https': 'https://corporate-proxy:8080'
    }
)
价值亮点：

🌐 网络穿透：企业防火墙环境
🔒 安全访问：通过代理访问内部资源
🌍 地域访问：跨境下载优化

14. method - HTTP方法
# 场景：需要POST请求的下载接口
result = marscore.downloader(
    url="https://api.service.com/download",
    method='post',  # 使用POST方法
    data={
        'file_id': '12345',
        'access_token': 'your-token'
    },
    threads=12
)

价值亮点：
🔄 方法灵活：支持GET/POST/PUT等
📝 表单下载：需要提交数据的下载场景
🔐 安全下载：Token等敏感数据通过POST发送

15. verify - SSL验证控制
# 场景：自签名证书或测试环境
result = marscore.downloader(
    url="https://internal-server/secure-file",
    verify=False,  # 关闭SSL验证
    threads=8
)
价值亮点：
🔓 证书绕过：自签名证书环境
🛡️ 测试便利：开发和测试环境
⚡ 连接加速：减少SSL握手开销


综合实战示例
场景1：企业级批量下载
python
# 企业数据同步 - 100个数据文件
urls = [f"https://data-warehouse/reports/daily_{i}.csv" for i in range(100)]

results = marscore.downloader(
    urls=urls,
    process_num=16,      # 16进程并行
    threads=8,           # 每个文件8线程
    retry_num=3,         # 3次重试
    output_dir="enterprise_data",
    buffer_size_mb=50,   # 50MB缓冲区
    timeout=30,          # 30秒超时
    headers={
        'User-Agent': 'DataSyncBot/1.0',
        'Authorization': 'Token enterprise-key-123'
    }
)
场景2：媒体文件高速下载
python
# 视频平台批量下载 - 20个高清视频
urls = [f"https://cdn.videoplatform.com/movie_{i}_1080p.mp4" for i in range(20)]

results = marscore.downloader(
    urls=urls,
    process_num=10,      # 10进程并行
    threads=20,          # 每个视频20线程分块
    buffer_size_mb=200,  # 200MB大缓冲区
    chunk_size=65536,    # 64KB大块传输
    retry_num=5,         # 5次重试保障
    output_dir="videos/hd_movies"
)
场景3：科研数据下载
python
# 科研数据集下载 - 大量小文件
urls = [
    f"https://research-data.org/dataset/part_{i:05d}.json" 
    for i in range(500)
]

results = marscore.downloader(
    urls=urls,
    process_num=32,      # 大量进程处理小文件
    threads=2,           # 小文件不需要多线程
    buffer_size_mb=10,   # 小缓冲区
    chunk_size=4096,     # 小数据块
    output_dir="research_dataset",
    retry_num=2
)


📊 性能配置参考表
场景类型	推荐进程数	推荐线程数	缓冲区	重试次数
大量小文件	高(16-32)	低(2-4)	小(10-20MB)	2-3
少量大文件	中(4-8)	高(16-24)	大(100-200MB)	3-5
混合类型	中(8-16)	中(8-12)	中(50-100MB)	3-4
不稳定网络	低(4-8)	中(8-12)	中(20-50MB)	5-8
