<template>
  <div class="goods_new">
    <div class="banner">
      <div class="title"></div>
    </div>

    <van-list v-model="loading"
              :finished="finished"
              :immediate-check="false"
              finished-text="没有更多了"
              @load="getNewtList">
      <van-card v-for="(item, i) in list"
                :key="i"
                :desc="item.brief"
                :title="item.name"
                :thumb="item.picUrl"
                :price="item.retailPrice + '万'"

                @click="itemClick(item.id)">
      </van-card>
    </van-list>

  </div>
</template>

<script>
import { goodsList } from '@/api/api';
import { Card, List } from 'vant';
import scrollFixed from '@/mixin/scroll-fixed';

export default {
  mixins: [scrollFixed],

  data() {
    return {
      list: [],
      page: 0,
      limit: 10,
      loading: false,
      finished: false
    };
  },

  created() {
    this.init();
  },

  methods: {
    init() {
      this.page = 0;
      this.list = [];
      this.getNewtList();
    },
    getNewtList() {
      this.page++;
      goodsList({
        isNew: true,
        page: this.page,
        limit: this.limit
      }).then(res => {
        this.list.push(...res.data.data.list);
        this.loading = false;
        this.finished = res.data.data.page >= res.data.data.pages;
      });
    },
    itemClick(id) {
      this.$router.push(`/items/detail/${id}`);
    }
  },

  components: {

    [List.name]: List,
    [Card.name]: Card
  }
};
</script>

<style lang="scss" scoped>
.goods_new {
  padding: 20px;
  .banner {
    height: 250px;
    background-image: url('http://oss1.rsc1.cn:9271/group2/M00/00/7C/rBIJomA_VJGAHmiGAAIsBf2a03c984.png');
    background-size: cover;
    margin-bottom: 20px;
    .title {
      text-align: center;
      line-height: 200px;
      color: #ffffff;
      font-size: 40px;
    }
  }
}
</style>
