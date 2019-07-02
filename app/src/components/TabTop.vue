<template>
  <ul class="tabs">
    <li
      class="tab"
      v-for="(item, index) in tabs"
      :key="index"
      :class="{'active':activeNum === index}"
      @click="active(index)"
    >
      <span>{{item.name}}</span>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      activeNum: 0,
      tabs: [
        {
          name: '推荐',
          code: '__all__'
        },
        {
          name: '热点',
          code: 'news_hot'
        },
        {
          name: '社会',
          code: 'news_society'
        },
        {
          name: '娱乐',
          code: 'news_entertainment'
        },
        {
          name: '科技',
          code: 'news_tech'
        },
        {
          name: '军事',
          code: 'news_military'
        },
        {
          name: '体育',
          code: 'news_sports'
        },
        {
          name: '汽车',
          code: 'news_car'
        },
        {
          name: '财经',
          code: 'news_finance'
        },
        {
          name: '国际',
          code: 'news_world'
        },
        {
          name: '时尚',
          code: 'news_fashion'
        },
        {
          name: '旅游',
          code: 'news_travel'
        },
        {
          name: '探索',
          code: 'news_discovery'
        },
        {
          name: '育儿',
          code: 'news_baby'
        },
        {
          name: '养生',
          code: 'news_regimen'
        },
        {
          name: '故事',
          code: 'news_story'
        },
        {
          name: '美文',
          code: 'news_essay'
        },
        {
          name: '游戏',
          code: 'news_game'
        },
        {
          name: '历史',
          code: 'news_history'
        },
        {
          name: '美食',
          code: 'news_food'
        },
      ]
    };
  },
  methods: {
    active(index) {
      this.activeNum = index;
      this.$emit("clickTab", this.tabs[index].code);
    }
  },
  created() {
    
  },
};
</script>

<style scoped>
.tabs {
  height: 1.5rem;
  color: #777;
  overflow-x: scroll;
  white-space: nowrap;
  font-size: 0.9rem;
  padding: 0.5rem 0;
  margin: 0;
}

.tabs::-webkit-scrollbar {
  display: none;
}

.tab {
  line-height: 1.5rem;
  display: inline-block;
  list-style: none;
  margin: 0 0.5rem;
}

.active {
  box-sizing: border-box;
  font-weight: 900;
  color: black;
  border-bottom: 2px solid skyblue;
}
</style>