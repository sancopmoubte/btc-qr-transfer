# BTC 离线二维码转账

这是基于 [`btc-quantum-miner`](https://github.com/sancopmoubte/btc-quantum-miner) 的独立静态网页版本。原网页不做任何修改；本项目保留身份、余额、ECDSA 签名和离线转账功能，移除挖矿与提现入口，核心改动是：

- 发起转账后生成二维码，不再要求复制一长串 Base64 凭证。
- 接收方上传收到的二维码图片即可扫描并完成转账。
- 支持下载和系统分享二维码图片。
- 保留“手动粘贴旧版凭证”作为兼容备用入口。
- 凭证格式使用 `BTCQ1.` 前缀，并兼容原网页的无前缀 Base64 凭证。
- 新生成二维码使用更紧凑的二进制 `BTC3.` 格式，只编码必要字段，尽量降低二维码复杂度；同时继续兼容 `BTCQ2.`、`BTCQ1.` 和原网页凭证。
- 每次生成二维码时，同时显示可复制的原始 Base64 凭证；如果屏幕截图二维码无法识别，可以复制该凭证，在接收页的备用输入框中验证接收。
- 二维码生成、识别和压缩脚本已经放在仓库的 `vendor/` 目录，不依赖 CDN、不依赖构建工具、不依赖 GitHub Actions。

## 使用流程

1. 在“身份”中生成身份。
2. 将接收方的公钥粘贴到“发起转账”，输入可用余额和金额并生成二维码。
3. 把二维码图片发送给对方。
4. 接收方在“扫码接收”中上传二维码图片，验证后入账。

## 本地运行

可以直接双击 `index.html` 打开。二维码生成和二维码图片上传识别不需要网络。

也可以在项目目录运行：

```bash
python3 -m http.server 4173
```

然后打开 <http://127.0.0.1:4173/>。

## GitHub Pages

本项目不再使用 GitHub Actions 部署。GitHub 仓库中直接包含可以托管的 `index.html`、`vendor/` 和 `.nojekyll`。

在仓库设置中选择 **Settings → Pages → Deploy from a branch → main → / (root)**，保存后即可使用 GitHub Pages。也可以直接把整个仓库上传到 Netlify、Cloudflare Pages、Vercel Static、对象存储静态网站或任何普通 Web 服务器。

## 说明

这是浏览器本地运行的离线代币演示系统，不会广播真实 Bitcoin 主网交易，也不会替代真实钱包。余额、私钥和已使用 nonce 保存在当前浏览器的 `localStorage` 中。二维码只承载经过发起方 ECDSA 签名的离线转账凭证。

本项目不使用摄像头，只通过二维码图片完成扫描和转账，因此直接用 `file://` 打开也可以使用。

## 第三方脚本

`vendor/qrcode.min.js` 来自 qrcodejs，`vendor/jsQR.js` 来自 jsQR。两者都随仓库一起部署，因此页面不依赖外部 CDN 才能生成和识别二维码。
