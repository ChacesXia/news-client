<template>
<div class="container">
  <div class="content home" distance="55" v-pull-to-refresh="refresh">
    <v-layer></v-layer>
    <slider :banner="banner"></slider>
      <v-content type="block-title" style="margin: 0 0 0.4rem;
    -webkit-box-shadow: 0 .06rem 0 #ccc;box-shadow: 0 .06rem 0 #ccc;background-color: white;">
      <btn style="float:left;margin: .4rem 0 .3rem .6rem;border:0;color:#6d6d72;padding:0">
      推荐新闻
      </btn>
      <btn types="link"
        style="float:right;margin: .4rem .6rem .3rem 0;border:0;border:0;padding:0"
        v-link="{path: '/list', replace: true}">
        更多
      </btn>
    </v-content>
    <div class="card-container">
      <v-card-container v-for="task in tasks | orderBy 'created' -1">
        <card type="content" @click="gotoarticle(task.url)">
          <list type="media">
              <li class="item-content">
                <item type="media">
                  <img :src="task.picUrl" onerror="this.src='{{noimg}}'" width="88">
                </item>
                <item type="inner">
                  <item type="title-row">
                    <item type="text">{{task.title}}</item>
                    <item type="text"></item>
                  </item>
                </item>
              </li>
          </list>
        </card>
        <card type="footer" >
          <div style="color:gray">
          {{task.ctime}}
          </div>
          <span style="margin-left: 1rem;padding: .1rem;">{{task.description}}</span>
        </card>
      </v-card-container>
    </div>
  </div>
</div>
</div>
</template>

<script>
import Slider from '../components/Slider'
import Bar from '../components/Bar'
import VLayer from '../components/PullToRefreshLayer'
import VCardContainer from '../components/Card'
import Btn from '../components/Button'
import Card from '../components/CardItem'
import VContent from '../components/Content'
import List from '../components/List'
import Item from '../components/ListItem'
import $ from 'zepto'

export default {
  route: {
    data ({to, next}) {
      return this.$http.get('http://apis.baidu.com/txapi/weixin/wxhot', {
        num: 20,
        rand: 1,
        page: 1
      })
      .then(response =>{
        this.$set('tasks', response.data.newslist)
      })
    }
  },
  ready () {
    $.init()
  },
  data () {
    return {
      noimg: require('src/assets/img/noimg.jpg'),
      banner: [],
      tasks: []
    }
  },
  computed: {
    length () {
      return this.tasks.length
    }
  },
  methods: {
    refresh () {
      setTimeout(function () {
        this.$http.get('http://apis.baidu.com/txapi/weixin/wxhot', {
          num: 20,
          rand: 1,
          // word: '*',
          page: 1
          // src: '*'
        })
        .then(response =>{
          this.$set('tasks', response.data.newslist)
        })
        $.pullToRefreshDone('.pull-to-refresh-content')
      }.bind(this), 1500)
    },
    gotoarticle (localtion) {
      window.open(localtion)
    }
  },
  components: {
    Slider,
    Bar,
    VLayer,
    VCardContainer,
    Card,
    VContent,
    List,
    Item,
    Btn
  }
}
</script>

<style>
.container {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}
.home {
  top: -2.2rem !important;
}
.home-bar {
  background: #efeff4;
  height: 2.8rem;
  position: relative;
  box-shadow: 0 .01rem .05rem rgba(0,0,0,.3);
}
.home-bar .tab-item {
  height: 2.8rem;
  background-color: white;
}
</style>
