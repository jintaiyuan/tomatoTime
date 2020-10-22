<template>
	<view class="wrap">
		<view class="lis-wrap">
		    <show-list showType=1 :show="show"></show-list>
		</view>
	</view>
    
</template>
<script>
	import showList from '../../../components/showList.vue'
    export default {
		onShow() {
			this.show=!this.show
		},
        data() {
			return {
				show:false
			}
        },
		components:{
			showList
		},
		methods:{
			updateList(){
				this.todoList=[]
				getApp().globalData.todoList.forEach((item)=>{
					if(item.state){
						this.todoList.push(item)
					}
				})
			}
		}
    };
</script>

<style>
	.wrap {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.lis-wrap {
		width:80%;
	}
</style>
