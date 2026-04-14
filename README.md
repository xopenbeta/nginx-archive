# nginx-archive

自动构建并发布 nginx 预编译二进制包，覆盖 macOS、Linux 与 Windows 的 x86_64 和 arm64 目标。

## 触发构建

```bash
git tag v202604141950
git push
git push origin v202604141950
```

构建完成后，GitHub Release 页面会自动发布全部支持平台与架构的二进制包。

## 支持版本

| 系列 | 最新版本 | 类型 |
|------|---------|------|
| 1.26 | 1.26.x | Stable |
| 1.28 | 1.28.x | Mainline |

## 构建环境

| 平台 | 架构 | Runner | 说明 |
|------|------|--------|------|
| macOS | x86_64 | `macos-14` | 在 Apple Silicon runner 上通过 Rosetta 与 x86_64 Homebrew 依赖链构建 |
| macOS | arm64 | `macos-14` | 最低部署目标 `macOS 12.0` |
| Linux | x86_64 | `ubuntu-22.04` | 原生构建 |
| Linux | arm64 | `ubuntu-22.04-arm` | 原生构建 |
| Windows | x86_64 | `windows-2022` | MSYS2 MINGW64 |
| Windows | arm64 | `windows-2022` | 在 x86_64 runner 上通过 MSYS2 clang/lld 交叉编译 |

说明：由于 GitHub Actions 已不再稳定提供 `macos-13`，macOS x86_64 产物改为在 `macos-14` Apple Silicon runner 上通过 Rosetta 与 x86_64 Homebrew 依赖链进行构建。

## 构建配置

- **SSL**: OpenSSL 3.x
- **启用模块**: `http_ssl_module`、`http_v2_module`、`http_realip_module`、`http_stub_status_module`、`http_gzip_static_module`、`http_sub_module`、`stream`、`stream_ssl_module`
- **源码**: 从 [nginx.org](https://nginx.org/en/download.html) 下载

## 使用方法

从 [Releases](../../releases) 页面下载对应架构的压缩包：

```bash
# macOS / Linux
tar xzf nginx-VERSION-OS-ARCH.tar.gz
cd nginx-VERSION-OS-ARCH

# 测试配置
./sbin/nginx -t -c $(pwd)/conf/nginx.conf

# 启动服务
./sbin/nginx -c $(pwd)/conf/nginx.conf

# 重载配置
./sbin/nginx -s reload

# 关闭服务
./sbin/nginx -s quit
```

Windows 包为 `.zip` 格式，解压后目录结构与 macOS / Linux 产物一致。

## 校验文件完整性

每个压缩包附带 SHA256 校验文件；macOS 和 Linux 产物对应 `.tar.gz.sha256`，Windows 产物对应 `.zip.sha256`。

```bash
shasum -a 256 -c nginx-VERSION-OS-ARCH.tar.gz.sha256
```
