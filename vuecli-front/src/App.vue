<template>
	<div id="app">
		<el-menu :default-active="activeIndex2" class="el-menu-demo" mode="horizontal" @select="handleSelect"
		 background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
			<el-menu-item index="1">
				<router-link to="/">首页</router-link>
			</el-menu-item>
			<el-submenu index="2">
				<template slot="title"> <router-link to="/list">列表页111</router-link></template>
				<el-menu-item index="2-1">总裁办222</el-menu-item>
				<el-menu-item index="2-2">编辑部333</el-menu-item>
				<el-menu-item index="2-3">技术部444</el-menu-item>
			</el-submenu>
			<el-menu-item index="3">
				<router-link to="/about">详情页</router-link>
			</el-menu-item>
			<el-menu-item index="4"><a href="https://www.ele.me" target="_blank">
					<router-link to="/myself">个人信息</router-link>
				</a></el-menu-item>
		</el-menu>
		<router-view />
	</div>
</template>

<script>
	 export default {
	    data() {
	      return {
	        activeIndex2: '1'
	      };
	    },
	    methods: {
	      handleSelect(key, keyPath) {
	        console.log(key, keyPath);
	      }
	    },
		created(){
			if(this.$route.path=="/myself"){
				this.activeIndex2="4";
			}
		}
	  }
</script>


<style>
	* {
		padding: 0px;
		margin: 0px;
	}
</style>
