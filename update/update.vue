<template>
	<view>
		<view class="example-body">
			<uni-search-bar radius="100" clearButton="always" @confirm="search"  placeholder="输入要查找的表单序号" />
		</view>
		<view class="example-body">
			<view class="example-box">
				<uni-card :is-shadow="true" title="最美的风景" mode="style" thumbnail="https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/muwu.jpg" extra="DCloud 2019-05-20 12:32:19" note="true" @click="clickCard">
					<view>
						uni-app 是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可编译到iOS、Android、H5、以及各种小程序等多个平台。 即使不跨端，uni-app同时也是更好的小程序开发框架
					</view>
					<template v-slot:footer>
						<view class="footer-box">
							<view class="footer-box__item" @click.stop="footerClick('删除')"><text> 删除</text></view>
							<view class="footer-box__item" @click.stop="footerClick('更新')"><text> 更新</text></view>
						</view>
					</template>
				</uni-card>
			</view>
			<view class="example-box">
				<uni-card :is-shadow="true" title="最美的风景" mode="style" thumbnail="https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/muwu.jpg" extra="DCloud 2019-05-20 12:32:19" note="true" @click="clickCard">
					<view>
						uni-app 是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可编译到iOS、Android、H5、以及各种小程序等多个平台。 即使不跨端，uni-app同时也是更好的小程序开发框架
					</view>
					<template v-slot:footer>
						<view class="footer-box">
							<view class="footer-box__item" @click.stop="footerClick('删除')"><text> 删除</text></view>
							<view class="footer-box__item" @click.stop="footerClick('更新')"><text> 更新</text></view>
						</view>
					</template>
				</uni-card>
			</view>
		</view>
	</view>
</template>

<script>
	import uniSearchBar from '@/components/uni-search-bar/uni-search-bar.vue'
	import uniCard from '@/components/uni-card/uni-card.vue'
	export default {
		components: {
			uniSearchBar,
			uniCard
		},
		data() {
			return {
				searchVal: '',
				Tips: ['更新', '修改']
			}
		},
		methods: {
			search(res) {
				uni.showModal({
					content: '搜索：' + res.value,
					showCancel: false
				})
			},
			input(res) {
				this.searchVal = res.value
			},
			cancel(res) {
				uni.showModal({
					content: '点击取消，输入值为：' + res.value,
					showCancel: false
				})
			},
			clickCard() {
				uni.showToast({
					title: '点击卡片',
					icon: 'none'
				})
			},
			footerClick(types) {
				uni.showToast({
					title: types,
					icon: 'none'
				})
			}
		}
	}
</script>

<style>
	.footer-box {
		display: flex;
		justify-content: space-between;
		width: 100%;
	
	}
	
	.footer-box__item {
		width: 100%;
		display: flex;
		align-items: center;
	}
	.footer-box__item:nth-child(2) {
		justify-content: center;
	}
	
	.footer-box__item:last-child {
		justify-content: flex-end;
	}
</style>
