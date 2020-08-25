
## ConditionPicker

效果图:

![avatar](./snapshot.png)

```js
import { FunctionComponent } from "react";
import Taro from '@tarojs/taro'
import { View, Picker } from "@tarojs/components";

import IconFont from '@src/components/iconfont'

import './index.less';

type Condition = {
  type: string
 range: any[]
  value: number
  rangeKey: string
  slectedFlag: boolean,
  showText: string
}
export interface ConditionPickerI {
  conditions: Condition[]
  onChange: (string, event) => void
}

const ConditionPicker: FunctionComponent<ConditionPickerI> = (props: ConditionPickerI) => {
  const { conditions=[], onChange } = props
  function changePicker(params, e) {
    onChange(params, e)
  }
  return (
    <View className='policy-type-list'>
      {
        conditions.map(item => {
          return <View className='policy-item' key={item.type}>
            <Picker mode='selector' range={item.range} value={item.value} rangeKey={item.rangeKey}
              onChange={changePicker.bind(null, item.type)}
            >
              <View className={`picker ${item.slectedFlag ? 'selected' : ''}`}>
                <View className='picker-text ellipse'>
                  {item.showText}
                </View>
                <View className='rotate-180 ml-5'>
                  <IconFont name='xiangshang' size={18} color={item.slectedFlag ? '#4D75FF': '#3B416B'} />
                </View>
              </View>
            </Picker>
          </View>
        })
      }
    </View>
  )
}

export default ConditionPicker
```

less样式
```less
@import '../../style/defaultVar.less';

/**
* policy-type start
*/
.policy-type-list {
  display: flex;
  justify-content: space-between;
  .policy-item + .policy-item {
    margin-left: 20px*@hd;
  }
  .policy-item {
    width: 100px*@hd;
  }
  .rotate-180 {
    transform: rotate(180deg);
  }
  .picker-text {
    width: 70px*@hd;
    text-align: center;
  }
  .picker {
    display: flex;
    align-items: center;
    justify-content: center;
    box-sizing: border-box;
    height:30px*@hd;
    line-height:30px*@hd;
    background:rgba(246,246,246,1);
    border-radius:15px*@hd;
    padding: 0 10px*@hd;
    font-size:14px*@hd;
    font-weight:400;
    color:rgba(59,65,107,1);
    // border: 1px*@hd solid rgba(246,246,246,1);
    transition: all 300ms ease-in;
    // width: 90px*@hd;
    &.selected {
      color: #4D75FF;
      background:rgba(77,117,255,0.15);
      // border:1px*@hd solid rgba(77,117,255,1);
    }
  }
}
/**
* policy-type end
*/
```

More skills for writing demo: https://d.umijs.org/guide/demo-principle
