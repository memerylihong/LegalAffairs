<template>
<div class="add-contact" @click.self="$emit('SaveAdd')">
	<div class="add-contact-info">
		<div class="nav">
			<span></span>
			<p>添加联系人</p>
			<span @click="saveAdd">保存</span>
		</div>
		<div class="list">
			<div>
				<span>姓名</span>
				<input type="text" placeholder="请输入姓名" v-model="username">
			</div>
			<div>
				<span>职务</span>
				<input type="text" placeholder="请输入职务" v-model="userwork">
			</div>
			<div>
				<span>电话</span>
				<input type="text" placeholder="请输入电话" v-model="userphone">
			</div>
			<div>
				<span>邮箱</span>
				<input type="text" placeholder="请输入邮箱" v-model="useremail">
			</div>
		</div>
	</div>
	<error-remind  v-if="showRemind" @Close_errorMind="showRemind = false" :errorRemind="errorRemind"></error-remind>
</div>
</template>

<script>
import ErrorRemind from "base/ErrorRemind.vue";
export default {
	components:{
		ErrorRemind
	},
	props:['name','phone','email','work','clicentIndex'],
	data () {
		return {
			username:'',
			userwork:'',
			userphone:'',
			useremail:'',
			showRemind:false,
			errorRemind:''
		}
	},
	watch:{
		name:{
			handler (val) {
				if (val !== '') {
					this.username = val
				}
			},
			immediate:true
		},
		work: {
			handler (val) {
				if (val !== '') {
					this.userwork = val
				}
			},
			immediate:true
		},
		phone:{
			handler (val) {
				if (val !== '') {
					this.userphone = val
				}
			},
			immediate:true
		},
		email:{
			handler (val) {
				if (val !== '') {
					this.useremail = val
				}
			},
			immediate:true
		}
	},
	methods:{
		saveAdd () {
			let phoneReg = /^[1][1,2,3,4,5,6,7,8,9,0][0-9]{9}$/
			let pattern = /^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/;
			if (this.username === '') {
				this.showRemind = true
				this.errorRemind = '请输入姓名'
				setTimeout(()=>{
					this.showRemind = false
					this.errorRemind = ''
				},2000)
			} else if (this.userphone === '') {
				this.showRemind = true
				this.errorRemind = '请输入手机号'
				setTimeout(()=>{
					this.showRemind = false
					this.errorRemind = ''
				},2000)
			}	else if (!phoneReg.test(this.userphone)) {
				this.showRemind = true
				this.errorRemind = '手机号格式不正确'
				setTimeout(()=>{
					this.showRemind = false
					this.errorRemind = ''
				},2000)
			}	else if (this.useremail !== '') {
				if (!pattern.test(this.useremail)) {
					this.showRemind = true
					this.errorRemind = '邮箱格式不正确'
					setTimeout(()=>{
							this.showRemind = false
							this.errorRemind = ''
					},2000)
				} 
			} else {
				let newObj={}
				newObj['name'] = this.username
				newObj['work'] = this.userwork
				newObj['phone'] = this.userphone
				newObj['email'] = this.useremail
				newObj['clicentIndex'] = this.clicentIndex
				this.$emit('SaveAdd',newObj)
			}
		}
	}
}
</script>

<style lang="less" scoped>
@import '../assets/css/flex.less';
.add-contact {
	position: fixed;
  top: 0;
  left: 0;
	right: 0;
	bottom:0;
  .f-d-f;
	.f-ai-c;
	.f-jc-c;
	z-index: 1111;
	background-color: rgba(0, 0, 0, 0.2);
	&-info{
		background-color: #fff;
		border-radius: 5px;
		width:88%;
		.f-d-f;
		.f-fd-c;
		.nav {
			width:100%;
			height:40px;
			padding:0 20px;
			.f-d-f;
			.f-jc-sb;
			.f-ai-c;
			p {
				color:#333;
				
			}
			span {
				color:rgb(29, 29, 250);
				font-size:14px;
			}
		}
		.list {
			.f-d-f;
			.f-fd-c;
			div {
				height:42px;
				width:100%;
				.f-d-f;
				.f-jc-sb;
				.f-ai-c;
				padding:0 0px 0 8px;
				border-bottom:1px solid #e5e5e5;
				padding:0 20px;
				span {
					color:#666;
					font-size:14px;
				}
				input {
					text-align: right;
					font-size:14px;
					color:#333;
				}
			}
		}
	}
}
</style>
