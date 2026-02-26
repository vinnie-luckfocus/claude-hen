# CLAUDE.md

本文档为 Claude Code (claude.ai/code) 提供本仓库的开发指导。

**重要提示：本项目的所有文档和对话均使用中文，本项目的开发必须严格遵守开发规范中的内容！**

## 开发规范（严格遵守！！！！！ 最重要！！！！）

### 规则路径
开发需要遵守的规则及新增规则均存放在 `/rules/`目录下

### 规则优先级
**开发必须严格遵守 `/rules/` 目录中的所有规范** 所有代码提交前必须确保符合相关规则要求。

### 任务规划
-  任务规划所涉及的文档均放在 `/plans/` 目录下
- **开发前必须先查看 `/plans/` 目录下对应的规划文档**
- **如无对应功能的规划文档，先创建规划文档**
- **开发进展必须严格遵循 plan 中的规划 ，如规划变更优先更新规划文档**
- **阶段任务完成，立刻更新规划文档**
- **阶段任务完成，代码通过审核后，提交修改**

### 任务规划编写要求

- **将更新规划文档写入任务规划中**
- **将代码审核任务编入任务规划中**
- **将规则中分形文档和回环检查编入任务规划中**

### 代码审核!!!
- **每完成一个phase的开发，必须调用 Codex 对修改的代码进行审核**
- 审核内容包括：
  - **代码逻辑审核**：检查代码正确性、性能、安全性、符合编码规范
  - **需求完成度审核**：检查是否完整实现了 plan 中定义的需求和验收标准
- 审核方式：对话中明确要求 Codex 进行代码审核
- **审核通过后方可进入下一个开发节点**
- **审核不通过的处理流程**：
  - 根据 Codex 的反馈意见逐条进行修改
  - 修改完成后，再次使用 Codex 进行审核
  - 重复此流程直至审核通过

### 分形文档三层更新（每Phase必须执行）!!!

根据 `/rules/FRACTAL_DOC.md` 规则，**每完成一个phase的开发，必须执行分形文档三层更新**：

**第3层（文件头）- 所有修改的文件**：
- 所有修改的 `.c` 和 `.h` 文件必须添加 History 条目
- 格式：`1.Date: YYYY-MM-DD  Author: [作者]  Modification: [变更说明]`
- 变更说明需简明扼要，如"新增 XXX 功能"、"修复 XXX 问题"

**第2层（组件 CLAUDE.md）- 如有修改**：
- 更新成员清单（新增/删除文件）
- 更新对外接口（新增/修改函数）
- 更新依赖关系（REQUIRES/PRIV_REQUIRES 变化）
- 更新 Kconfig 配置（如有新增配置项）

**第1层（根目录 CLAUDE.md）- 如有架构变化**：
- 更新模块结构表（新增组件、模块状态变化）
- 更新最近变更记录
- 更新分区配置（如修改分区表）

### 回环检查（每Phase必须执行）!!!

根据 `/rules/CONTEXT_CHECKLIST.md` 规则，**每完成一个phase的开发，必须执行回环检查**：

**分形文档同构性检查**：
```markdown
- [ ] 第3层：所有修改文件的 History 条目已更新
- [ ] 第3层：文件头 Description 与实际代码职责一致
- [ ] 第2层：组件 CLAUDE.md 成员清单包含所有文件
- [ ] 第2层：接口契约与实际 export 一致
- [ ] 第2层：REQUIRES/PRIV_REQUIRES 依赖关系准确
- [ ] 第1层：根目录 CLAUDE.md 模块结构表完整
```

**ESP-IDF 构建验证**：
```markdown
- [ ] `idf.py build` 构建成功
- [ ] 无编译警告（或已评估并接受）
- [ ] 链接成功，无未定义符号
```

**资源检查（如相关）**：
```markdown
- [ ] 新增任务栈大小合理（建议 2048-4096）
- [ ] 新增任务优先级合理（1-24，避免与系统任务冲突）
- [ ] 队列长度合理
- [ ] 动态内存使用可控
```

### 开发流程总结（严格遵守）

```
1. Read 3次 → 建立完整上下文（第3层 → 第2层 → 第1层）
2. Modify 代码 → 实现需求
3. Update 第3层 → 所有修改文件添加 History 条目
4. Update 第2层 → 如有需要更新组件 CLAUDE.md
5. Update 第1层 → 如有需要更新根目录 CLAUDE.md
6. Build 检查 → `idf.py build` 成功
7. Check 同构 → 三层文档一致性验证
8. Codex 审核 → 代码逻辑 + 需求完成度
9. 提交代码 → 只有通过审核后才能提交
```

**核心原则：三层文档，三次 Read，同构一致，变更回环，构建验证，审核通过。**

## 项目概述

本项目是 **planck Mini & Air** 智能牙刷的固件，基于 ESP32-C3 (RISC-V) 使用 ESP-IDF v5.5.3 开发。固件管理一个复杂的刷牙系统，包括基于 IMU 的姿态检测、压力感应、LCD UI、WiFi/BLE 连接以及 OTA 升级功能。

## 构建命令

### 前置条件
- 安装 ESP-IDF v5.5.3，路径：`/Users/luckfocus/esp/v5.5.3/esp-idf/`
- 目标芯片：`esp32c3` (RISC-V)
- Python 3.14+ (ESP-IDF 5.5 需要)

### 环境设置（每次新终端）

```bash
# 方法1：使用 export.sh 自动设置（推荐）
. /Users/luckfocus/esp/v5.5.3/esp-idf/export.sh

# 方法2：手动设置环境变量
export IDF_PATH=/Users/luckfocus/esp/v5.5.3/esp-idf
export PATH=$IDF_PATH/tools:$PATH
. $IDF_PATH/export.sh
```

### 常用命令

```bash
# 设置目标芯片（首次配置或删除 build 目录后）
idf.py set-target esp32c3

# 构建项目
idf.py build

# 烧录固件（自动检测串口）
idf.py flash

# 烧录固件并打开串口监控
idf.py flash monitor

# 仅打开串口监控（115200 波特率）
idf.py monitor

# 菜单配置
idf.py menuconfig

# 完全清理构建（删除 build 目录）
idf.py fullclean

# 仅重新编译（不清理）
idf.py app

# 退出监控：Ctrl+]
```

### 完整编译流程示例

```bash
# 1. 进入项目目录
cd /Users/luckfocus/Projects/smart-toothbrush

# 2. 激活 ESP-IDF 环境
. /Users/luckfocus/esp/v5.5.3/esp-idf/export.sh

# 3. 首次编译需要设置目标芯片
idf.py set-target esp32c3

# 4. 编译项目
idf.py build

# 5. 烧录到设备并监控输出
idf.py flash monitor
```

### 调试/发布构建
- 调试版本：保持 sdkconfig 中默认的 `CONFIG_BOOTLOADER_LOG_LEVEL_INFO`
- 发布版本：禁用调试输出 - 在 menuconfig 中设置 `CONFIG_BOOTLOADER_LOG_LEVEL_NONE` 并禁用 UART 输出

### ESP-IDF v5.5 注意事项

与 v4.4.8 相比的主要变化：
1. **GCC 版本**：从 GCC 8.4 升级到 GCC 14.2，检查更严格，可能产生更多警告
2. **API 弃用警告**：
   - `esp_spi_flash.h` → 使用 `spi_flash_mmap.h`
   - `esp_log_buffer_hex()` → 使用 `ESP_LOG_BUFFER_HEX` 宏
   - Legacy ADC driver → 使用 `esp_adc/adc_oneshot.h` 或 `esp_adc/adc_continuous.h`
3. **Python 要求**：需要 Python 3.14+
4. **分区警告**：若出现 `The smallest app partition is nearly full`，考虑优化代码或调整分区表

已知警告（可安全忽略）：
- `cabs` 内置函数冲突（`fft_float` 组件）
- 未使用变量警告（不影响功能）
- `volatile` 限定符丢弃警告（需要代码重构解决）

## 模块结构

| 模块 | 路径 | 核心职责 | 文档 |
|------|------|----------|------|
| **核心组件** ||||
| app_task | components/app_task | 主状态机，系统生命周期管理 | [CLAUDE.md](components/app_task/CLAUDE.md) |
| board | components/board | GPIO定义、硬件版本配置 | [CLAUDE.md](components/board/CLAUDE.md) |
| main | main | 程序入口 | [CLAUDE.md](main/CLAUDE.md) |
| **应用逻辑** ||||
| app_brush | components/app_brush | 刷牙检测逻辑和评分 | [CLAUDE.md](components/app_brush/CLAUDE.md) |
| app_lcd | components/app_lcd | LCD显示驱动和UI界面 | [CLAUDE.md](components/app_lcd/CLAUDE.md) |
| app_wifi | components/app_wifi | WiFi/HTTP/MQTT通信 | [CLAUDE.md](components/app_wifi/CLAUDE.md) |
| app_adc | components/app_adc | ADC采样（电池/压力） | [CLAUDE.md](components/app_adc/CLAUDE.md) |
| pose | components/pose | 姿态估计算法 | [CLAUDE.md](components/pose/CLAUDE.md) |
| brushing_ident | components/brushing_ident | 刷牙识别算法 | [CLAUDE.md](components/brushing_ident/CLAUDE.md) |
| **连接功能** ||||
| app_ota | components/app_ota | ESP32固件OTA升级 | [CLAUDE.md](components/app_ota/CLAUDE.md) |
| app_esc_ota | components/app_esc_ota | 电机控制器OTA升级 | [CLAUDE.md](components/app_esc_ota/CLAUDE.md) |
| dev_ble | components/dev_ble | BLE GATT服务 + 实时数据传输（50Hz IMU + 1Hz姿态 + 10Hz状态） | [CLAUDE.md](components/dev_ble/CLAUDE.md) |
| app_wireless | components/app_wireless | WiFi/BLE协调管理 | [CLAUDE.md](components/app_wireless/CLAUDE.md) |
| **硬件驱动** ||||
| dev_spi | components/dev_spi | SPI总线管理 | [CLAUDE.md](components/dev_spi/CLAUDE.md) |
| imu_hal | components/imu_hal | IMU硬件抽象层 | [CLAUDE.md](components/imu_hal/CLAUDE.md) |
| app_icm | components/app_icm | ICM-42670驱动 | [CLAUDE.md](components/app_icm/CLAUDE.md) |
| dev_wifi | components/dev_wifi | WiFi底层驱动 | [CLAUDE.md](components/dev_wifi/CLAUDE.md) |
| app_uart | components/app_uart | UART通信驱动 | [CLAUDE.md](components/app_uart/CLAUDE.md) |
| app_vibration | components/app_vibration | 振动控制 | [CLAUDE.md](components/app_vibration/CLAUDE.md) |
| dev_low_power | components/dev_low_power | 低功耗管理 | [CLAUDE.md](components/dev_low_power/CLAUDE.md) |
| **工具组件** ||||
| app_flash | components/app_flash | NVS存储管理 | [CLAUDE.md](components/app_flash/CLAUDE.md) |
| app_time | components/app_time | 时间/农历/节气 | [CLAUDE.md](components/app_time/CLAUDE.md) |
| app_score | components/app_score | 刷牙评分计算 | [CLAUDE.md](components/app_score/CLAUDE.md) |
| emath | components/emath | 数学运算库（IQ格式） | [CLAUDE.md](components/emath/CLAUDE.md) |
| fft_float | components/fft_float | FFT信号处理 | [CLAUDE.md](components/fft_float/CLAUDE.md) |
| app_crc | components/app_crc | CRC校验 | [CLAUDE.md](components/app_crc/CLAUDE.md) |
| app_md5 | components/app_md5 | MD5哈希 | [CLAUDE.md](components/app_md5/CLAUDE.md) |
| app_easy_json | components/app_easy_json | JSON处理 | [CLAUDE.md](components/app_easy_json/CLAUDE.md) |
| memory_check | components/memory_check | 内存检查 | [CLAUDE.md](components/memory_check/CLAUDE.md) |
| mj_tool | components/mj_tool | 通用工具 | [CLAUDE.md](components/mj_tool/CLAUDE.md) |
| shake_ident | components/shake_ident | 摇晃识别 | [CLAUDE.md](components/shake_ident/CLAUDE.md) |

## 架构

### 组件结构
组件按功能组织在 `/components/` 目录下：

**硬件抽象层：**
- `board/` - GPIO 定义、硬件版本配置（base_info.h、hardware_init.h）
- `dev_spi/` - SPI 总线管理（LCD 和 IMU 共享）
- `dev_wifi/`、`dev_ble/` - 无线通信驱动
- `imu_hal/` - IMU 硬件抽象层

**应用逻辑：**
- `app_task/` - **主状态机**（关键文件）。管理系统状态：HARDWARE_INIT → TASK_IDLE → MOVING_NO_BUSHING → BUSHING → PRE_BUSH_STOP → BUSH_STOP → DEEP_SLEEP_INIT
- `app_brush/` - 刷牙检测逻辑和评分
- `app_lcd/` - LCD 显示驱动和 UI 界面（0.96" TFT）
- `app_adc/` - 电池电压和压力传感器 ADC 读取
- `pose/` - 使用 IMU 数据进行刷牙角度检测的姿态估计算法
- `brushing_ident/` - 刷牙识别算法

**连接功能：**
- `app_wifi/` - WiFi HTTP 客户端、MQTT 数据上传、天气/时间同步
- `app_ota/` - ESP32 固件 OTA 升级
- `app_esc_ota/` - 通过 UART 升级电机控制器（SPD1148）固件
- `dev_ble/` - 用于 APP 通信的 BLE GATT 服务，支持实时数据传输（刷牙状态50Hz IMU+1Hz姿态，非刷牙状态10Hz状态包）

**工具组件：**
- `app_flash/` - NVS（非易失性存储），存储 SN 码、WiFi 凭证、设置
- `app_time/` - Unix 时间戳和时区管理
- `app_score/` - 刷牙评分计算
- `emath/`、`fft_float/` - 信号处理数学库

### 关键状态机
`app_task.c` 中的主状态机控制牙刷生命周期：

1. **HARDWARE_INIT** - 初始化 SPI、GPIO、IMU、LCD、ADC
2. **TASK_IDLE** - 等待运动/压力唤醒
3. **MOVING_NO_BUSHING** - 检测到运动但未刷牙
4. **BUSHING** - 正在刷牙，收集 IMU 数据
5. **PRE_BUSH_STOP** - 停止前 12 秒超时（允许恢复）
6. **BUSH_STOP** - 刷牙结束，计算评分，上传数据
7. **DEEP_SLEEP_INIT** → 进入深度睡眠

### 硬件配置
开发板变体通过 `base_info.h` 中的编译标志选择：
- 默认：planck Mini/Air（0.96" TFT、压力传感器）
- planckO2：不同的 GPIO 映射，用于 AMOLED 变体

关键硬件规格：
- **LCD**：0.96" TFT，160x80，SPI @ 15MHz
- **IMU**：ICM-42670（SPI @ 20MHz）
- **压力传感器**：基于 ADC，每台设备单独校准
- **电池**：3.7V 锂电池，使用分压电阻进行 ADC 监测
- **电机控制器**：通过 UART 连接的 SPD1148

### 分区表
`planck_partitions.csv`：
- ota_0 / ota_1：各 1980K（双 OTA 槽位）
- escdata：64K（电机控制器固件存储）
- nvs：16K（设置、SN 码、WiFi 凭证）

### 数据流
1. **刷牙检测**：IMU → 姿态估计 → brushing_ident → app_brush → 状态机
2. **压力监测**：ADC → app_adc → 压力阈值检测 → 振动提醒
3. **数据上传**：刷牙结束 → MQTT (EMQX) 通过 app_wifi → 云服务器
4. **OTA**：检查服务器 → 下载到 ota_x 分区 → 重启 → 验证 → 切换槽位

## 开发说明

### 调试输出
- 使用 `idf.py monitor` 查看串口输出（115200 波特率）
- 监控时自动解码堆栈回溯（若崩溃）

### 崩溃堆栈分析

当设备崩溃时，monitor 会输出类似以下的信息：
```
Backtrace: 0x42008a57:0x3fc9a2b0 0x42008a9a:0x3fc9a2d0 ...
```

分析方法：

```bash
# 方法1：使用 idf.py 自动解析（推荐）
idf.py monitor  # 崩溃时会自动解码堆栈

# 方法2：手动解析单个地址
riscv32-esp-elf-addr2line -pfiaC -e build/planckMini.elf 0x42008a57

# 方法3：使用脚本批量解析（Windows）
./idf_prase_backtrace.bat

# 方法4：使用 idf.py 解析堆栈字符串
idf.py monitor --print-filter="E"  # 仅显示错误
```

工具路径（ESP-IDF 5.5）：
```
/Users/luckfocus/.espressif/tools/riscv32-esp-elf/esp-14.2.0_20251107/riscv32-esp-elf/bin/riscv32-esp-elf-addr2line
```

### 调试标志（在 app_task.c 中）
- `DEBUG_NO_DEEP_SLEEP` - 调试时防止进入睡眠
- `DEBUGT_NO_UPDATE` - 调试时禁用 OTA

### 关键代码模式
1. **SPI 总线共享**：LCD 和 IMU 共享 SPI 总线，访问时需要互斥保护
2. **NVS 操作**：读取前始终检查键是否存在；SN 码有三重备份存储
3. **IMU 数据**：以 1kHz 时钟频率收集，用于姿态估计处理
4. **电源管理**：超时后进入深度睡眠；通过压力传感器中断或 IMU 运动唤醒

### 版本管理
- 版本定义在 `version.txt`（例如："02.00.16"）
- 未绑定时充电界面显示版本号
- OTA 检查服务器是否有新版本

### 出厂默认值（base_info.h）
- 默认 WiFi：`Manufacture` / `dgevowera`
- 服务器：`http://www.aiboshua.com`
- MQTT：`mqtt://zh-emq.evowera.com`
- 出厂 GPS：北京坐标 (116.38, 39.90)

## 文件规范

- 头文件遵循公司模板，包含版权和文件信息
- 业务逻辑代码可使用中文注释
- 变量命名：`u8_`、`u16_`、`i32_` 前缀表示无符号/有符号整数；`s` 前缀表示结构体
- 调试输出使用 `ESP_LOGx()` 宏或自定义 `LOGI()`/`LOGE()`

## 最近变更记录

| 日期 | 变更内容 | 作者 | 相关Phase |
|------|----------|------|-----------|
| 2026-02-26 | 创建通用项目初始化规范计划：提取现有开发规范为可复用模板 | Claude | Phase 1 规划 |
| 2026-02-26 | BLE实时数据传输Phase 4完成：功耗优化（动态频率、低电量保护） | Claude | Phase 4 |
| 2026-02-26 | BLE实时数据传输Phase 3完成：数据生产端集成（刷牙状态50Hz IMU+1Hz姿态，非刷牙状态10Hz状态包） | Claude | Phase 3 |
| 2026-02-26 | BLE实时数据传输Phase 2完成：GATT服务扩展、实时数据任务、连接状态管理、命令处理 | Claude | Phase 2 |
| 2026-02-26 | BLE实时数据传输Phase 1完成：数据包结构体定义、特征值索引、消息类型枚举 | Claude | Phase 1 |

