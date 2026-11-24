# xaga 5.10.240 实验内核

这个仓库模板用于在 GitHub Actions 上编译 ztc1997 的 Android 12 GKI 5.10.240 内核。流程固定源码提交 `6596b957a660` 和 Android Clang 21 构建 `14229180`，使用该源码的 ARM64 GKI 配置。

## 放到 GitHub

1. 解压 ZIP。将 `.github/workflows/build-xaga-kernel.yml` 和 `patches/` 下的两个补丁按原路径提交到你有写权限的仓库默认分支。
2. 打开仓库的 **Actions → Build xaga 5.10.240 test Image → Run workflow**。
3. 选择构建项，等待任务结束。进入该次运行，在 **Artifacts** 下载产物。

| 构建项 | 改动 |
| --- | --- |
| `lz4kd` | 注册 LZ4KD 作为 4 KiB 页的可选 ZRAM 压缩算法，建议最先构建。 |
| `teo` | 回移植 ESK xaga v3.1 的 TEO 空闲调度，沿用原内核的 TEO 选择优先级。 |
| `lz4kd-teo` | 同时合入上述两个补丁，适合单项验证后再构建。 |

产物含 ARM64 `Image`、内核配置、源码提交号、编译器版本和 SHA-256。它用于编译和兼容性验证。准备刷机包之前，还需要核对澎湃 OS 2 的厂商模块兼容性及已有启动镜像。

如果最终使用 LZ4KD，仍需让 ROM 的 ZRAM 初始化流程选中 `lz4kd`。测试 TEO 时，先确认 `/sys/devices/system/cpu/cpuidle/current_governor` 显示 `teo`。

本地验证过两个源码补丁都能应用到固定提交中的对应文件。GitHub 云端编译和小米 xaga 真机测试仍待执行。
