<template>
	<view class="slot">
		<view class="slot-inner" :style="[showSearch ? { minHeight: navBarHeight + 'px' } : { height: navBarHeight + 'px' }]">
			<view
				v-if="showSearch"
				class="inner-showSearch"
				:style="[
					{ minHeight: menuHeight + 'px' },
					{ lineHeight: menuHeight + 'px' },
					{ paddingLeft: menuRight * 2 + 'px' },
					{ paddingRight: menuRight * 2 + 'px' },
					{ paddingTop: navBarHeight - menuHeight - menuTop + 'px' },
					{ paddingBottom: '20rpx' }
				]"
			>
				<view class="tabList">
					<view class="tab" v-for="(item, index) in tabOps" :key="index" :class="{ active: current == index }" @click="change(index)">{{ item }}</view>
				</view>
				<view class="search-slot" :style="{ paddingTop: '16px' }">
					<u-search
						class="search-components"
						@click="goSearchPage(url)"
						disabled
						:placeholder="placeholder"
						:showAction="false"
						shape="square"
						borderRadius="0rpx"
						:bg-color="searchBgColor"
						:class="showSlot ? 'active' : ''"
					></u-search>
					<slot showSlot></slot>
				</view>
			</view>
			<view class="inner" v-else>
				<view
					@click="$u.route({ type: 'navigateBack', delta: 1 })"
					class="left"
					v-if="showBack"
					:style="[{ minHeight: menuHeight + 'px' }, { lineHeight: menuHeight + 'px' }, { left: menuRight * 2 + 'px' }, { bottom: menuBotton + 'px' }]"
				>
					<u-icon size="32" name="arrow-leftward"></u-icon>
				</view>
				<view
					class="tabList"
					:style="[
						{ minHeight: menuHeight + 'px' },
						{ lineHeight: menuHeight + 'px' },
						{ left: showBack ? menuRight * 2 + 40 + 'px' : menuRight * 2 + 'px' },
						{ bottom: menuBotton + 'px' }
					]"
				>
					<view class="tab" v-for="(item, index) in tabOps" :key="index" :class="{ active: current == index }" @click="change(index)">{{ item }}</view>
				</view>
			</view>
		</view>
		<view class="slot-height" v-if="showSearch" :style="[{ height: navBarHeight + menuHeight + 27 + 'px' }]"></view>
		<view class="slot-height" v-else :style="[{ height: navBarHeight + 'px' }]"></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	name: 'navbar-tab-search',
	props: {
		// ?????????
		tabOps: {
			type: Array,
			default: () => {
				return ['tab1', 'tab2'];
			}
		},
		// ????????????
		placeholder: {
			type: String,
			default: '?????????'
		},
		// ?????????????????????????????????????????????????????????
		showSlot: {
			type: Boolean,
			default: false
		},
		// ?????????????????????
		showSearch: {
			type: Boolean,
			default: false
		},
		// ?????????url
		url: {
			type: String,
			default: ''
		},
		// ????????????????????????
		showBack: {
			type: Boolean,
			default: false
		}
	},
	data() {
		return {
			// ???????????????
			navBarHeight: app.globalData.navBarHeight,
			menuRight: app.globalData.menuRight,
			menuBotton: app.globalData.menuBotton,
			menuHeight: app.globalData.menuHeight,
			menuTop: app.globalData.menuTop,
			// ??????tab
			current: 0,
			// ?????????
			searchBgColor: this.$appTheme.appThemeSearchBgColor
		};
	},
	methods: {
		// ??????tab
		change(index) {
			this.current = index;
			this.$emit('change', index);
		},

		// ???????????????
		goSearchPage(url) {
			uni.navigateTo({
				url
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.slot {
	width: 100vw;
}
.slot-inner {
	width: 100%;
	position: fixed;
	top: 0;
	left: 0;
	z-index: 899;
	overflow: hidden;
}
.tabList {
	display: flex;
	align-items: center;
	.tab {
		font-size: 36rpx;
		font-weight: 400;
		margin-right: 48rpx;
		color: $app-theme-navbar-tab-color;
		&.active {
			font-weight: 500;
			color: $app-theme-navbar-tab-color-active;
			font-size: 40rpx;
			position: relative;
			&::before {
				content: '';
				width: 16rpx;
				height: 6rpx;
				background: $app-theme-navbar-tab-color-active;
				border-radius: 1px;
				position: absolute;
				bottom: -10rpx;
				left: 50%;
				transform: translate(-50%, 0);
			}
		}
	}
}
.inner {
	width: 100%;
	position: relative;
	height: 100%;
	background-color: $app-theme-bg-color;
	.left {
		position: absolute;
		z-index: 999;
		display: flex;
		align-items: center;
		// ?????????
		width: 30x;
		margin-right: 10px;
	}
	.tabList {
		width: 50%;
		position: absolute;
		z-index: 999;
		display: flex;
		align-items: center;
	}
}
.inner-showSearch {
	width: 100%;
	height: 100%;
	background-color: $app-theme-bg-color;
	.tabList {
		display: flex;
		align-items: center;
	}
}
.search-slot {
	width: 100%;
	display: flex;
	justify-content: space-between;
	align-items: center;
	.search-components {
		width: 100%;
		&.active {
			width: 86%;
		}
	}
}
</style>
