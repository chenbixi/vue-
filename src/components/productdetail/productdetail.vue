<template>
	<transition name="food-detail">
	<div class="food" v-show="showFlag" ref="foodView">
		<div class="food-wrapper">
			<!--内容部分-->
			<div class="food-content">
				<div class="img-wrapper">
					<!--一个大图,从数据中拿-->
					<img class="food-img" :src="food.picture"/>
					<!--三个小图-->
					<!--关闭-->
					<span class="fa fa-close close-bt" @click="closeView"></span>
					<!--分享-->
					<img class="share-bt"src="./img/share.png"/> 
					<!--更多-->
				    <img class="more-bt"src="./img/more.png"/> 
				</div>
				
				<!--文字部分-->
				<div class="content-wrapper">
					<!--名字-->
					<h3 class="name">{{food.name}}</h3>
					<!--销售了多少-->
					<p class="saled">{{food.month_saled_content}}</p>
					<!--图片-->
					<img class="product" v-show="food.product_label_picture" :src="food.product_label_picture"/>
					<p class="price">
						<!--多少钱-->
						<span class="text">{{food.min_price}}</span>
						<!--单位-->
						<span class="unit">{{food.unit}}</span>
					</p>
					<!--选规格,如果已经点了选规格变成加减号,所以加入加减号组件-->
					<div class="cartcontrol-wrapper">
						<app-cartcontrol :food="food"></app-cartcontrol>
					</div>
					<!--如果没有点选规格则是黄色选规格文字-->
					<div class="buy" @click="addProduct" v-show="!food.count||food.count==0">
						选规格
					</div>			
				</div>
			</div>
			<app-split></app-split>
			
			<!--外卖评价-->
			<div class="rating-wrapper">
				<!--评价头部-->
				<div class="rating-title">
					<div class="like-ratio" v-if="food.rating">
						<span class="title">{{food.rating.title}}</span>
						<span class="retio">
							(
							{{food.rating.like_ratio_desc}}
							<i>{{food.rating.like_ratio}}</i>
							)
						</span>
					</div>
					<div class="snd-title" v-if="food.rating">
						<span class="text">{{food.rating.snd_title}}</span>
						<span class="fa fa-angle-right"></span>
					</div>
				</div>
			
				<ul class="rating-content" v-if="food.rating">
					<li v-for="(comment,index) in food.rating.comment_list" 
						:key="index"
						class="comment-item">
						<!--评论列表结构-->
						<div class="comment-header">
							<img :src="comment.user_icon" v-if="comment.user_icon" />
							<img src="./img/anonymity.png" v-if="!comment.user_icon" />
						</div>
						<div class="comment-main">
							<div class="user">	
								{{comment.user_name}}
							</div>
							<div class="time">	
								{{comment.comment_time}}
							</div>
							<div class="content">
								{{comment.comment_content}}
							</div>
						</div>												
					</li>
				</ul>
			</div>
		</div>
		
	</div>
	</transition>
</template>

<script>
	import Vue from 'vue'
	import BScroll from 'better-scroll'
	import CartControl from '../cartcontrol/cartcontrol'
	import Split from "../split/split"
	export default {
		components:{
			"app-cartcontrol": CartControl,
			"app-split":Split
		},
		data() {
			return {
				showFlag:false
			};
		},
		props:{
			food:{
				type:Object
			}
			
		},
		methods:{
			showView(){
				this.showFlag=true
				this.$nextTick(()=>{
					if(!this.scroll){
						this.scroll=new BScroll(this.$refs.foodView,{
							click:true
						});
					}else{
						this.scroll.refresh()
					}
				})
			},
			addProduct(){
				Vue.set(this.food,'count',1);
			},
			closeView(){
				this.showFlag=false
			},
		}
		
		
	}
</script>

<style scoped>
.food{
	position: fixed;
	left: 0;
	top: 0;
	bottom: 51px;
	background: white;
	width: 100%;
	z-index: 90;
}
/*动画样式*/
.food-detail-enter-active, .food-detail-leave-active {
  transition:  1.0s
}
.food-detail-enter, .food-detail-leave-to {
  transform: translateX(100%);
}
/* 图片容器 */
.food .food-wrapper .food-content .img-wrapper{
	position: relative;
	width: 100%;
	height: 0;
	padding-top: 100%;
}

.food .food-wrapper .food-content .img-wrapper .food-img{
	position: absolute;
	left: 0;
	bottom: 0;
	width: 100%;
	height: 100%;
}

.food .food-wrapper .food-content .img-wrapper .food-img{
	position: absolute;
	left: 0;
	bottom: 0;
	width: 100%;
	height: 100%;
}
.food .food-wrapper .food-content .img-wrapper .close-bt{
	width: 20px;
	height: 20px;
	position: absolute;
	left: 10px;
	top: 10px;
	text-align: center;
	font-size: 14px;
	line-height: 20px;
	color: white;
	background: #7f7f7f;
	border-radius: 50%;
}
.food .food-wrapper .food-content .img-wrapper .share-bt,
.food .food-wrapper .food-content .img-wrapper .more-bt
{
	width: 30px;
	height: 30px;
	position: absolute;
	top: 10px;
}
.food .food-wrapper .food-content .img-wrapper .share-bt{
	right: 50px;
}
.food .food-wrapper .food-content .img-wrapper .more-bt{
	right: 10px;
}
/*下面部分样式价格月售等*/
.food .content-wrapper{
	padding: 0 0 16px 16px;
	position: relative;
}
.food .content-wrapper .name{
	font-size: 15px;
	color: #333333;
	line-height: 30px;
	font-weight: bold;
}
.food .content-wrapper .saled{
	font-size: 11px;
	color: #9d9d9d;
	margin-bottom: 6px;
}
.food .content-wrapper .product{
	height: 15px;
	margin-bottom: 16px;
}
.food .content-wrapper .price{
	font-size: 0;
}
.food .content-wrapper .price .text{
	font-size: 17px;
	color: #FB4E44;
}
.food .content-wrapper .price .unit{
	font-size: 11px;
	color: #9D9D9D;
}
.food .cartcontrol-wrapper{
	 position: absolute;
	right: 12px;
	bottom: 10px;
	padding: 2px; 
	width: 50px;
}
/*选规格样式*/
.food .buy{
	width: 64px;
	height: 30px;
	font-size: 12px;
	line-height: 30px;
	text-align: center;
	background: #FFD161;
	border-radius: 30px;
	position: absolute;
	right: 12px;
	bottom: 10px;
}
/*具体评价部分样式*/
.food .food-wrapper .rating-wrapper{
	padding-left: 16px;
}
.food .food-wrapper .rating-wrapper .rating-title{
	padding: 16px 16px 16px 0;
}
.food .food-wrapper .rating-wrapper .rating-title .like-ratio{
	float: left;
	font-size: 0;
}
.food .food-wrapper .rating-wrapper .rating-title .like-ratio .title{
	font-size: 13px;
}
.food .food-wrapper .rating-wrapper .rating-title .like-ratio .ratio{
	font-size: 11px;
}
.food .food-wrapper .rating-wrapper .rating-title .like-ratio .ratio i{
	color: #FB4E44;
	font-size: 11px;
}

.food .food-wrapper .rating-wrapper .rating-title .snd-title{
	float: right;
	font-size: 0;
}
.food .food-wrapper .rating-wrapper .rating-title .snd-title .text,
.food .food-wrapper .rating-wrapper .rating-title .snd-title .icon
{
	font-size: 13px;
	color: #9D9D9D;
	display: inline-block;
}
.food .food-wrapper .rating-wrapper .rating-title .snd-title .icon{
	margin-left: 12px;
}


.food .food-wrapper .rating-wrapper .comment-item{
	padding: 15px 14px 17px 0;
	border-bottom: 1px solid #F4F4F4;
	width: 100%;
	box-sizing: border-box;
	display: flex;
}
.food .food-wrapper .rating-wrapper .comment-item .comment-header{
	flex: 0 0 41px;
	margin-right: 10px;
}
.food .food-wrapper .rating-wrapper .comment-item .comment-header img{
	width: 41px;
	height: 41px;
	border-radius: 50%;
}

.food .food-wrapper .rating-wrapper .comment-item .comment-main{
	flex: 1;
	margin-top: 6px;
}
.food .food-wrapper .rating-wrapper .comment-item .comment-main .user{
	width: 50%;
	float: left;
	font-size: 12px;
	color: #333333;
}
.food .food-wrapper .rating-wrapper .comment-item .comment-main .time{
	width: 50%;
	float: right;
	text-align: right;
	font-size: 10px;
	color: #666666;
}
.food .food-wrapper .rating-wrapper .comment-item .comment-main .content{
	margin-top: 17px;
	font-size: 13px;
	line-height: 19px;
}
</style>
<style src="../../common/css/font-awesome.css"></style>
