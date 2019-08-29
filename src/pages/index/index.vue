<template>
	<view>
		<view id='picbox' :class="{'white-bg': isShow}" :style="{'background-image': !isShow?'url('+imageUrl+')':'none'}">
			<view class="pic" :id="index" :class="{'show-pic': isShow}" v-for="(item,index) in pic" :key="index" :data-index='dataIndexs[index]'
			 @touchstart.stop.prevent="touchStart($event,index)" @touchmove.stop.prevent="touchmove($event,index)"
			 @touchend.stop.prevent="touchEnd($event,index)" :style="{'background-image': 'url('+imageUrl+')','background-position': item.backgroundPosition,'left': item.left + 'px','top': item.top + 'px','z-index': item.zIndex,'transition': item.transition}"></view>
		</view>
		<view class='image-title'>原图：</view>
		<view class='controller'>
			<img class="original-image" :src="imageUrl">
			<view id='go' @click="go">{{isShow?'重置':'开始'}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pic: [{
						left: 0,
						top: 0,
						backgroundPosition: '0px 0px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 100,
						top: 0,
						backgroundPosition: '-100px 0px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 200,
						top: 0,
						backgroundPosition: '-200px 0px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 0,
						top: 100,
						backgroundPosition: '0px -100px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 100,
						top: 100,
						backgroundPosition: '-100px -100px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 200,
						top: 100,
						backgroundPosition: '-200px -100px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 0,
						top: 200,
						backgroundPosition: '0px -200px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 100,
						top: 200,
						backgroundPosition: '-100px -200px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
					{
						left: 200,
						top: 200,
						backgroundPosition: '-200px -200px',
						zIndex: 0,
						transition: 'all 0.5s ease 0s'
					},
				],
				picbox: '',
				isShow: false,
				dataIndexs: [1, 2, 3, 4, 5, 6, 7, 8, 9],
				dx: '',
				dy: '',
				newLeft: '',
				newtop: '',
				picWidth: '',
				picHeight: '',
				picboxWidth: '',
				picboxHeight: '',
				startX: '',
				startY: '',
				endX: '',
				endY: '',
				imageUrl: require('@/static/1.jpg')
			}
		},
		onReady() {
			var vw = wx.getSystemInfoSync().windowWidth;
			this.pic.forEach(item => {
				if (item.left == 100) {
					item.left = (vw - 60) / 3;
					if (item.top == 100) {
						item.top = (vw - 60) / 3;
						item.backgroundPosition = -(vw - 60) / 3 + 'px ' + (-(vw - 60) / 3) + 'px';
					} else if (item.top == 200) {
						item.top = 2 * (vw - 60) / 3;
						item.backgroundPosition = -(vw - 60) / 3 + 'px ' + (-(vw - 60) / 3 * 2) + 'px';
					} else {
						item.backgroundPosition = -(vw - 60) / 3 + 'px ' + '0px';
					}
				} else if (item.left == 200) {
					item.left = 2 * (vw - 60) / 3;
					if (item.top == 100) {
						item.top = (vw - 60) / 3;
						item.backgroundPosition = -(vw - 60) / 3 * 2 + 'px ' + (-(vw - 60) / 3) + 'px';
					} else if (item.top == 200) {
						item.top = 2 * (vw - 60) / 3;
						item.backgroundPosition = -(vw - 60) / 3 * 2 + 'px ' + (-(vw - 60) / 3 * 2) + 'px';
					} else {
						item.backgroundPosition = -(vw - 60) / 3 * 2 + 'px ' + '0px';
					}
				} else {
					if (item.top == 100) {
						item.top = (vw - 60) / 3;
						item.backgroundPosition = '0px ' + (-(vw - 60) / 3) + 'px';
					} else if (item.top == 200) {
						item.top = 2 * (vw - 60) / 3;
						item.backgroundPosition = '0px ' + (-(vw - 60) / 3 * 2) + 'px';
					}
				}
			})
			var query = wx.createSelectorQuery();
			var picbox = query.select('#picbox');
			var pic = query.selectAll('.pic');
			pic.boundingClientRect(data => {
				this.picHeight = data[0].height;
				this.picWidth = data[0].width;
			}).exec();
			picbox.boundingClientRect(data => {
				this.picbox = data;
				this.picboxHeight = data.height;
				this.picboxWidth = data.width;
			}).exec();
		},
		methods: {
			go() {
				this.isShow = true;
				for (var i = 0; i < 20; i++) { //随机打乱
					var a = Math.floor(Math.random() * 9);
					var b = Math.floor(Math.random() * 9);
					if (a != b) {
						this.random(a, b);
					}
				}
			},
			random(a, b) {
				var aEle = this.pic[a];
				var bEle = this.pic[b];
				var _left;
				_left = aEle.left;
				aEle.left = bEle.left;
				bEle.left = _left;
				var _top;
				_top = aEle.top;
				aEle.top = bEle.top;
				bEle.top = _top;
				var _index;
				_index = this.dataIndexs[a];
				this.dataIndexs[a] = this.dataIndexs[b];
				this.dataIndexs[b] = _index;
			},
			touchStart(e, index) {
				if (!this.isShow) {
					return false;
				}
				this.pic[index].zIndex = 100; //设置拖拽元素的z-index值，使其在最上面。
				this.dx = e.touches[0].pageX - this.pic[index].left; //记录触发拖拽的水平状态发生改变时的位置
				this.dy = e.touches[0].pageY - this.pic[index].top; //记录触发拖拽的垂直状态发生改变时的位置
				this.startX = this.pic[index].left; //记录当前初始状态水平发生改变时的位置
				this.startY = this.pic[index].top; //offsetTop等取得的值与this.style.left获取的值区别在于前者不带px,后者带px
				this.pic[index].transition = 'none';
			},
			touchmove(e, index) {
				if (!this.isShow) {
					return false;
				}
				this.newLeft = e.touches[0].pageX - this.dx; //记录拖拽的水平状态发生改变时的位置
				this.newtop = e.touches[0].pageY - this.dy;
				if (this.newLeft <= -this.picWidth / 2) { //限制边界代码块，拖拽区域不能超出边界的一半
					this.newLeft = -this.picWidth / 2;
				} else if (this.newLeft >= (this.picboxWidth - this.picWidth / 2)) {
					this.newLeft = (this.picboxWidth - this.picWidth / 2);
				}
				if (this.newtop <= -this.picHeight / 2) {
					this.newtop = -this.picWidth / 2;
				} else if (this.newtop >= (this.picboxHeight - this.picHeight / 2)) {
					this.newtop = (this.picboxHeight - this.picHeight / 2);
				}
				this.pic[index].left = this.newLeft;
				this.pic[index].top = this.newtop; //设置目标元素的left,top
			},
			change(obj, x, y) { //交换函数，判断拖动元素的位置是不是进入到目标原始1/2，这里采用绝对值得方式
				for (var i = 0; i < this.pic.length; i++) { //还必须判断是不是当前原素本身。将自己排除在外
					if (Math.abs(this.pic[i].left - x) <= this.picWidth / 2 && Math.abs(this.pic[i].top - y) <= this.picHeight / 2 &&
						i !=
						obj)
						return i;
				}
				return obj; //返回当前
			},
			touchEnd(e, index) {
				if (!this.isShow) {
					return false;
				}
				this.pic[index].zIndex = 0;
				this.pic[index].transition = 'all 0.5s ease 0s'; //添加css3动画效果
				this.endX = e.changedTouches[0].pageX - this.dx;
				this.endY = e.changedTouches[0].pageY - this.dy; //记录滑动结束时的位置，与进入元素对比，判断与谁交换
				var obj = this.change(e.target.id, this.endX, this.endY); //调用交换函数
				if (obj == e.target.id) { //如果交换函数返回的是自己
					this.pic[obj].left = this.startX;
					this.pic[obj].top = this.startY;
				} else { //否则
					var _left = this.pic[obj].left;
					this.pic[obj].left = this.startX;
					this.pic[index].left = _left;
					var _top = this.pic[obj].top;
					this.pic[obj].top = this.startY;
					this.pic[index].top = _top;
					var _index = this.dataIndexs[obj];
					this.dataIndexs[obj] = this.dataIndexs[index];
					this.dataIndexs[index] = _index; //交换函数部分，可提取
				}
				if (this.dataIndexs.join('') == '123456789') {
					let _this = this;
					uni.showModal({
						title: '提示',
						showCancel: false,
						content: '恭喜你完成了拼图！',
						success: function(res) {
							if (res.confirm) {
								_this.isShow = false;
							}
						}
					});
				}
			}
		}
	}
</script>

<style>

	.controller {
		display: flex;
		margin: 20px auto;
		width: calc(100vw - 60px);
		justify-content: space-between;
		align-items: center;
	}

	#picbox {
		position: relative;
		height: calc(100vw - 60px);
		background-size: contain;
		margin: 20px 30px 50px 30px;
	}

	.image-title {
		margin: 10px auto;
		width: calc(100vw - 60px);
		font-size: 18px;
	}

	#picbox.white-bg {
		background: #FFFFFF;
	}

	#go {
		width: 70px;
		line-height: 40px;
		border: 1px solid #333333;
		font-size: 22px;
		text-align: center;
	}

	.original-image {
		width: calc((100vw - 60px)/3*2);
		height: calc((100vw - 60px)/3*2);
	}

	.pic {
		width: calc((100vw - 60px)/3 - 3px);
		height: calc((100vw - 60px)/3 - 3px);
		float: left;
		background-size: calc(100vw - 60px) calc(100vw - 60px);
		position: absolute;
		transition: all 0.5s ease 0s;
	}

	.pic.show-pic {
		display: block;
	}
</style>
