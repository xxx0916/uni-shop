<template>
	<view>
		<view class="goods-list">
			<view v-for="(goods,i) in goodsList" :key="i" @click="gotoDetail(goods)">
				<!-- 为 my-goods 组件动态绑定 goods 属性的值 -->
				<my-goods :goods="goods"></my-goods>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 请求参数对象
				queryObj:{
					// 查询关键字
					query:'',
					// 商品分类id
					cid:'',
					// 页码值
					pagenum:1,
					// 每页显示多少条数据
					pagesize:7
				},
				// 商品列表数据
				goodsList:[],
				// 总数量，用于分页
				tatal:0,
				// 是否正在请求数据
				isloading:false
			};
		},
		onLoad(options) {
			// 将页面参数转存到this.queryObj对象中
			this.queryObj.query=options.query||''
			this.queryObj.cid=options.cid||''
			this.getGoodsList()
		},
		// 触底事件
		onReachBottom(){
			// 判断是否还有下一页数据
			// 当前的页码值 * 每页显示多少条数据 >= 总数条数
			// pagenum * pagesize >= total
			if(this.queryObj.pagenum*this.queryObj.pagesize>=this.tatal) return uni.$showMsg("数据加载完毕")
			// 判断是否正在请求其它数据，如果是，则不发起额外的请求
			if(this.isloading) return
			// 让页码值自增 +1
			this.queryObj.pagenum+=1
			// 重新获取列表数据
			this.getGoodsList()
		},
		// 下拉刷新操作
		onPullDownRefresh(){
			// 重置关键数据
			this.queryObj.pagenum=1
			this.tatal=0
			this.isloading=false
			this.goodsList=[]
			// 重新发起请求
			this.getGoodsList(()=>uni.stopPullDownRefresh())
		},
		methods:{
			// 获取商品列表
			async getGoodsList(cb){
				this.isloading=true
				const {data:res} = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
				this.isloading=false
				// 只要数据请求完毕，就立即按需调用 cb 回调函数
				cb&&cb()
				if(res.meta.status !== 200) return uni.$showMsg()
				this.goodsList=[...this.goodsList,...res.message.goods]
				this.total=res.message.tatal
			},
			// 点击跳转到商品详情页
			gotoDetail(goods){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id=' + goods.goods_id
				})
			}
		}
	}
</script>

<style lang="scss">

</style>
