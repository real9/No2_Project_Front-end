<!--验证码组件-->
<template>
 <div class="l-canvas">
    <canvas id="l-canvas" :width="contentWidth" :height="contentHeight">
        浏览器不支持canvas!
    </canvas>
 </div>
</template>
<script>
export default {
//组件传值
 props: {
 identifyCode: {
  type: String,
  default: '1234'
 },
 //文字大小
 fontSizeMin: {
  type: Number,
  default: 25
 },
 fontSizeMax: {
  type: Number,
  default: 30
 },
 //绘制画布背景色选值范围（默认：白色）
 backgroundColorMin: {
  type: Number,
  default: 255
 },
 backgroundColorMax: {
  type: Number,
  default: 255
 },
 colorMin: {
  type: Number,
  default: 0
 },
 colorMax: {
  type: Number,
  default: 160
 },
 //干扰线颜色范围
 lineColorMin: {
  type: Number,
  default: 100
 },
 lineColorMax: {
  type: Number,
  default: 255
 },
 //干扰点颜色范围
 dotColorMin: {
  type: Number,
  default: 0
 },
 dotColorMax: {
  type: Number,
  default: 255
 },
 //上下文大小
 contentWidth: {
  type: Number,
  default: 112
 },
 contentHeight: {
  type: Number,
  default: 31
 }
 },
 methods: {
 randomNum: function(min, max) {// 生成一个随机数
  return Math.floor(Math.random() * (max - min) + min)
 },
 randomColor: function(min, max) {// 生成一个随机的颜色
  let r = this.randomNum(min, max)
  let g = this.randomNum(min, max)
  let b = this.randomNum(min, max)
  return 'rgb(' + r + ',' + g + ',' + b + ')'
 },
 drawPic: function(){//绘制画布
  let _this = this;
  let canvas = document.getElementById('l-canvas');
  if(canvas.getContext){//检查浏览器支持性
    let ilength = _this.identifyCode.length;
    let ctx = canvas.getContext('2d');
    ctx.textBaseline = 'bottom';
    //绘制背景
    ctx.fillStyle = _this.randomColor(_this.backgroundColorMin,_this.backgroundColorMax);
    ctx.fillRect(0,0,_this.contentWidth,_this.contentHeight);
    //逐步绘制文字
    for (let i = 0; i < ilength; i++){
        _this.drawText(ctx, _this.identifyCode[i], i);
    }
    //绘制干扰线和点
    _this.drawLine(ctx);
    _this.drawDot(ctx);
  }
 },
 drawText(ctx, txt, i) {
  ctx.fillStyle = this.randomColor(this.colorMin, this.colorMax)
  ctx.font = this.randomNum(this.fontSizeMin, this.fontSizeMax) + 'px SimHei'
  let x = (i + 1) * (this.contentWidth / (this.identifyCode.length + 1))
  let y = this.randomNum(this.fontSizeMax, this.contentHeight - 5)
  var deg = this.randomNum(-45, 45)
  // 修改坐标原点和旋转角度
  ctx.translate(x, y)
  ctx.rotate(deg * Math.PI / 180)
  ctx.fillText(txt, 0, 0)
  // 恢复坐标原点和旋转角度
  ctx.rotate(-deg * Math.PI / 180)
  ctx.translate(-x, -y)
 },
 drawLine(ctx) {// 绘制干扰线(4个)
  for (let i = 0; i < 5; i++) {
  ctx.strokeStyle = this.randomColor(this.lineColorMin, this.lineColorMax)
  ctx.beginPath()
  ctx.moveTo(this.randomNum(0, this.contentWidth), this.randomNum(0, this.contentHeight))
  ctx.lineTo(this.randomNum(0, this.contentWidth), this.randomNum(0, this.contentHeight))
  ctx.stroke()
  }
 },
 drawDot(ctx) {// 绘制干扰点(80个)
  for (let i = 0; i < 80; i++) {
  ctx.fillStyle = this.randomColor(this.dotColorMin, this.dotColorMax);
  ctx.beginPath();
  ctx.arc(this.randomNum(0, this.contentWidth), this.randomNum(0, this.contentHeight), 1, 0, 2 * Math.PI);
  ctx.fill();
  }
 }
 },
 watch: {//观察验证码变化
 identifyCode:function(){
  this.drawPic()
 }
 },
 mounted() {
 this.drawPic()
 }
}
</script>

<style scoped>
.l-canvas canvas{
  border: 1px solid #DCDFE6;
  border-radius: 4px;
}
</style>
