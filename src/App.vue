<template>
  <div class="generate-poster">
    <div id="qrcode" style="display:none"></div>
    <button @click="handleImgDownload">下载图片</button>
    <canvas width="500" height="300" id="canvas"></canvas>
  </div>
</template>

<script setup>
import { onMounted } from '@vue/runtime-core'
// 插件对比
import QRCode from 'qrcodejs2'
console.log(QRCode)
const img2 = new Image()
onMounted(() => {
  const qr = document.querySelector("#qrcode")                  //放二维码的盒子
  const canvas = document.querySelector("#canvas")              //画布
  const ctx = canvas.getContext("2d")                           //取得2d上下文开始使用
  const img1 = new Image()
  //vue下动态创建img ，src不起效的问题
  img1.src = require("./assets/eee.jpeg")
  img1.onload = () => {
    ctx.drawImage(img1, 0, 0, canvas.width, canvas.height)      //绘制到canvas上
    const qrcode = new QRCode(qr, {                             //在qr的位置生成二维码
      width: 100,
      height: 100,
      colorDark: '#000',
      colorLight: '#fff'
    })
    console.log(qrcode)
    qrcode.makeCode("https://zhuanlan.zhihu.com/p/509441771")   //扫描二维码跳转的位置
    let base64 = qr.querySelector('canvas').toDataURL("image/png")  //将二维码转base64格式
    img2.src = base64
    img2.onload = () => {
      ctx.drawImage(img2, 400, 0, 100, 100)                   //在背景canvas上绘制二维码
    }
  }

})
// 下载二维码
const handleImgDownload = () => {
  const a = document.createElement('a')
  a.href = img2.src
  a.setAttribute("download", "qrcode");
  a.click()
}
</script>
<script>

export default {
  name: 'GeneratePoster',

}
</script>

<style>
</style>
