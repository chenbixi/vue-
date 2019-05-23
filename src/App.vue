<template>
  <div id="app">
    <!-- header -->
		<app-header :poiInfo="poiInfo"></app-header>
		<!-- nav -->
		<app-nav :commentNum="commentNum"></app-nav>
		<!-- content -->
		<router-view> </router-view>
    
  </div>
</template>

<script>
import Header from './components/header/header'
import Nav from './components/nav/nav'
import axios from 'axios'
export default {
  name: 'App',
	components:{
		"app-header":Header,
		"app-nav":Nav
	},
	data(){
		return{
			poiInfo:{},
			commentNum:0
		}
	},
	created(){
		axios.get('./api/goods').then((res)=>{
			// console.log(res.data.data.poi_info)
			this.poiInfo = res.data.data.poi_info;
			// console.log(this.poiInfo);
		}),
		axios.get('./api/ratings').then((res)=>{
			this.commentNum = res.data.data.comment_num;
			console.log(this.commentNum);
		})
	}
	
}
</script>

<style>

</style>
<style src="./common/css/font-awesome.css"></style>
