<template>
  <div class="container">
    <nav class="bar bar-nav">
      <a class="button button-link button-nav pull-left" v-link="{path: '/home', replace: true}">
        <span class="icon icon-left"></span>
      </a>
      <h1 class="title">新闻列表</h1>
    </nav>
    <div class="content list" v-infinite-scroll="loadMore">
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
</template>

<script>
import {loader} from '../util/util'
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
        num: 10,
        rand: 1,
        page: 1
      })
      .then(response =>{
        this.$set('tasks', response.data.newslist)
      })
    }
  },
  ready () {
  },
  data () {
    return {
      noimg: require('src/assets/img/noimg.jpg'),
      tasks: [],
      loadMoreNum: 1,
      loading: false
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
  },
  computed: {
    length () {
      return this.items.length
    }
  },
  methods: {
    gotoarticle (localtion) {
      window.open(localtion)
    },
    loadMore () {
      if (this.loading) {
        return
      }
      this.loading = true
      let scroller = $('.list')
      loader.show()
      setTimeout(() => {
        this.loadMoreNum++
        this.$http.get('http://apis.baidu.com/txapi/weixin/wxhot', {
          num: 10,
          rand: 1,
          page: this.loadMoreNum
        })
        .then(response =>{
          this.tasks = [...this.tasks, ...response.data.newslist]
        })
        let scrollTop = scroller[0].scrollHeight - scroller.height() - 20
        scroller.scrollTop(scrollTop)
        this.loading = false
        loader.hide()
      }, 1500)
    }
  }
}
</script>
