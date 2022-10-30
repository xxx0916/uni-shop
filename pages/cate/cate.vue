<template>
	<view class="nav-list">
		<view class="nav-item" v-for="(item,i) in navList" :key="i">
			<image :src="item.cat_icon"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onLoad(){
			
		},
		methods:{
			
		}
	}
</script>

<style lang="scss">

</style>
