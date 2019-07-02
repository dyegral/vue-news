<template>
  <div id="content">
    <div class="option">
      <div class="back">
        <img src="../../static/images/back.png" alt @click="back">
      </div>
      <div class="menu">
        <img src="../../static/images/menu.png" alt>
      </div>
    </div>
    <div class="text">
      <div class="title">
        <h3>{{data.title}}</h3>
      </div>
      <div class="real-content" v-html="data.content"></div>
    </div>
    <div class="comments">
      <div class="comment-num" v-if="comments!==''">热门评论</div>
      <div class="comment-item" v-for="(item, index) in comments.comments" :key="index">
        <img class="head" :src="item.user.avatar_url" alt>
        <div class="name-and-star">
          <div class="name">{{item.user.name}}</div>
          <div class="star">
            <span>{{item.digg_count}}</span>
            <img class="img-star" src="../../static/images/star.png" alt>
          </div>
        </div>
        <div class="comment-content">{{item.text}}</div>
        <div class="time">{{item.create_time}}</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      data: "",
      comments: "",
      commentsNum: 0,
      group_id: "",
      item_id: ""
    };
  },
  methods: {
    back() {
      this.$router.go(-1);
    }
  },
  created() {
    this.group_id = this.$route.query.group_id;
    this.item_id = this.$route.query.item_id;

    // console.log(this.group_id+' '+this.item_id)

    let that = this;
    // console.log(this.$route.query.id+' '+this.$route.query.signature)
    //获取新闻内容
    axios
      .get(
        `http://zzzia.net:8080/toutiao/cross?url=https://m.toutiao.com/i${
          this.$route.query.item_id
        }/info/?_signature=${this.$route.query.signature}&i=${
          this.$route.query.item_id
        }`
      )
      .then(function(res) {
        //返回的数据都在res.data里面
        // console.log(res.data);
        that.data = res.data.info.data;
        // console.log(that.data);
      })
      .catch(function() {});

    //获取评论
    axios
      .get(
        `http://zzzia.net:8080/toutiao/cross?url=https://www.toutiao.com/api/comment/list/?group_id=${
          this.$route.query.group_id
        }&item_id=${this.$route.query.item_id}&offset=${
          this.commentsNum
        }&count=20`
      )
      .then(function(res) {
        //返回的数据都在res.data里面
        // console.log(res.data);
        
        for (let i = 0; i < res.data.info.data.comments.length; i++) {
          res.data.info.data.comments[i].create_time = timeToDate(
            res.data.info.data.comments[i].create_time
          );
        }

        that.comments = res.data.info.data;
        that.commentsNum += 20;
      })
      .catch(function() {});

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
  },
  mounted() {
    // let that = this;
    // window.addEventListener("scroll", function() {
    //   //滑动到底部 发请求加载更多
    //   if (
    //     document.documentElement.scrollTop + window.screen.height >=
    //     document.documentElement.scrollHeight - 1
    //   ) {
    //     //获取评论
    //     var url = `http://zzzia.net:8080/toutiao/cross?url=https://www.toutiao.com/api/comment/list/?group_id=${
    //           that.group_id
    //         }&item_id=${that.item_id}&offset=${
    //           that.commentsNum
    //         }&count=20`;
    //         console.log(url);
    //     axios
    //       .get(
    //         url
    //       )
    //       .then(function(res) {
    //         //返回的数据都在res.data里面
    //         console.log(res.data);
    //         for (let i = 0; i < res.data.info.data.comments.length; i++) {
    //           that.comments.comments.push(res.data.info.data.comments[i]);
    //         }
    //         that.commentsNum += 20;
    //       })
    //       .catch(function() {});
    //   }
    // });
  }
};
</script>

<style scoped>
.content {
  box-sizing: border-box;
}

.option {
  box-sizing: border-box;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: space-between;
  height: 2.5rem;
  width: 100%;
  padding: 0.5rem;
  background-color: #fff;
  z-index: 999;
}

.option img {
  width: 1.5rem;
  height: 1.5rem;
}

.text {
  margin-top: 3rem;
  padding: 0 0.5rem;
}

.comments {
  padding: 0 0.5rem;
}

.comment-num {
  margin: 1rem 0;
  font-size: 0.9rem;
  font-weight: 600;
}

.real-content >>> img {
  width: 100%;
  height: 100%;
}

.real-content >>> .pgc-img-caption {
  text-align: center;
  font-size: 70%;
  color: darkgrey;
}

.real-content >>> .tt-video-box {
  /* width: 100%; */
  /* height: 100%; */
  width: auto !important;
  height: 200px;
  min-height: 200px;
}

.real-content >>> p {
  font-size: 0.9rem;
  letter-spacing: 2px;
  font-weight: 400;
  /* text-indent: 1.5em; */
}

.real-content >>> p:first-child {
  color: slategray;
  font-size: 80%;
}

.real-content >>> h1 {
  /* text-decoration: underline; */
  font-size: 200%;
  font-weight: lighter;
}

.comment-item {
  position: relative;
  margin-left: 2rem;
  margin-bottom: 2rem;
}

.comment-item .head {
  position: absolute;
  top: 0;
  left: -2rem;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
}

.name-and-star {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.name {
  font-size: 0.7rem;
  font-weight: 600;
}

.star {
  display: flex;
  align-items: center;
  font-size: 0.7rem;
}

.img-star {
  width: 1rem;
  height: 1rem;
  margin-left: 0.1rem;
}

.comment-content {
  font-size: 0.8rem;
  margin: 0.5rem 0;
}

.time {
  font-size: 0.5rem;
}
</style>