<template>
    <div>
     <!-- <div class="tools">
			<bk-button :theme="'default'" type="submit" :title="'基础按钮'" @click.stop="prePage" class="mr10"> 上一页</bk-button>
			<bk-button :theme="'default'" type="submit" :title="'基础按钮'" @click.stop="nextPage" class="mr10"> 下一页</bk-button>
			<div class="page">{{pageNum}}/{{pageTotalNum}} </div>
			<bk-button :theme="'default'" type="submit" :title="'基础按钮'" @click.stop="clock" class="mr10"> 顺时针</bk-button>
			<bk-button :theme="'default'" type="submit" :title="'基础按钮'" @click.stop="counterClock" class="mr10"> 逆时针</bk-button>
		</div> -->
		<pdf ref="pdf" 
		:src="url" 
		:page="pageNum"
		:rotate="pageRotate"  
		@progress="progress($event)"
		@page-loaded="pageLoaded($event)" 
		@num-pages="pageTotalNum=$event" 
		@error="pdfError($event)" 
		@link-clicked="page = $event">
		</pdf>
    <loading-block ref="loadingRef"></loading-block>


    </div>
</template>
<script>
  import pdf from 'vue-pdf'
  import loadingBlock from '@/components/loading/loading.vue'
	export default {
		name: 'Home',
		components: {
      pdf,
      loadingBlock
		},
		data() {
			return {
				url: "https://test.hwasbank.com/files/2020-08-03/2e7bdfbe-12bc-4120-bbdc-896455a6ecf5.pdf",
				pageNum: 1,
				pageTotalNum: 1,
				pageRotate: 0,
				// 加载进度
				loadedRatio: 0,
				curPageNum: 0,
			}
		},
		mounted: function() {},
		methods: {
      progress(e){
        console.log(e);
        this.loadedRatio=e
        if(e===1) {
         this.$refs.loadingRef.showLoading=false
       }
      },
            // 上一页函数，
			prePage() {
				var page = this.pageNum
				page = page > 1 ? page - 1 : this.pageTotalNum
				this.pageNum = page
			},
            // 下一页函数
			nextPage() {
				var page = this.pageNum
				page = page < this.pageTotalNum ? page + 1 : 1
				this.pageNum = page
			},
            // 页面顺时针翻转90度。
			clock() {
				this.pageRotate += 90
			},
            // 页面逆时针翻转90度。
			counterClock() {
				this.pageRotate -= 90
			},
            // 页面加载回调函数，其中e为当前页数
			pageLoaded(e) {
				this.curPageNum = e

      },
            // 其他的一些回调函数。
			pdfError(error) {
				console.error(error)
			},
    },
    created(){
      this.url = this.$route.query.pdfUrl
    }
	}
</script>

<style lang="scss" scoped>
.pic {
  height:2.09rem ;
}
.content {
  margin-top: .18rem;
  margin-bottom: .49rem;
  .item {
    margin-bottom: .14rem;

  }
}

</style>
