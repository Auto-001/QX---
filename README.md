# Spotify Quantumult X 配置

将 app2smile 的 Quantumult X 规则与 devchristian1337 的新版配置处理逻辑整合，用于适配 Spotify iOS 9.1.98 起改用的 `remote-config-resolver` / `nebula` 接口。

> 配置已在手机端 Quantumult X 与 Spotify 中测试通过。Spotify 服务端接口或应用版本变化后，规则可能需要更新。

## 文件说明

- [`spotify-qx.conf`](./spotify-qx.conf)：Quantumult X 重写规则。
- [`spotify-qx.js`](./spotify-qx.js)：Protobuf 响应处理脚本，已适配 QX 的二进制响应 `bodyBytes`。
- [`app2smile.txt`](./app2smile.txt)：app2smile 原规则链接。
- [`devchristian1337.txt`](./devchristian1337.txt)：devchristian1337 的 Loon 规则参考。

## 支持的接口

脚本处理旧版和新版配置接口：

| 接口 | 用途 |
| --- | --- |
| `bootstrap/v1/bootstrap` | 旧版启动配置 |
| `user-customization-service/v1/customize` | 旧版自定义配置 |
| `remote-config-resolver/v*/configuration` | 新版远程配置 |
| `nebula/v*/configuration/attributes` | 新版账号属性 |

配置同时包含请求头规则，用于移除 `If-None-Match`，避免客户端继续使用未修改的 304 缓存响应。

## 安装

1. 在 Quantumult X 中添加 [`spotify-qx.conf`](./spotify-qx.conf) 的规则。
2. 确认 Quantumult X 已启用 HTTPS 解密，并将 `spclient.wg.spotify.com` 和 `*.spclient.spotify.com` 加入 MITM 主机名。
3. 确认脚本可以从配置中的 GitHub Raw 地址加载：
   `https://raw.githubusercontent.com/Auto-001/QX---/refs/heads/main/spotify-qx.js`
4. 启用规则后，重新打开 Spotify。若配置仍未刷新，可重新登录或重启应用。

## 更新与排查

- 修改脚本或规则后，请确认对应文件已推送到 GitHub，且 Raw 链接可以直接打开。
- 如果脚本无法执行，先检查 QX 的 HTTPS 解密、MITM 主机名和脚本权限。
- 如果仍看到旧配置，检查请求头规则是否启用，并重新启动 Spotify。
- 应用接口变化时，可对照 devchristian1337 的上游脚本更新 Protobuf 处理逻辑。

## 来源

- [app2smile/rules](https://github.com/app2smile/rules)
- [devchristian1337/loon-plugins](https://github.com/devchristian1337/loon-plugins)
- 本项目中的 QX 脚本保留上游作者信息，并对 QX 二进制响应读写方式进行了适配。
