# 马到橙功.ai 助手

面向吃喝玩乐与出行场景的网页助手。前端提供对话和地图界面，Flask 后端提供规划、天气、地点检索和美团相关工具接口。

## 项目结构

| 路径 | 作用 |
| --- | --- |
| `index.html` | 可直接打开的网页入口 |
| `frontend/index.html`、`frontend/style.css` | 前端页面与样式源文件 |
| `backend/app.py` | Flask API、对话与行程规划入口 |
| `backend/hei_zhen_zhu_local.py`、`backend/michelin_rag.py` | 餐饮推荐与可选的知识检索 |
| `backend/meituan-travel/` | 美团旅行技能 |
| `backend/meituan-fenxiao-promotion-coupon/` | 分销券技能与命令 |
| `backend/meituan-venue-guide/` | 场馆导览技能与授权脚本 |
| `backend/mt-paotui/` | 跑腿技能、文档与 Node.js 运行脚本 |
| `render.yaml`、`requirements.txt`、`runtime.txt` | 部署和 Python 依赖配置 |

## 本地运行

需要 Python 3.11。部分美团技能还需要 Node.js 18 或更高版本。

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python backend/app.py
```

后端默认监听 `http://127.0.0.1:5000`。访问 `http://127.0.0.1:5000/api/health` 检查服务状态，再在浏览器中打开根目录的 `index.html`。本地页面默认连接该端口；需要连接其他后端时，可在页面 URL 中添加 `?backend=https://your-backend.example`。

## 配置

使用环境变量或仅保存在本地的 `backend/.env` 配置外部服务。常用变量包括 `DEEPSEEK_API_KEY`、`LONGCAT_API_KEY`、`AMAP_WEBSERVICE_KEY`、`AMAP_JSAPI_KEY`、`BAIDU_SERVER_AK`、`BAIDU_TRANSLATE_KEY` 和 `MEITUAN_API_KEY`。没有对应凭据时，相关在线能力可能不可用或使用代码中的回退逻辑。不要将 `.env`、数据库、聊天记录或真实密钥提交到仓库。

`backend/michelin_rag.py` 的向量检索还依赖额外的 LangChain/Chroma 包和 `backend/rag_documents.csv`。该数据集及生成的向量库不在本仓库中；未配置时，主服务会尝试继续运行，但检索能力不可用。

## 部署与接口

`render.yaml` 定义了 Render 上的 Python 服务，启动命令使用 `gunicorn --chdir backend app:app`。部署时在平台环境变量中设置所需密钥，不要写进配置文件。可先检查 `GET /api/health`，再根据需要调用 `POST /api/chat`、`POST /api/agent`、`POST /api/plan_route` 等接口。

各美团技能的具体命令和参数见对应目录下的 `SKILL.md`、`README.md` 与 `references/`。
