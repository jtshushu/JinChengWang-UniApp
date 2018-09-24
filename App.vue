<script>
	export default {
		onLaunch: function () {
			console.log('App Launch');
			//#ifdef APP-PLUS
			/* 5+环境锁定屏幕方向 */
			plus.screen.lockOrientation('portrait-primary'); //锁定
			/* 5+环境升级提示 */
			var server = "https://uniapp.dcloud.io/update"; //检查更新地址
			var req = { //升级检测数据
				"appid": plus.runtime.appid,
				"version": plus.runtime.version,
				"imei": plus.device.imei
			};
			uni.request({
				url: server,
				data: req,
				success: (res) => {
					console.log("success", res);
					if (res.statusCode == 200 && res.data.isUpdate) {
						let openUrl = plus.os.name === 'iOS' ? res.data.iOS : res.data.Android;
						uni.showModal({ //提醒用户更新
							title: '更新提示',
							content: res.data.note ? res.data.note : '是否选择更新',
							success: (res) => {
								if (res.confirm) {
									plus.runtime.openURL(openUrl);
								}
							}
						})
					}
				}
			})
			//#endif
		},
		onShow: function () {
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */

	page,
	view {
		display: flex;/* uni-app默认使用flex布局。因为flex布局有利于跨更多平台，尤其是采用原生渲染的平台。如不了解flex布局，请参考http://www.w3.org/TR/css3-flexbox/。若不需要flex布局可删除本行*/
	}

	page {
		min-height: 100%;
	}
</style>
