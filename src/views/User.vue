<template>
  <div>
    <div>用户信息保存</div>
    <button @click="asc">升序</button>
    <button @click="desc">降序</button>
    <button @click="reset">重置</button>
    <ul>
      <li v-for="(item, index) in userList" :key="index">
        {{item.name}}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      userList: []
    }
  },
  async created () {
    // 解构赋值
    const { data } = await axios.get('http://jsonplaceholder.typicode.com/users')
    // 代理
    this.proxy = new Proxy({}, {
      get (target, key) {
        if (key === 'asc') { // 升序
          return [].concat(data).sort((a, b) => a.name > b.name ? 1 : -1)
        } else if (key === 'desc') { // 降序
          // data.sort会改变原数组,不能保留原数组
          // return data.sort((a, b) => b.name > a.name ? 1 : -1)
          return [].concat(data).sort((a, b) => b.name > a.name ? 1 : -1)
        } else {
          return data
        }
      },
      set () {
        return false
      }
    })
    // console.log(data)
    this.userList = this.proxy.default
  },
  // created () {
  //   axios.get('http://jsonplaceholder.typicode.com/users')
  //     .then(res => {
  //       console.log(res)
  //       this.userList = res.data
  //     }).catch(err => {
  //       console.log(err)
  //     })
  // }
  methods: {
    asc () {
      this.userList = this.proxy.asc
    },
    desc () {
      this.userList = this.proxy.desc
    },
    reset () {
      this.userList = this.proxy.default
    }
  }
}
</script>

<style>
ul {
  padding: 0;
}
li {
  list-style: none;
}
</style>
