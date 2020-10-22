<template>
	<view class="list">
		<view class="item" 
		v-for="(item,index) in todoList" 
		:key="index" 
		:style="levelColor[item.level]"
		@click="buttonShow(index)">
			<view class="move" :class="buttonState==index?'action':'noe'">
				<view class="text">{{item.name}}</view>
				<view class="changeState" 
				@click="changeState(item)">
						{{item.state?'撤回':'完成'}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		mounted() {
			this.updateList()
		},
		data(){
			return {
				todoList:[],
				levelColor:[
						{boxShadow:'0 0 10rpx red'},
						{boxShadow:'0 0 10rpx orange'},
						{boxShadow:'0 0 10rpx yellow'},
						{boxShadow:'0 0 10rpx blue'}
				],
				lastState:10001,
				buttonState:10000
			}
		},
		props:['showType','show'],
		
		methods:{
			buttonShow(index){
				if(this.lastState==index){
					this.buttonState=11111
					this.lastState=10001
					return
				}
				this.buttonState=index
				this.lastState=index
				console.log(arguments.callee())
			},
			changeState(item){
				let glo=getApp().globalData.todoList
				glo[glo.indexOf(item)].state=!glo[glo.indexOf(item)].state
				this.updateList()
			},
			updateList(){
				this.todoList=[]
					getApp().globalData.todoList.forEach((item)=>{
						if(item.state==this.$props.showType){
							this.todoList.push(item)
						}
					})
					this.todoList.sort(function(a,b){
						if(a.level<b.level){
							return -1
						}
					})
					this.todoList.forEach((item)=>{
						this.$set(item,'buttonState',false)
					})
			}
		},
		watch:{
			show(val){
				this.updateList()
			}
		}
	}
</script>

<style>
	
	.list {
		width: 100%;
	}
	.item {
		margin-top: 50rpx;
		border-radius: 10rpx;
		min-height: 100rpx;
		line-height: 100rpx;
		box-shadow:0 0 10rpx black;
		overflow: hidden;
	}
	.text{
		margin-top: -1px;
		display: inline-block;
		width: 100%;
		padding-left: 10rpx;
		vertical-align: middle;
		white-space: normal;
		box-shadow: 1px 0 0 0px #ded8d8;
	}
	.changeState {
		display: inline-block;
		width:25%;
		height: 100%;
		border-radius: 10rpx;
		transition: left .2s;
		overflow: hidden;
		white-space: nowrap;
		text-align: center;
		vertical-align: middle;
	}
	.move {
		transition: .2s;
		white-space: nowrap;
	}
	.action{
		transform: translateX(-25%);
		
	}
</style>
