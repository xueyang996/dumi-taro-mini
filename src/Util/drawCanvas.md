### DrawCanvas

canvas绘图

```js

import Taro from '@tarojs/taro'

function saveImgToLocal(tempFilePath: string, callback) {
  Taro.saveImageToPhotosAlbum({
    filePath: tempFilePath,
    //保存成功失败之后，都要隐藏画板，否则影响界面显示。
    success: () => {
      callback && callback()
      Taro.showToast({
        title: '保存成功',
        icon: 'none',
        duration: 1500,
        mask: false,
        success: function () {
        }
      });
    },
    fail: () => {
      Taro.showToast({
        title: '保存失败',
        icon: 'none',
        duration: 1500,
        mask: false,
        success: function () {
        }
      });
    },
  })
}

/**
   * 加载图片 promise 化
   * @param canvas
   * @param imgsrc
   */
export function loadImage(canvas,imgsrc){
  return new Promise((resolve,reject)=>{
    const _img = canvas.createImage();
    _img.onload=()=>{
      resolve(_img);
    }
    _img.onerror=(err)=>{
      reject(err);
    }
    _img.src=imgsrc;
  });
}
/**
   * params
   *  @ctx    canvas context
   *  @color  border颜色
   *  @startX 开始位置x
   *  @startY 开始位置y
   *  @endX   结束位置x
   *  @endY   结束位置y
   *  @width  border宽度
   */
export function drawBorder({ ctx, startX, startY, endX, endY, color, width}) {
  // 绘制border
  // ctx.beginPath()
  ctx.moveTo(startX, startY)
  ctx.lineTo(endX, endY)
  ctx.strokeStyle=color
  ctx.lineWidth=width
  ctx.stroke()
}
/**
   * params
   *  @ctx   canvas context
   *  @color 字体颜色
   *  @text  文字内容
   *  @size  文字大小
   *  @x     文字位置x ###important 字体位置从字体的bottom开始计算
   *  @y     文字位置y
   *  @maxWidth 文字最大宽度
   */
export const drawText = ({ctx, color, text, weight=400, size, x=0, y=0, alignCenter=false}) => {
  if(alignCenter) {
    ctx.textAlign = 'center';
  } else {
    ctx.textAlign = 'start';
  }
  ctx.font= `${weight} ${size}px sans-serif`
  ctx.fillStyle=color
  ctx.fillText(text, x, y)
  console.log(text, x, y, size*2, color, 'hhh444hh#######')
}
export const drawRectWithRadius= ({ ctx, startX, startY, width, height, borderRadius}) => {
  if(typeof borderRadius !== 'object') {
    borderRadius=Array(4).fill(borderRadius)
  }
  ctx.beginPath()
  ctx.moveTo((startX + borderRadius[0]), startY)
  ctx.lineTo((startX+width-borderRadius[1]), startY)
  if(borderRadius[1]) {
    ctx.arc((startX +width - borderRadius[1]), (startY + borderRadius[1]), borderRadius[1], 1.5 * Math.PI, 2 * Math.PI)
  }
  ctx.lineTo((startX + width), (startY + height - borderRadius[2]))
  if(borderRadius[2]) {
    ctx.arc((startX + width - borderRadius[2]), (startY + height -borderRadius[2]), borderRadius[2], 0, 0.5 * Math.PI)
  }
  ctx.lineTo((startX + borderRadius[3]), (startY + height))
  if(borderRadius[3]) {
    ctx.arc((startX + borderRadius[3]), (startY + height - borderRadius[3]), borderRadius[3], 0.5*Math.PI, 1 * Math.PI)
  }
  ctx.lineTo((startX), (startY + borderRadius[0]))
  if(borderRadius[0]) {
    ctx.arc((startX + borderRadius[0]), (startY + borderRadius[0]), borderRadius[0], 1 * Math.PI, 1.5 * Math.PI)
  }
}

export const saveImg = (canvas, callback) => {
  Taro.showLoading({
    title: '生成图片中',
  })
  Taro.canvasToTempFilePath({
    x: 0,
    y: 0,
    canvas,
    canvasId: '',
    // width: screenWidth,
    // height: 2 * screenWidth,
    destWidth: canvas.width,
    destHeight: canvas.height,
    quality: 1,
    success: function (res) {
      saveImgToLocal(res.tempFilePath, callback)
      if (!res.tempFilePath) {
        Taro.showModal({
          title: '提示',
          content: '图片绘制中，请稍后重试',
          showCancel: false
        })
      }
      Taro.hideLoading()
    }
  })

  // 二维码页面埋点
  // ReportLogs({
  //   payload: {
  //     eventId: 'zg_jobcode_pic',
  //     moduleName: '智能招工',
  //     pageName: '二维码页面'
  //   }
  // })
}



```
