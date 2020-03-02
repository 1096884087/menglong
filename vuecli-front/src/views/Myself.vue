<template>
	<div class="about">
		<div class="infoform">
			<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
				<el-form-item label="头像" prop="myimg">
					<el-upload class="avatar-uploader" action="http://localhost:8888/imgUpload" :show-file-list="false"
					 :on-success="handleAvatarSuccess" :before-upload="beforeAvatarUpload">
						<img v-if="imageUrl" :src="imageUrl" class="avatar">
						<i v-else class="el-icon-plus avatar-uploader-icon"></i>
					</el-upload>
				</el-form-item>

				<el-form-item label="用户昵称" prop="username">
					<el-input v-model="ruleForm.username"></el-input>
				</el-form-item>
				<el-form-item label="手机号" prop="phNumber">
					<el-input v-model="ruleForm.phNumber"></el-input>
				</el-form-item>
				<el-form-item label="收货地址" prop="region">
					<el-select v-for="(arrItem,key) in selectList" :key="key" v-model="selectArr[key]" filterable placeholder="请选择"
					 value-key="value" @change="selected" @focus="position=key" style="margin:10px">
						<el-option v-for="item in arrItem" :key="item.value" :label="item.label" :value="item">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="活动时间" required>
					<el-col :span="11">
						<el-form-item prop="date1">
							<el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1" style="width: 100%;"></el-date-picker>
						</el-form-item>
					</el-col>
					<el-col class="line" :span="2">-</el-col>
					<el-col :span="11">
						<el-form-item prop="date2">
							<el-time-picker placeholder="选择时间" v-model="ruleForm.date2" style="width: 100%;"></el-time-picker>
						</el-form-item>
					</el-col>
				</el-form-item>

				<el-form-item label="个人描述" prop="desc">
					<el-input type="textarea" v-model="ruleForm.desc"></el-input>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
					<el-button @click="resetForm('ruleForm')">重置</el-button>
				</el-form-item>
			</el-form>
		</div>

	</div>
</template>

<script>
	
	export default {
		data() {
			var validatephNum = (rule, value, callback) => {
				if((/^\d{7,11}$/igm).test(value)){
					callback();
				}else {
					callback(new Error('请输入电话号，7~11位！'));
				}
			};
			return {
				imageUrl: '',
				position: null,
				selectArr: [],
				selectList: [
					[{
							value: '第1级-选项1',
							label: '第1级-选项1'
						},
						{
							value: '第1级-选项2',
							label: '第1级-选项2'
						},
						{
							value: '第1级-选项3',
							label: '第1级-选项3'
						},
						{
							value: '第1级-选项4',
							label: '第1级-选项4'
						},
						{
							value: '第1级-选项5',
							label: '第1级-选项5'
						}
					]
				],
				activeName: 'second',
				ruleForm: {
					username: '',
					region: '',
					date1: '',
					date2: '',
					desc: '',
					phNumber: "",
					myimg: ""
				},
				rules: {
					username: [{
							required: true,
							message: '请输入活动名称',
							trigger: 'blur'
						},
						{
							min: 3,
							max: 5,
							message: '长度在 3 到 5 个字符',
							trigger: 'blur'
						}
					],
					phNumber: [
						{
								required: true,
								message: '请输入电话号',
								trigger: 'blur'
						},
						{
							validator: validatephNum,
							trigger: 'blur'
						}
					],
					region: [{
						required: true,
						message: '请选择活动区域',
						trigger: 'change'
					}],
					date1: [{
						type: 'date',
						required: true,
						message: '请选择日期',
						trigger: 'change'
					}],
					date2: [{
						type: 'date',
						required: true,
						message: '请选择时间',
						trigger: 'change'
					}],
					desc: [{
						required: true,
						message: '请填写活动形式',
						trigger: 'blur'
					}]
				}
			};
		},
		methods: {
			handleAvatarSuccess(res, file) {
// 前端(el-upload)--后端返回数据--handleAvatarSuccess--imgUrl 
// imgUrl
				console.log(res, file)
				this.imageUrl =res.data[0]// URL.createObjectURL();
				
				
			},
			beforeAvatarUpload(file) {
				const isJPG = file.type === 'image/jpeg';
				const isLt2M = file.size / 1024 / 1024 < 2;

				if (!isJPG) {
					this.$message.error('上传头像图片只能是 JPG 格式!');
				}
				if (!isLt2M) {
					this.$message.error('上传头像图片大小不能超过 2MB!');
				}
				return isJPG && isLt2M;
			},
			//模拟获取下一级选项（实际项目可通过接口获取或者根据实际情况自行修改）
			addSelectList(index) {
				return [{
						value: '第' + (index + 2) + '级-选项1',
						label: '第' + (index + 2) + '级-选项1'
					},
					{
						value: '第' + (index + 2) + '级-选项2',
						label: '第' + (index + 2) + '级-选项2'
					},
					{
						value: '第' + (index + 2) + '级-选项3',
						label: '第' + (index + 2) + '级-选项3'
					},
					{
						value: '第' + (index + 2) + '级-选项4',
						label: '第' + (index + 2) + '级-选项4'
					},
					{
						value: '第' + (index + 2) + '级-选项5',
						label: '第' + (index + 2) + '级-选项5'
					}
				]
			},
			selected(item) {
				// console.log(item);  // item为当前选中项的对象
				var nextSelect = this.addSelectList(this.position); //模拟获取下一级选项
				this.selectList.splice(this.position + 1, this.selectList.length, nextSelect);
				this.selectArr.splice(this.position + 1, this.selectArr.length);
			},
			handleClick(tab, event) {
				console.log(tab, event);
			},
			submitForm(formName) {
				this.$refs[formName].validate((valid) => {
					if (valid) {
						this.ruleForm.myimg=this.imageUrl
						this.axios.post("/userinfo/edit",this.ruleForm)
						alert('submit!');
					} else {
						console.log('error submit!!');
						return false;
					}
				});
			},
			resetForm(formName) {
				this.$refs[formName].resetFields();
			}
		},
		created() {
			// axios.get("/userinfo/get",{
			// 	params:{
			// 		userId:localStorage.getItem("userid")
			// 	}
			// }).then(
			// 	(res)=>{
			// 		this.ruleForm.username=res.data.name;
			// 		this.ruleForm.phNumber=res.data.number;
			// 		this.imageUrl=res.data.imgurl;
			// 	}
			// )//需要传递当前登录用户的id
		}
	}
</script>

<style>
	.infoform {
		width: 40%;
		margin: 15px auto;
	}

	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}

	.avatar-uploader .el-upload:hover {
		border-color: #409EFF;
	}

	.avatar-uploader-icon {
		font-size: 28px;
		color: #8c939d;
		width: 178px;
		height: 178px;
		line-height: 178px;
		text-align: center;
	}

	.avatar {
		width: 178px;
		height: 178px;
		display: block;
	}
</style>
