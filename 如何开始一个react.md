#### 类组件

    import React, { Component } from "react";
    import { connect } from "react-redux";
    import {BrowserRouter as Router} from 'react-router-dom
    import { getA } from "./redux";
    import Home from "./Home"
    
    > react关联redux
    
    @connect((state) => ({ a: state.a }), { getA })
    class test extends Component {
    > 自定义状态
    	state = { b: 111 };
    > 修改状态
    	setB = () => {
    > 使用redux的状态
      this.setState({ b: this.props.a });
    > 生命周期钩子
    	componentDidMount(){
    > 调用redux方法
      	this.props.getA(this.state.b)
    }
    };
    render() {
    const c = true;
    const { b } = this.state;
    if (c) {
      return (<>
        <div>{b}</div>
    > 导航路由
        <Link to="/Home"></Link>
    > 注册路由
        <Route path="/Home" exact component={Home}></Route>
        </>)
    }
    return <div onClick={this.setB}>{b}</div>;
      }
    }
    
    > 在这里暴露
    export default test;

#### 函数组件

    import React, { useState, useEffect } from "react";
    import { connect } from "react-redux";
    import { BrowserRouter as Route, withRouter } from "react-router-dom";
    import { getB } from "./redux";
    import Home from "./Home";
    
    function test(props) {
    > hooks模拟状态
      const [c, setb] = useState(0);
    > 编程式路由
      const toHome = () => {
        props.history.push("/Home")
      };
    > hooks模拟生命周期
      useEffect(() => {
    > 设置模拟状态
        setb(111);
    > 调用action方法
        props.getB()
      }, []);
      return (
        <>
          <div onClick={toHome}>{props.a}</div>
          <Route path="/Home" component={Home}></Route>
        </>
      );
    }
    
    > 如果组件没有被路由包裹，withRouter可以获取location，history和match
    export default withRouter(connect((state) => ({ a: state.a }), { getB })(test));

#### 路由匹配

> 模糊匹配

    在这个状态下，Route的path会匹配所有以path开头的路由
    / 会成功匹配所有的路由

> 精确匹配

    在Route中加上exact，只会匹配完全一致的路由

> Switch 组件，有一个符合就不往下匹配了

#### redux

###### actions

    import { reqA } from "./api";
    import { GET_A } from "./constants";
    
    const getASync = (data) => ({ type: GET_A, data });
    
    //异步获取数据
    export const getA = (location) => {
      return (dispatch) => {
        return reqA(location).then((res) => {
          dispatch(getASync(res));
          //下面的return是将结果返回给调用的位置
          //获取结果即可得到，避免redux异步操作不能及时获取
          return res;
        });
      };
    };
    
    //同步修改reducer状态
    export function clearA(data) {
      return { type: "CLEAR_A", data };
    }

###### reducer

    import { GET_A, CLAER_DATA, GET_B } from "./constants";
    //从其他位置获取初始数据
    const b = JSON.parse(localStorage.getItem("B"));
    //定义默认数据
    const initState = { a: 1, b };
    
    export default function MyData(preState = initState, action) {
      switch (action.type) {
        case GET_A:
          return {
            ...preState,
            a: action.data,
          };
        case CLAER_DATA:
          return {
            ...preState,
            a: action.data,
            b: action.data,
          };
        case GET_B:
          return {
            ...preState,
            b: action.data,
          };
        default:
          return preState;
      }
    }

###### store

    //在redux分支中
    import {getA} from './actions'
    import MyData from './reducer'
    export{getA,MyData}
    
    //在主支reducer
    import {combineReducers} from 'redux
    import {MyData} from './my/redux'
    ...
    
    //合并分支reducer到主支
    export default combineReducers({
      MyData,
      ...
    })
    
    //store中使用中间件
    import { createStore, applyMiddleware } from 'redux'
    import thunk from 'redux-thunk'
    import { composeWithDevTools } from 'redux-devtools-extension'
    import reducer from './reducer'
    
    //根据当前环境决定中间件是否加上浏览器开发工具
    const middleware =
      process.env.NODE_ENV === 'development'
        ? composeWithDevTools(applyMiddleware(thunk))
        : applyMiddleware(thunk)
    
    export default createStore(reducer, middleware)
    
    //在react组件中使用redux
    import {Provider} from 'react-redux'
    import store from './redux/store
    
    return(
      <Provider store={store}>
        <App></App>
      </Proviser>
    )

#### 权限管理和懒加载

> 根据登录状态渲染公共页面/私密页面
> 公共页面路由从公共路由表渲染获得
> 私密页面路由从用户权限列表渲染获得
> 按钮路由从用户权限列表渲染活动
> 懒加载：将公共组件和私密组件抽到模块中，使用react的lazy函数包裹，集中暴露
> 使用时需要调用才能得到懒加载组件

#### 组件通信

> PubSubjs
> context
> 


