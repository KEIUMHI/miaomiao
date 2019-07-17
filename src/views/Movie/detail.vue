<template>
	<div id="detailContainer" class="slide-enter-active">
		<Header title="影片详情">
			<i class="iconfont iconleft" @touchstart="handleToBack"></i>
		</Header>
		<Loading v-if="isLoading" />
		<div v-else id="content" class="contentDetail">
			<!-- <div class="detail_list">
				<div class="detail_list_bg"></div>
				<div class="detail_list_filter"></div>
				<div class="detail_list_content">
					<div class="detail_list_img">
						<img src="/images/maozz01.jpg" alt="">
					</div>
					<div class="detail_list_info">
						<h2>无名之辈</h2>
						<p>A Cool Fish</p>
						<p>9.2</p>
						<p>喜剧，犯罪</p>
						<p>中国大陆 / 108分钟</p>
						<p>2018-11-16大陆上映</p>
					</div>
				</div>
			</div>
			<div class="detail_intro">
				<p></p>
			</div>
			<div class="detail_player swiper-container">
				<ul class="swiper-wrapper">
					<li class="swiper-slide">
						<div>
							<img src="/images/maozz02.jpeg" alt="">
						</div>
						<p>陈建斌</p>
						<p>马先勇</p>
					</li>
				</ul>
			</div> -->
			<div class="detail_list">
				<div class="detail_list_bg"></div>
				<div class="detail_list_filter"></div>
				<div class="detail_list_content">
					<div class="detail_list_img">
						<img :src="detailMovie.img | setWH('148.208')" alt="">
					</div>
					<div class="detail_list_info">
						<h2>{{detailMovie.nm}}</h2>
						<p>{{detailMovie.enm}}</p>
						<p>{{detailMovie.sc}}</p>
						<p>{{detailMovie.cat}}</p>
						<p>{{detailMovie.src}} / {{detailMovie.dur}}分钟</p>
						<p>{{detailMovie.pubDesc}}</p>
					</div>
				</div>
			</div>
			<div class="detail_intro">
				<p>{{detailMovie.dra}}</p>
			</div>
			<div class="detail_player swiper-container" ref="detail_player">
				<ul class="swiper-wrapper">
					<li class="swiper-slide" v-for="(item, index) in detailMovie.photos" :key="index">
						<div>
							<img :src="item | setWH('140.127')" alt="">
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
	import Header from '@/components/Header'
	export default {
		name: 'Detail',
		props: {
			movieId: {
				type: String || Number,
				required: true
			}
		},
		data() {
			return {
				detailMovie: {},
				isLoading: true
			}
		},
		mounted() {
			// console.log(this.movieId)
			this.axios.get(`/api/detailmovie?movieId=${this.movieId}`).then((res) => {
				console.log(res)
				const msg = res.data.msg
				if (msg === 'ok') {
					this.isLoading = false
					this.detailMovie = res.data.data.detailMovie
					this.$nextTick(() => {
						new Swiper(this.$refs['detail_player'], {
							slidePerView: 'auto',
							freeMode: true,
							freeModeSticky: true
						})
					})
				}
			})
		},
		methods: {
			handleToBack() {
				this.$router.back()
			}
		},
		components: {
			Header
		}
	}
</script>

<style scoped>
	#detailContainer {
		position: absolute;
		top: 0;
		left: 0;
		z-index: 100;
		width: 100%;
		min-height: 100%;
		background: #fff;
	}

	#detailContainer.slide-enter-active {
		animation: slideMove .3s;
	}

	@keyframes slideMove {
		0% {transform: translateX(100%);}
		100% {transform: translateX(0);}
	}

	#content.contentDetail {
		display: block;
		margin-bottom: 0;
	}

	#content .detail_list {
		position: relative;
		overflow: hidden;
		width: 100%;
		height: 200px;
	}

	.detail_list .detail_list_bg {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: url(/images/maozz01.jpg) 0 40%;
		background-size: cover;
		filter: blur(20px);
	}

	.detail_list .detail_list_filter {
		position: absolute;
		top: 0;
		left: 0;
		z-index: 1;
		width: 100%;
		height: 100%;
		background-color: #40454d;
		opacity: .55;
	}


	.detail_list .detail_list_content {
		display: flex;
		position: absolute;
		top: 0;
		left: 0;
		z-index: 2;
		width: 100%;
		height: 100%;
	}

	.detail_list .detail_list_content {
		display: flex;
		position: absolute;
		top: 0;
		left: 0;
		z-index: 2;
		width: 100%;
		height: 100%;
	}

	.detail_list .detail_list_img {
		margin: 20px;
		width: 108px;
		height: 150px;
		border: 1px solid #f0f2f3;
	}

	.detail_list .detail_list_img img {
		width: 100%;
		height: 100%;
	}

	.detail_list .detail_list_info {
		margin-top: 20px;
	}

	.detail_list .detail_list_info h2 {
		color: #fff;
		font-size: 20px;
		font-weight: normal;
		line-height: 40px;
	}

	.detail_list .detail_list_info p {
		color: #ccc;
		font-size: 14px;
		line-height: 20px;
	}

	#content .detail_intro {
		padding: 10px;
	}

	#content .detail_player {
		margin: 20px;
	}

	.detail_player .swiper-slide {
		margin-right: 20px;
		width: 70px;
		text-align: center;
		font-size: 14px;
	}

	.detail_player .swiper-slide img {
		margin-bottom: 5px;
		width: 100%;
	}

	.detail_player .swiper-slide p:nth-of-type(2) {
		color: #999;
	}
</style>