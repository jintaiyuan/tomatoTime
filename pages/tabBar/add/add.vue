<template>
	<view class="uni-container">
		<input type="text" v-model="works"  class="input-work" placeholder="请输入事项"/>
		<view class="level-wrap">
			<view class="level" v-for="(item,index) in 4" 
			:key="index" 
			:style="levelColor[index]"
			:class="level==index?'active':''"
			@click="chooseLevel(index)">
				
			</view>
		</view>
		<view class="add input-work" @click="addWorks">添加</view>
		
		<view class="description">
			<p>从左到右重要等级依次降低</p>
			<p>列表中会将等级高的显示在前面</p>
		</view>
	</view>
	
</template>
<script>
	export default {
		data() {
			return {
				works:'',
				levelColor:[
						{boxShadow:'0 0 10rpx red'},
						{boxShadow:'0 0 10rpx orange'},
						{boxShadow:'0 0 10rpx yellow'},
						{boxShadow:'0 0 10rpx blue'}
				],
				level:0
			}
		},
		
		methods: {
			addWorks(){
				let workObj={
					name:this.works,
					state:false,
					level:this.level
				}
				getApp().globalData.todoList.push(workObj)
				this.works=''
				uni.showToast({
					title:'添加成功'
				})
				console.log('添加成功')
			},
			chooseLevel(index){
				this.level=index
				console.log(this.level)
			}
		}
	}
</script>

<style>
	.input-work {
		width:80%;
		height: 100rpx;
		margin: 50rpx auto 0 auto;
		border-radius: 10rpx;
		padding-left: 20rpx;
		border:1px solid #4CD964;
	}
	.add {
		line-height: 100rpx;
		padding-left: 0;
		text-align: center;
	}
	.level-wrap {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		width:80%;
		height: 60rpx;
		margin: 50rpx auto 0 auto;
	}
	.level {
		width: 16%;
		height: 100%;
		border-radius: 5rpx;
	}
	.active:before {
		content:'\2713';
		display: block;
		text-align:center;
		width: 100%;
		color:#4CD964;
	}
	.description {
		margin: 0 auto;
		margin-top: 100rpx;
		width: 80%;
	}
</style>
