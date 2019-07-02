<template>
  <div id="index">
    <div class="top-fixed">
      <TabTop @clickTab="clickTab" class="tab-top"></TabTop>
    </div>
    <div class="content">
      <div v-if="tag===istag">
        <ItemNews v-for="(item, index) in ItemNews" :key="index" :detail="item" :signature="signature"></ItemNews>
      </div>
      <div class="loading">loading</div>
    </div>
    <div class="to-top">
      <img src="../../static/images/top.png" alt @click="toTop">
    </div>
  </div>
</template>

<script>
import TabTop from "./TabTop";
import ItemNews from "./ItemNews";

import axios from "axios";

export default {
  components: {
    TabTop,
    ItemNews
  },
  data() {
    return {
      tag: "__all__",
      istag: "__all__",
      as: "",
      cp: "",
      signature: "",
      ItemNews: "",
      canGet: true
    };
  },
  methods: {
    clickTab(tag) {
      //子组件通信
      this.tag = tag;

      let that = this;
      //请求3个加密信息
      // axios
      //   .get("http://zzzia.net:8080/toutiao/init")
      //   .then(function(res) {
      //     //返回的数据都在res.data里面
      //     // console.log(res.data);
      //     if (res.data.status == 200) {
      //       that.as = res.data.info.as;
      //       that.cp = res.data.info.cp;
      //       that.signature = res.data.info._signature;
      //     }
          
          //请求具体内容
          axios
            .get(
              `http://zzzia.net:8080/toutiao/cross?url=https://m.toutiao.com/list/?ac=wap&format=json_raw&tag=${
                that.tag
              }&as=${that.$route.query.as}&cp=${that.$route.query.cp}&_signature=${that.$route.query.signature}`
            )
            .then(function(res) {
              //返回的数据都在res.data里面
              //console.log(res.data);
              that.ItemNews = res.data.info.data;
              // console.log(eval(`'${res.data}'`));
              that.istag = tag;
            })
            .catch(function() {});
        // })
        // .catch(function() {});
    },
    toTop() {
      document.documentElement.scrollTop = 0;
    }
  },
  created() {
    let that = this;
    //请求3个加密信息
    axios
      .get("http://zzzia.net:8080/toutiao/init")
      .then(function(res) {
        //返回的数据都在res.data里面
        // console.log(res.data);
        if (res.data.status == 200) {
          that.as = res.data.info.as;
          that.cp = res.data.info.cp;
          that.signature = res.data.info._signature;
        }

        //请求列表内容
        axios
          .get(
            `http://zzzia.net:8080/toutiao/cross?url=https://m.toutiao.com/list/?ac=wap&format=json_raw&tag=${
              that.tag
            }&as=${that.as}&cp=${that.cp}&_signature=${that.signature}`
          )
          .then(function(res) {
            //返回的数据都在res.data里面
            // console.log(res.data);
            that.ItemNews = res.data.info.data;
            // console.log(eval(`'${res.data}'`));
          })
          .catch(function() {});
      })
      .catch(function() {});
  },
  mounted() {
    let that = this;
    window.addEventListener("scroll", function() {
      //滑动到底部 发请求加载更多
      if(!that.canGet) {
        return;
      }
      if (
        document.documentElement.scrollTop + window.screen.height >=
        document.documentElement.scrollHeight - 1
      ) {
        that.canGet = false;
        //请求具体内容
        axios
          .get(
            `http://zzzia.net:8080/toutiao/cross?url=https://m.toutiao.com/list/?ac=wap&format=json_raw&tag=${
              that.tag
            }&as=${that.as}&cp=${that.cp}&_signature=${that.signature}`
          )
          .then(function(res) {
            //返回的数据都在res.data里面
            // console.log(res.data);
            for (let i = 0; i < res.data.info.data.length; i++) {
              that.ItemNews.push(res.data.info.data[i]);
            }
            // console.log(eval(`'${res.data}'`));
            // if (res.data.info.data == null) {
            //   //更新3个加密信息
            //   axios
            //     .get(`http://zzzia.net:8080/toutiao/init`)
            //     .then(function(res) {
            //       if (res.data.status == 200) {
            //         that.as = res.data.info.as;
            //         that.cp = res.data.info.cp;
            //         that._signature = res.data.info._signature;
            //       }
            //     })
            //     .catch(function() {});
            // }

            that.canGet = true;
          })
          .catch(function() {
            that.canGet = true;
          });
        that.canGet = true;
      }
    });
  }
};
</script>

<style scoped>
.top-fixed {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #fff;
  border-bottom: 1px solid #eee;
}

.content {
  padding: 0 0.5rem;
  margin-top: 2.6rem;
}

.loading {
  height: 4rem;
  text-align: center;
  line-height: 4rem;
}

.to-top {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  width: 2rem;
  height: 2rem;
}
.to-top img {
  width: 100%;
  height: 100%;
}
</style>