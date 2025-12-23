## 1️⃣ quiet-dev（寫 code / 日常安靜）

```bash
#!/bin/bash
set -e

echo "▶ quiet-dev: 安靜開發模式"

# 1. 設低功耗 profile
powerprofilesctl set power-saver

# 2. 限制 CPU 最大頻率到 3.2GHz（甜蜜點）
sudo cpupower frequency-set -u 3200MHz

# 3. 使用 powersave governor
sudo cpupower frequency-set -g powersave

echo "✓ quiet-dev applied: max freq 3.2GHz, powersave governor"
```

---

## 2️⃣ focus-build（編譯 / 測試）

```bash
#!/bin/bash
set -e

echo "▶ focus-build: 編譯 / 測試模式"

# 1. 設平衡 profile
powerprofilesctl set balanced

# 2. 放開 CPU 上限到最大 3.51GHz
sudo cpupower frequency-set -u 3510MHz

# 3. 使用 performance governor
sudo cpupower frequency-set -g performance

echo "✓ focus-build applied: max freq 3.51GHz, performance governor"
```

---

## 3️⃣ full-power（極致效能 / benchmark）

```bash
#!/bin/bash
set -e

echo "▶ full-power: 極致效能模式"

# 1. 設 performance profile
powerprofilesctl set performance

# 2. CPU 上限最大
sudo cpupower frequency-set -u 3510MHz

# 3. 使用 performance governor
sudo cpupower frequency-set -g performance

echo "✓ full-power applied: max freq 3.51GHz, performance governor"
```

---

## 4️⃣ system-default（回復系統預設）

```bash
#!/bin/bash
set -e

echo "▶ system-default: 回復系統預設"

# 1. 回到 OS 預設平衡 profile
powerprofilesctl set balanced

# 2. CPU 上限最大值
sudo cpupower frequency-set -u 3510MHz

# 3. 回到預設 governor (powersave/performance 根據系統)
sudo cpupower frequency-set -g powersave

echo "✓ system-default applied: restored to system defaults"
```

---

## 🔧 建議放置與使用

```bash
mkdir -p ~/bin
# 存成四個檔案：quiet-dev, focus-build, full-power, system-default
chmod +x ~/bin/*
```

#### Optional：設定 alias

在 `~/.bashrc` 或 `~/.zshrc`：

```bash
alias qd='quiet-dev'
alias fb='focus-build'
alias fp='full-power'
alias sd='system-default'
```

重啟 shell 後：

```bash
qd    # 開發安靜模式
fb    # 編譯 / 測試模式
fp    # 極致效能模式
sd    # 回復預設
```

---

## 🧠 特點總結

1. **quiet-dev** → 3.2GHz + powersave → 風扇幾乎不吵，寫 code 最舒服
2. **focus-build** → 3.51GHz + performance → 編譯 / 測試效能全開
3. **full-power** → performance profile + max freq → benchmark / 壓測用
4. **system-default** → 回復平衡 / 預設 governor → 安全回復

✅ 這套方案 **完全對應你 Ryzen AI 7 H 350 實測範圍**
✅ 可隨時切換
✅ reboot 後仍可回復預設

---
