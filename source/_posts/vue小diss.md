---
title:  computed用法,vuex简单使用
date:  2019-05-30 15：30
tags:
---

### vue

- computed用法之一
        template里面绑定{{text}}
         computed: {
            text() {
              return this.type == 0 ? `下单成功，请拨打电话${this.mobile}` : '交付成功！'
            }
          }
          
- vuex简单使用

    store文件下-
    
    1.action.js -- 操作行为处理模块,由commit()来触发mutation的调用 , 间接更新 state
                
            const actions = {
              token({commit},value) {
                commit("TOKEN", value)
              }
            }
            export default actions
                
    2.mutations.js -- commit：状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法。
    
        const mutations = {
         TOKEN(state, value){  //存储token
            state.token = value
            localStorage.setItem('token',value)
          },
          }
        export default mutations
        
    3.state.js -- 集中存储Vue components中data对象的零散数据,页面显示所需的数据从该对象中进行读取
    
         const state = {
              token: localStorage.getItem('token') ? localStorage.getItem('token') : '',
            }
          export default state
        
    4.getters.js -- state对象读取方法。类似于计算属性 会随着state里面的数据变化而变化
    
       export const token = state => state.token

    5.store.js
    
        import Vue from 'vue'
        import Vuex from 'vuex'
        import state from './state'
        import mutations from './mutations'
        import actions from './actions'
        import * as getters from './getters'
        
        Vue.use(Vuex)
        export default new Vuex.Store({
          state,
          mutations,
          actions,
          getters
        })

- 页面引入  

        import { mapMutations } from "vuex";
         methods: {
            ...mapMutations(["CHOOSE_ADDRESS"]),
         add(){
           this.CHOOSE_ADDRESS(item);
          }
         }

### vuex 第二种使用

- 比如后台管理系统 切换主题色

1. store文件下 新建modules文件夹 和 index.js

    - index.js中
      import Vue from 'vue'
      import Vuex from 'vuex'
      import theme from './modules/theme'
      Vue.use(Vuex)
      export default new Vuex.Store({
        modules: {
          theme
        }
      })

2. modules>theme.js文件中 

      //1.存储组件中data对象的数据 页面需要调用的时候 从该对象中读取
        const state = { 
        themeColor: '409eff',
        themeData: [{
          id: 0,
          value: '409eff',
          text: '默认'
        }, {
          id: 1,
          value: 'fc7167',
          text: '珊瑚红'
        }]
      }
    //2. state对象的读取方法
    const getters = {
      themeColor (state) {
        if (getTheme()) {
          state.themeColor = getTheme()
        }
        return state.themeColor
      }
    }
    //简写 const getters = { themeColor = state => state.themeColor}
    //3. mutations 具体业务逻辑 由actions中的commit触发调用
    const mutations = {
      THEME_COLOR(state,value){
        state.themeColor = value
      }
    }
    //4.actions  所有的方法 的commit触发mutations的调用 ,间接更新state
        const actions = {
          theme_color({commit},value){
            commit("CHOOSE_ADDRESS", value)
          }
        }

        export default {
        state,
        getters,
        actions,
        mutations
      }
### get post 封装

1. 引入
        
        import axios from 'axios'
        import qs from 'qs'
        const http = {
          get(url, params) {
            return axios({
              method: 'get',
              url,
              params,
              headers: {
                token: getStore('token') || '',
                worktoken: getStore('worktoken') || ''
              }
            })
              .then(res => {
                if (res.data.code == 1) {
                  return res.data
                } else if (res.data.code == 4007) {
                  clearStore(res.data.msg, '/')
                } else {
                  vue.$toast(res.data.msg)
                  return res.data
                }
              })
              .catch(err => {
                clearStore(err.msg, '/')
              })
          },
          post(url, data, especial = false) {
            if (url !== '/api/common/upload') {
              data = qs.stringify(data)
            }
            return axios({
              method: 'post',
              url,
              data,
              headers: {
                token: getStore('token') || '',
                worktoken: getStore('worktoken') || ''
              }
            })
              .then(res => {
                if (especial) {
                  return res.data
                } else {
                  if (res.data.code == 1) {
                    return res.data
                  } else if (res.data.code == 4007) {
                    // 没有权限
                    clearStore(res.data.msg, '/')
                  } else {
                    vue.$toast(res.data.msg)
                    return res.data
                  }
                }
              })
              .catch(err => {
                console.log('err', err)
                clearStore(err.msg, '/')
              })
          }
        }
        export default http
 
2.  之前的做法是 main.js里面 引入http.js 并全局注册Vue.use(‘http’)
    http.js 里面是请求头 请求拦截 axios的封装get post请求
    api.js   放所有的接口请求      

    没必要全局注册 在单个页面去引入就可以了 所有api.js中去引入http.js 

- computed用法之一

         computed: {
            text() {
              return this.type == 0 ? `下单成功，请拨打电话${this.mobile}` : '交付成功！'
            }
          }
          
- vuex简单使用

    store文件下-
    
    1.action.js -- 操作行为处理模块,由commit()来触发mutation的调用 , 间接更新 state
                
            const actions = {
              token({commit},value) {
                commit("TOKEN", value)
              }
            }
            export default actions
                
    2.mutations.js -- commit：状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法。
    
        const mutations = {
         TOKEN(state, value){  //存储token
            state.token = value
            localStorage.setItem('token',value)
          },
          }
        export default mutations
        
    3.state.js -- 集中存储Vue components中data对象的零散数据,页面显示所需的数据从该对象中进行读取
    
         const state = {
              token: localStorage.getItem('token') ? localStorage.getItem('token') : '',
            }
          export default state
        
    4.getters.js -- state对象读取方法。
    
       export const token = state => state.token

    5.store.js
    
        import Vue from 'vue'
        import Vuex from 'vuex'
        import state from './state'
        import mutations from './mutations'
        import actions from './actions'
        import * as getters from './getters'
        
        Vue.use(Vuex)
        export default new Vuex.Store({
          state,
          mutations,
          actions,
          getters
        })

- 页面引入  

        import { mapMutations } from "vuex";
         methods: {
            ...mapMutations(["TOKEN"]),
         add(){
           this.CHOOSE_ADDRESS(item);
          }
         }

