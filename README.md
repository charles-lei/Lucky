# 基于EOS区块链BlockHash的抽奖程序


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```
## 说明
此程序是基于EOS区块链的blockhash的抽奖程序，可以获取最新block_header_hash值，并根据当前hash值产生随机数。

## 使用

电脑需要连网，用浏览器打开dist/index.html即可。

## 关于配置总人数:

用文本编辑器打开dist/index.html，找到window.total = 82, 更改82为总人数即可。