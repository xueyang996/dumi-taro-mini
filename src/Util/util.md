## 常用工具类

说明：
openDocument：打开文件操作


```js
/* eslint-disable import/prefer-default-export */
import Taro from '@tarojs/taro'
const IMG_TYPE = /jpg|png|gif|jpeg|heic/
/**
 *
 * @param params 参数对象或者参数key的数组
 * @param paramsValue 参数对象
 */
export function openDocument (fileUrl: string, loadingText?: string) {
  if(!fileUrl) {
    return Taro.showToast({
      title: '暂无文件',
      icon: 'none',
      duration: 2000
    })
  }
  // 判定是否是图片, 图片单独预览
  const index = fileUrl.indexOf('?')
  const checkUrl = index>0? fileUrl.substring(0, index) : fileUrl
  const type = checkUrl.substring(checkUrl.lastIndexOf('.')+1).toLowerCase()
  if (IMG_TYPE.test(type)) {
    return Taro.previewImage({
      current: fileUrl,
      urls: [fileUrl]
    })
  }

  Taro.showLoading({
    title: (typeof loadingText ==='string' && loadingText) ? loadingText : '加载中'
  })
  Taro.downloadFile({
    url: fileUrl,
    success: function (res) {
      // 只要服务器有响应数据，就会把响应内容写入文件并进入 success 回调，业务需要自行判断是否下载到了想要的内容
      if (res.statusCode === 200) {
        const filePath = res.tempFilePath
        Taro.openDocument({
          filePath: filePath,
          success: function () {
            Taro.hideLoading()
          },
          fail: function () {
            Taro.hideLoading()
            Taro.showToast({
              title:'打开失败',
              icon: 'none',
              duration: 2000
            })
          },
        })
      }
    },
    fail: function () {
      Taro.hideLoading()
      Taro.showToast({
        title:'下载失败',
        icon: 'none',
        duration: 2000
      })
    },
    complete: function() {

    }
  })
}

```
