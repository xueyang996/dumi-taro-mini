## ListRender

说明：
小程序端-列表，下拉加载更多功能
通用listRender组件

效果图:

![avatar](./snapshotPage.png)


```js
import Taro, { Component } from '@tarojs/taro'
import { View } from '@tarojs/components'
import gotoPageGlobal from '@src/utils/gotoPageGlobal'
import Deadline from '@src/components/deadline'
import NoData from '@src/components/noData'
import fetch from '@src/utils/request'

import './index.less'

const PAGE_SIZE = 10
// #region 书写注意
//
// 目前 typescript 版本还无法在装饰器模式下将 Props 注入到 Taro.Component 中的 props 属性
// 需要显示声明 connect 的参数类型并通过 interface 的方式指定 Taro.Component 子类的 props
// 这样才能完成类型检查和 IDE 的自动提示
// 使用函数模式则无此限制
// ref: https://github.com/DefinitelyTyped/DefinitelyTyped/issues/20796
//
// #endregion

type PageStateProps = {
  fetchUrl?: string
  fetchMethod?: string
  pageSize?: number
  loadMore?: boolean
  onLoadOver?: (any) => void
  renderCat?: () => void
  nodataText?: string
}

type PageState = {
  listArray: [],
  total: number
}

// @connect<PageStateProps>(state => {
//   const { listArray, total } = state.account
//   return { listArray, total }
// }, {dispatchMatchedList})

class Index extends Component<PageStateProps, PageState> {
  constructor(props) {
    super(props);
    this.state = {
      listArray: [],
      total: 0
    };
  }
  componentDidMount() {
    this.getList()
  }

  componentWillReceiveProps (nextProps) {
    console.log(nextProps, '???????/')
    if(nextProps.loadMore) {
      this.getList()
    }
  }
    getData = ({size= PAGE_SIZE, page=1}) => {
      const { fetchUrl='', fetchMethod='GET', onLoadOver=(()=>{})} = this.props
      const url = fetchUrl.replace('{size}', size+'').replace('{page}', page+'')
      fetch({ url, method: fetchMethod }).then((res) => {
        if(res) {
          const {records, total} = res
          const { listArray } = this.state
          const newArray = page === 1 ? records : [...listArray, ...records]
          onLoadOver({total, currentArray: newArray})
          console.log(newArray)
          this.setState({
            listArray: newArray,
            total
          })
        }
      })
    }
    getList = () => {
      const { listArray = []} = this.state
      const { pageSize= PAGE_SIZE } = this.props
      const params = {
        size: pageSize,
        page: Math.floor(listArray.length/pageSize) + 1
      }
      this.getData(params)
    }
    gotoPage = (params) => {
      gotoPageGlobal(params)
    }

    render () {
      const { listArray, total } = this.state
      const { nodataText='暂无匹配数据', children } = this.props
      // let result
      // if(renderCat) {
      //   result = renderCat({listArray, onGotoPage: this.gotoPage})
      // }
      // console.log(listArray, '$$$$$$$$$4', result, renderCat, this.props)
      return (
        <View className='container'>
          {/* matched start */}
          {
            children
          }
          {/* matched end */}
          {
            total > 0 && listArray.length === total &&
              <Deadline></Deadline>
          }
          {
            total <= 0 &&
            <NoData nodataText={nodataText}></NoData>
          }
        </View>
      )
    }
}

export default Index


```
