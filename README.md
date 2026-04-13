# nginx-archive

为 macOS 预编译的 nginx 二进制包，支持 Intel (x86_64) 和 Apple Silicon (arm64)。

## 触发构建

```bash
git tag v202603191508
git push
git push origin v202603191508
```

构建完成后，GitHub Release 页面会自动发布全部支持版本在 arm64 与 x86_64 两个目标架构上的二进制包。

## 支持版本

| 系列 | 最新版本 | 类型 |
|------|---------|------|
| 1.26 | 1.26.x | Stable |
| 1.27 | 1.27.x | Mainline |

## 构建环境

| 架构 | Runner | 最低 macOS |
|------|--------|-----------|
| x86_64 (Intel) | `macos-14` | 12.0 Monterey |
| arm64 (Apple Silicon) | `macos-14` | 12.0 Monterey |

说明：由于 GitHub Actions 已不再稳定提供 `macos-13`，x86_64 产物改为在 `macos-14` Apple Silicon runner 上通过 Rosetta 与 x86_64 Homebrew 依赖链进行交叉编译。

## 构建配置

- **SSL**: OpenSSL 3.x
- **启用模块**: `http_ssl_module`、`http_v2_module`、`http_realip_module`、`http_stub_status_module`、`http_gzip_static_module`、`http_sub_module`
- **源码**: 从 [nginx.org](https://nginx.org/en/download.html) 下载

## 使用方法

从 [Releases](../../releases) 页面下载对应架构的压缩包：

```bash
# 解压
tar xzf nginx-VERSION-macos-ARCH.tar.gz
cd nginx-VERSION-macos-ARCH

# 测试配置
./sbin/nginx -t -c $(pwd)/conf/nginx.conf

# 启动服务
./sbin/nginx -c $(pwd)/conf/nginx.conf

# 重载配置
./sbin/nginx -s reload

# 关闭服务
./sbin/nginx -s quit
```

## 校验文件完整性

每个压缩包附带 SHA256 校验文件：

```bash
shasum -a 256 -c nginx-VERSION-macos-ARCH.tar.gz.sha256
```
