<template>
  <div>
    <div :style="{width:pdfsize+'%'}" class="pdf-style">
      <iframe ref="pdf" :page="pageNum" :rotate="pageRotate" :src="url"
              style="width: 1000px; height: 1000px" @error="pdfError($event)" @progress="loadedRatio = $event"
              @page-loaded="pageLoaded($event)" @num-pages="pageTotalNum=$event" @link-clicked="page = $event">
      </iframe>
    </div>
  </div>
</template>

<script>
import pdf from 'vue-pdf';

export default {
  props: {
    url: ''
  },
  name: 'PdfView',
  components: {
    pdf
  },
  data() {
    return {
      pageNum: 1,
      pageTotalNum: 1,
      pageRotate: 0,
      // 加载进度
      loadedRatio: 0,
      curPageNum: 0,
      pdfsize: 60
    };
  },
  methods: {
    // 页面加载回调函数，其中e为当前页数
    pageLoaded(e) {
      this.curPageNum = e;
    },
    // 其他的一些回调函数。
    pdfError(error) {
      console.error(error);
    },
    handlein() {
      this.pdfsize = this.pdfsize + 10;
    },
    handleout() {
      this.pdfsize = this.pdfsize - 10;
    }

  }
};
</script>

<style scoped>
.pdf-style {
  margin: auto;
}

.div1 {
  margin-left: 40%;
  margin-top: 20px;
}

.page {
  float: left;
}

.page2 {
  margin: 8px;
}
</style>