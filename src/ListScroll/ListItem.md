## ListItem

说明：
小程序端-列表，下拉加载更多功能
具体每个item渲染内容

效果图:

![avatar](./snapshotPage.png)


```js
import { FunctionComponent } from "react";
import Taro from '@tarojs/taro'
import { View } from "@tarojs/components";
import { formatTimeZone } from '@src/utils/time.js'
import IconFont from '@src/components/iconfont'

import './index.less';

type item= {
  age: string,
  deliverSource: number,
  deliveryTime: string,
  hireCount: string,
  name: string,
  positionSubType: string,
  resumeStatus: number,
  salaryRange: string,
  workExperience: string
  positionId: string
  positionName: string
  companyName: string
}

export interface ResumeListI {
  list: item[],
  onGotoPage?: (any) => void
}

const ResumeStatusEnum = {
  0: '未查看',
  1: '已查看',
  2: '不合适',
  3: '已获取联系方式',
}

const ResumeList: FunctionComponent<ResumeListI> = (props: ResumeListI) => {
  const { list = [], onGotoPage } = props
  function goDetail (params) {
    onGotoPage && onGotoPage(params)
  }
  const result = list.map(item => (
    <View className='resume-item' key={item.positionId} onClick={() => goDetail(item.positionId)}>
      <View className='part'>
        <View className='con'>
          <View className='name'>{item.positionName}</View>
          <View className='job-detail'>
            <View className='mr-7'>职位详情</View>
            <IconFont name='xiangyou' color='#aaa' size={24} />
          </View>
        </View>
      </View>
      <View className='com-name mt-6'>{item.companyName}</View>
      <View className='part2'>
        <View className='item'>{item.workExperience}</View>
        <View className='border'></View>
        <View className='item'>{item.hireCount}</View>
        <View className='border'></View>
        <View className='money'>{item.salaryRange}/月</View>
      </View>
      <View className='part mt-10'>
        <View className='con'>
          <View className='flex'>
            <View className='time com-name'>{formatTimeZone(item.deliveryTime, 'YYYY/MM/DD')}</View>
            <View className={`com-name ${item.deliverSource ? 'active': ''}`}>{item.deliverSource === 0 ? '主动投递' : '智能匹配'}</View>
          </View>
          <View className={`status status${item.resumeStatus}`}>{ResumeStatusEnum[item.resumeStatus]}</View>
        </View>
      </View>
    </View>
  ))
  return (
    <View className='history-container'>
      {result}
    </View>
  )
}
ResumeList.options = {
  addGlobalClass: true
}

export default ResumeList

```
