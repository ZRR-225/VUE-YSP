<template>
  <div>
    <ul v-if="datalist.length" v-infinite-scroll="loadMore"
      infinite-scroll-disabled="loading"
      infinite-scroll-immediate-check= "false"
      infinite-scroll-distance="10">
      <li v-for="data in datalist" @click="handlieClick(data.id)" :key="data.id">
        <img :src="data.images[0]" alt="">
      <p>{{data.summary}}</p>
      <p>{{data.brand.name}}</p>
      <p>{{data.name}}</p>
      <p class="price">￥{{data.price}}. <span>0</span></p>
      </li>
    </ul>
    <p>{{msg}}</p>
  </div>
</template>

<script>
import axios from "axios"
export default {
  name: 'Cloth',
  data () {
    return {
      datalist:[],
      current:0,
      loading:false,
      total:0,
      msg:"加载中......"
    }
  },
  mounted(){
    axios.get("/app/search/product?limit=20&offset=0&categoryId=220,242,243,245,249&sortField=RELEASE&sortOrder=DESC").then(res=>{
      console.log(res.data);
      this.datalist = res.data.data.items;
      this.total = res.data.data.total;
      console.log(this.total);
    })
  },
  methods:{
    handlieClick(id){
      this.$router.push('/ClothDetail/' + id);
    },
    loadMore(){
      console.log('到底了');

      this.current = this.current + 20;

      if(this.current>this.total){
        this.loading = true;
        this.msg = "到底了~~";
        return
      }
      axios.get(`/app/search/product?limit=20&offset=${this.current}&categoryId=220,242,243,245,249&sortField=RELEASE&sortOrder=DESC`).then(res=>{
        this.datalist = [...this.datalist,...res.data.data.items]
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
ul{
  margin-top:50px;
  overflow:hidden;
  li{
    float: left;
    width: 170px;
    font-size: 16px;
    height: 275px;
    list-style: none;
    padding:15px;
    p{
      white-space:nowrap;
      overflow:hidden;
      text-overflow:ellipsis;
    }
    .price{
      color: #004c46;
    }
    span{
      font-size:12px;
    }
    img{
      width: 100%
    }
  }
}

</style>
