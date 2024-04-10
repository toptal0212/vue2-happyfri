Project Overview
A very simple Vue2 + Vuex project, the entire process is clear at a glance. Though small, it encompasses all the essential components, suitable for beginners' practice.

If it helps you, you can click the "Star" button in the top right corner to support. Thank you! ^_^

Alternatively, you can "follow" me, and I will continuously open source more interesting projects.

If you have any questions, please directly raise them in Issues, or if you find issues and have very good solutions, feel free to submit a PR 👍

Development Environment: macOS 10.12.3, Chrome 56, Node.js 6.10.0

This project is mainly for beginners to practice Vue2 + Vuex. Additionally, I recommend a more complex large-scale Vue2 project that covers most of the Vue.js knowledge points. The project has been completed. The address is here

Project Setup (Node.js 6.0+)
bash
Copy code
# Clone the project locally
git clone https://github.com/bailicangdu/vue2-happyfri.git

# Enter the directory
cd vue2-happyfri

# Install dependencies
npm install or yarn (recommended)

# Launch the local server at localhost:8088
npm run dev

# Build for production
npm run build
Demo
demo link (Please preview using Chrome's mobile mode)

Scan the QR code below with your mobile device to view the demo:
<img src='https://github.com/bailicangdu/vue2-happyfri/blob/master/src/images/demo.png' width="200" height="200" />
Router Configuration
```
import App from '../App'

export default [{
    path: '/',
    component: App,
    children: [{
        path: '',
        component: r => require.ensure([], () => r(require('../page/home')), 'home')
    }, {
        path: '/item',
        component: r => require.ensure([], () => r(require('../page/item')), 'item')
    }, {
        path: '/score',
        component: r => require.ensure([], () => r(require('../page/score')), 'score')
    }]
}]
```
Configuration of Actions
```
import ajax from '../config/ajax'

export default {
    addNum({ commit, state }, id) {
        commit('REMEMBER_ANSWER', id);
        if (state.itemNum < state.itemDetail.length) {
            commit('ADD_ITEM_NUM', 1);
        }
    },
    initializeData({ commit }) {
        commit('INITIALIZE_DATA');
    }
}
```
Mutations
```
const ADD_ITEM_NUM = 'ADD_ITEM_NUM'
const REMEMBER_ANSWER = 'REMEMBER_ANSWER'
const REMEMBER_TIME = 'REMEMBER_TIME'
const INITIALIZE_DATA = 'INITIALIZE_DATA'
export default {
    [ADD_ITEM_NUM](state, payload) {
        state.itemNum += payload.num;
    },
    [REMEMBER_ANSWER](state, payload) {
        state.answerId[state.itemNum] = payload.id;
    },
    [REMEMBER_TIME](state) {
        state.timer = setInterval(() => {
            state.allTime++;
        }, 1000)
    },
    [INITIALIZE_DATA](state) {
        state.itemNum = 1;
        state.allTime = 0;
    },
}
```
Store Creation
```
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'
import actions from './action'

Vue.use(Vuex)

const state = {
    level: 'First Week',
    itemNum: 1,
    allTime: 0,
    timer: '',
    itemDetail: [],
    answerId: {}
}

export default new Vuex.Store({
    state,
    actions,
    mutations
})
```
Creating Vue Instance
```
import Vue from 'vue'
import VueRouter from 'vue-router'
import routes from './router/router'
import store from './store/'

Vue.use(VueRouter)
const router = new VueRouter({
    routes
})

new Vue({
    router,
    store,
}).$mount('#app')
```
