<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="KeyWords"
      />&nbsp;<button @click="searchUsers">Search</button>
    </div>
  </section>
</template>

<script>
import axios from 'axios'
export default {    
    name:'Search',
    data() {
        return {
            KeyWords:''
        }
    },
    methods: {
        searchUsers(){
            this.$bus.$emit('updateList',{users:[],isFirst:false,isLoading:true,errMsg:''})
            axios.get(`https://api.github.com/search/users?q=${this.KeyWords}`).then(response=>{
                this.$bus.$emit('updateList',response.data.items)
                this.$bus.$emit('updateList',{users:response.data.items,isLoading:false,errMsg:''})
            },
            error=>{
                console.log('请求失败');
                this.$bus.$emit('updateList',{users:[],isLoading:false,errMsg:error})
            })
        }
    },

}
</script>

<style>
</style>