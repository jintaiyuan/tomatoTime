<template>
	<view class="wrap">
		<view class="container" @click="start">
			<p>点击开始or结束一个番茄钟</p>
			<p>{{tomatoState}}</p>
			<p>{{time[0]}}分{{time[1]}}秒</p>
		</view>
		
		
		<view class="list-wrap">
			<show-list showType=0 :show='show'></show-list>
		</view>
		
	</view>
</template>

<script>
	import showList from '../../components/showList.vue'
	export default {
		onLoad() {
			this.manager=this.tomatoPlus()
		},
		onShow() {
			this.show=!this.show
		},
		data() {
			return {
				tomatoState:'',
				time:[],
				manager:{},
				state:0,
				show:false
			}
		},
		methods: {
			start(){
				this.state%2==0?this.manager.launch():this.manager.stop()
				this.state++
			},
			tomatoPlus (){
				let forbidden=false, //防止重复触发定时器
					timmer,
					count;
				let logTime = (str,time)=>{
					let start =0 //初始时间
					
					//毫秒转换时分秒函数
					let change = (str,mss)=>{
						let day = parseInt(mss/(1000*60*60*24)),
							hour =parseInt(mss%(1000*60*60*24)/(1000*60*60)),
							minute =parseInt(mss%(1000*60*60)/(1000*60)),
							second =parseInt(mss%(1000*60)/1000);
							this.time=[minute,second]
						console.log(str,day,hour,minute,second)
					}
					
					//函数开始初始化
					change(str,start)
					start=start+1000
					
					//每秒执行函数
					let doLog = function (){
						count = setTimeout(()=>{
						 	if(time === start || start > time){
						 		return
						 	}
						 	change(str,start)
						 	start=start+1000
							doLog()
						 },1000)
					 }
					 doLog()
				}
				var num =0
				var bigr=0
				var arr= [
							{time:25*1000,str:'工作中'},
							{time:5*1000,str:'小休息'},
							{time:25*1000,str:'大休息'}]
							
							
				var doArr =(n)=>{
					console.log(n,arr[num].str,arr[num].time)
					this.tomatoState=arr[num].str
					logTime(arr[num].str,arr[num].time)
				    timmer =setTimeout(()=>{
								doArr(arr[num].time)
							},n)
					num++
					//判断函数若放上边会导致数组判断不准确
					if(bigr%3==0 && bigr !==0){
						if(num==1){
							num=2
						}
						if (num == arr.length){
							num=0
							bigr++
						}
					}else{
						if (num == arr.length-1){
							num=0
							bigr++
						}
					}
					
				}
				
				let stop = ()=>{
					clearTimeout(timmer)
					clearTimeout(count)
					forbidden=false
					num=0
					}
					
					
				return {
					launch:function(){
						if(forbidden){
							console.log('已经在执行一个番茄钟了')
							return
						}
						doArr(arr[num].time)
						forbidden=true
					},
					stop
				}
			}
		},
		components:{
			showList
		}
	}
</script>

<style>
	.wrap {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin-top: 50rpx;
		align-items: center;
		text-align: center;
		width: 80%;
		height: 300rpx;
		border-radius: 10rpx;
		box-shadow:0 0 10rpx black;
	}
	.list-wrap {
		width: 80%;
	}
</style>
