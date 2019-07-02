<template>
  <div id="item-news" @click="toContent">
    <div class="detail">
      <div class="title">
        <span>{{detail.title}}</span>
      </div>
      <div class="info">
        <div class="from">{{detail.source}}</div>
        <div class="comment-num">{{detail.comment_count}}评</div>
        <div class="time">{{time}}</div>
      </div>
    </div>
    <div class="img">
      <img v-if="detail.has_image" :src="imgUrl" alt>
    </div>
  </div>
</template>

<script>
export default {
  props: ["detail", "signature"],
  computed: {
    imgUrl: function() {
      if (this.detail.has_image) {
        if (this.detail.image_list.length > 0) {
          return this.detail.image_list[0].url;
        } else if (this.detail.image_list.length == 0) {
          return this.detail.image_url;
        }
      }
      return null;
    },
    time: function() {
      return timeToDate(this.detail.publish_time);
      function timeToDate(time) {
        // 获取当前时间戳
        var currentTime = parseInt(new Date().getTime() / 1000);
        var diffTime = currentTime - time;
        // var second = 0;
        // var minute = 0;
        // var hour = 0;
        if (null != diffTime && "" != diffTime) {
          if (diffTime < 60) {
            diffTime = "刚刚";
          } else if (diffTime > 60 && diffTime < 60 * 60) {
            diffTime = parseInt(diffTime / 60.0) + "分钟前";
          } else if (diffTime >= 60 * 60 && diffTime < 60 * 60 * 24) {
            diffTime = parseInt(diffTime / 3600.0) + "小时前";
          } else {
            //超过1天
            var date = new Date(parseInt(time) * 1000);
            diffTime =
              date.getFullYear() +
              "/" +
              (date.getMonth() + 1) +
              "/" +
              date.getDate();
            //diffTime = parseInt(diffTime) + "秒";
          }
        }
        return diffTime;
      }
    }
  },
  methods: {
    toContent() {
      this.$router.push({
        path: "/content",
        query: {
          item_id: this.detail.item_id,
          signature: this.signature,
          group_id: this.detail.group_id
        }
      });
      // console.log(this.detail.item_id+' signature:'+this.signature)
    }
  }
};
</script>

<style scoped>
#item-news {
  display: flex;
  justify-content: space-around;
  padding: 0.5rem;
  border-bottom: 1px solid #eee;
}

.detail {
  flex: 7;
  max-width: 16rem;
}

.title {
  max-height: 5.1rem;
  overflow: hidden;
}

.info {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  line-height: 0.5rem;
  margin: 1rem 0 0.5rem 0;
  font-size: 0.5rem;
  color: #666;
}

.info > div {
  margin-right: 1em;
}
/*
.from {
  max-width: 6rem;
  overflow: hidden;
}*/

.img {
  height: 4rem;
  max-width: 6rem;
  min-width: 6rem;
  flex: 3;
  margin-left: 0.5rem;
}

.img img {
  width: 100%;
  height: 100%;
  border-radius: 0.2rem;
}
</style>
