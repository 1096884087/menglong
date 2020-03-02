<template>
	<div class="home">
		<!-- <img alt="Vue logo" src="../assets/logo.png"> -->
		
		<el-carousel indicator-position="outside" @change="handleChange" height="420px">
			<el-carousel-item v-for="(item,i) in arr" :key="i" >
				<!-- <h3>{{ item }}</h3> -->
				<img :src="item" alt="" class="mycarousel">
			</el-carousel-item>
		</el-carousel>
	</div>
</template>

<script>
	// @ is an alias to /src
	export default {
		name: 'Home',
		data:function(){
			return {
				arr:["./img/1.webp","./img/2.webp","./img/3.webp"]
			}
		},
		// 富文本编辑框/echarts/d3/……
		methods:{
			handleChange(i){
				console.log(i);
				
				if(i==2){
					this.$message('这是一条消息提示');
				}
				
				
			}
		}
	}
</script>

<style>
	.el-carousel__item h3 {
		color: #475669;
		font-size: 18px;
		opacity: 0.75;
		line-height: 300px;
		margin: 0;
	}

	.el-carousel__item:nth-child(2n) {
		background-color: #99a9bf;
	}

	.el-carousel__item:nth-child(2n+1) {
		background-color: #d3dce6;
	}
	
	.mycarousel {
		height: 420px;
	}
</style>
