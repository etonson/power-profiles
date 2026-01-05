當然可以，我幫你把剛剛針對 **AMD + Ubuntu 24.04** 改寫、有效控制風扇的版本整理成 **Markdown 文件**，方便閱讀與使用：

---

# 💻 CPU 模式切換腳本（AMD + Ubuntu 24.04）

這套腳本針對 **Dell 14 DC14255 + AMD Ryzen + Ubuntu 24.04**，能實際控制 **EPP 與 CPU boost**，讓風扇安靜或全力運行。

---

## 1️⃣ quiet-dev（寫 code / 日常安靜）

```bash
#!/bin/bash
set -e

echo "▶ quiet-dev: 安靜開發模式 (AMD)"

# 1. 設低功耗 profile
powerprofilesctl set power-saver

# 2. AMD EPP = power（抑制瞬間拉頻）
for cpu in /sys/devices/system/cpu/cpu*/cpufreq/energy_performance_preference; do
  [ -f "$cpu" ] && echo power | tee $cpu > /dev/null
done

# 3. 關閉 CPU boost
if [ -f /sys/devices/system/cpu/cpufreq/boost ]; then
  echo 0 | tee /sys/devices/system/cpu/cpufreq/boost > /dev/null
fi

echo "✓ quiet-dev applied: power-saver profile, EPP=power, boost off"
```

---

## 2️⃣ focus-build（編譯 / 測試）

```bash
#!/bin/bash
set -e

echo "▶ focus-build: 編譯 / 測試模式 (AMD)"

# 1. 設平衡 profile
powerprofilesctl set balanced

# 2. AMD EPP = balance_performance
for cpu in /sys/devices/system/cpu/cpu*/cpufreq/energy_performance_preference; do
  [ -f "$cpu" ] && echo balance_performance | tee $cpu > /dev/null
done

# 3. 開啟 CPU boost
if [ -f /sys/devices/system/cpu/cpufreq/boost ]; then
  echo 1 | tee /sys/devices/system/cpu/cpufreq/boost > /dev/null
fi

echo "✓ focus-build applied: balanced profile, EPP=balance_performance, boost on"
```

---

## 3️⃣ full-power（極致效能 / benchmark）

```bash
#!/bin/bash
set -e

echo "▶ full-power: 極致效能模式 (AMD)"

# 1. 設 performance profile
powerprofilesctl set performance

# 2. AMD EPP = performance
for cpu in /sys/devices/system/cpu/cpu*/cpufreq/energy_performance_preference; do
  [ -f "$cpu" ] && echo performance | tee $cpu > /dev/null
done

# 3. 開啟 CPU boost
if [ -f /sys/devices/system/cpu/cpufreq/boost ]; then
  echo 1 | tee /sys/devices/system/cpu/cpufreq/boost > /dev/null
fi

echo "✓ full-power applied: performance profile, EPP=performance, boost on"
```

---

## 4️⃣ system-default（回復系統預設）

```bash
#!/bin/bash
set -e

echo "▶ system-default: 回復系統預設 (AMD)"

# 1. 回到平衡 profile
powerprofilesctl set balanced

# 2. AMD EPP = default
for cpu in /sys/devices/system/cpu/cpu*/cpufreq/energy_performance_preference; do
  [ -f "$cpu" ] && echo default | tee $cpu > /dev/null
done

# 3. CPU boost 開啟
if [ -f /sys/devices/system/cpu/cpufreq/boost ]; then
  echo 1 | tee /sys/devices/system/cpu/cpufreq/boost > /dev/null
fi

echo "✓ system-default applied: balanced profile, EPP=default, boost on"
```

---

## 🔧 安裝與使用建議

```bash
mkdir -p ~/bin
# 將四個腳本存入 ~/bin
chmod +x ~/bin/*.sh
```

### 建議 alias（放在 ~/.zshrc 或 ~/.bashrc）

```bash
alias qd='~/bin/quiet-dev.sh'
alias fb='~/bin/focus-build.sh'
alias fp='~/bin/full-power.sh'
alias sd='~/bin/system-default.sh'
```

重載 shell：

```bash
source ~/.zshrc  # 或 source ~/.bashrc
```

使用方式：

```bash
qd    # 安靜開發模式
fb    # 編譯 / 測試模式
fp    # 極致效能模式
sd    # 回復系統預設
```

---

## 🧠 特點總結

| 模式             | EPP                 | Boost | Power Profile | 適用場景                |
| -------------- | ------------------- | ----- | ------------- | ------------------- |
| quiet-dev      | power               | off   | power-saver   | 開發 / 日常，風扇安靜        |
| focus-build    | balance_performance | on    | balanced      | 編譯 / 測試，效能穩定        |
| full-power     | performance         | on    | performance   | Benchmark / 壓測，全力運行 |
| system-default | default             | on    | balanced      | 回復系統預設設定            |

---

這套 **MD 文件**完全對應你 Dell 14 AMD + Ubuntu 24.04 的實際控制方式，比你原本的 Intel 版 cpupower 腳本 **更可靠、風扇安靜效果明顯**。

---

如果你願意，我可以再幫你做一個 **一鍵安裝腳本**，把四個模式搬到 `/usr/local/bin`、去掉 `.sh`、生成 alias，一條命令就可以全部生效。

你想要我幫你做嗎？

