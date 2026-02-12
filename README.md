# WeightStation - 重量管理系統

自動化製作 Windows EXE 執行檔的版本控制倉庫。

## 功能

- 車重/空車自動讀取（RS232/USB轉接）
- 商品與客戶管理
- 交易記錄數據庫存儲
- 結帳鎖定機制

## 開發環境要求

- Python 3.8+
- pyserial >= 3.5
- tkinter (通常內建於 Python)

## 本地開發

```bash
# 克隆倉庫
git clone https://github.com/yourusername/WeightStation.git
cd WeightStation

# 建立虛擬環境
python -m venv venv

# 啟用虛擬環境
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# 安裝依賴
pip install -r requirements.txt

# 執行應用
python main.py
```

## 製作 EXE 檔

### 自動化方式（GitHub Actions）

1. 推送代碼至 GitHub
2. 建立 git tag：`git tag -a v1.0.0 -m "Release v1.0.0"`
3. 推送 tag：`git push origin v1.0.0`
4. GitHub Actions 會自動構建 EXE 並在 Release 中提供下載

### 本地製作

```bash
# 安裝 PyInstaller
pip install PyInstaller

# 生成 EXE
pyinstaller --onefile \
  --windowed \
  --name WeightStation \
  --icon icon.ico \
  --add-data "CustomerID.db;." \
  --add-data "ItemValue.db;." \
  --hidden-import=serial \
  main.py

# 成品會在 dist/ 資料夾
```

## 文件結構

```
WeightStation/
├── .github/
│   └── workflows/
│       └── build-exe.yml          # GitHub Actions 工作流
├── main.py                         # 主程式入口
├── InfoSet.py                      # 商品/客戶設置模組
├── AddCargo.py                     # 車牌/廠商輸入模組
├── NoPad.py                        # 數值鍵盤模組
├── requirements.txt                # Python 依賴
├── setup.py                        # 打包設置
├── .gitignore                      # Git 忽略規則
└── README.md                       # 此檔案
```

## GitHub Actions 工作流說明

工作流檔案 `.github/workflows/build-exe.yml` 會：

1. 在 Windows 環境安裝 Python
2. 安裝所有依賴
3. 使用 PyInstaller 構建 EXE
4. 上傳 EXE 至 Artifacts（每次推送）
5. 若是 Release tag，自動建立 Release 並附加 EXE 檔案

## 更新 GitHub Actions 工作流

如需調整構建流程，編輯 `.github/workflows/build-exe.yml` 並推送即可。

## 常見問題

**Q：為什麼 EXE 製作失敗？**
A：檢查 `build-exe.yml` 中的 PyInstaller 命令，確保所有依賴已在 `requirements.txt` 中。

**Q：如何包含自訂圖標？**
A：將 `icon.ico` 放在專案根目錄，工作流會自動使用。

**Q：EXE 檔案在哪裡下載？**
A：
- 每次推送：Actions 頁面 → 最新工作流 → Artifacts
- Release 版本：Releases 頁面 → 對應版本下載

## 授權

MIT License
