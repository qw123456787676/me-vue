<template>
    <div @click="loadingCiyuanData" class="more-button"> 获取更多次元日报 </div>
</template>
<style>
    .more-button {
        width: 4rem;
        height: .8rem;
        color: #FF4B84;
        border: 1px solid ;
        border-radius: .3rem;
        margin: 0.2rem auto;
        text-align: center;
        line-height: .8rem;
        font-size: .4rem;
        cursor: pointer;
        box-shadow: .06rem .02rem .06rem .02rem;
    }

</style>
<script>
    import '../../vuex/store'
    import {beforeStory,beforeDate,moreButtonState} from '../../vuex/action'
    import {getNowDay,getBeforeDate,getBeforeStory} from '../../vuex/getters'
    export default {
        vuex: {
          getters: {
            getNowDay,getBeforeDate,getBeforeStory
          },
          actions: {
              beforeStory,beforeDate,moreButtonState
          }
        },
        methods: {
          loadingCiyuanData () {
              this.$dispatch('loading_status',true)
              if (!this.getBeforeStory.length) {
                  this.beforeDate(new Date(this.getNowDay))
              }
              this.$http.get(this.getBeforeDate).then(function (response) {
                  response.body.stories.forEach (function (entry) {
                      entry.images[0] = entry.images[0].replace(/http\w{0,1}:\/\/p/g, 'https://images.weserv.nl/?url=p');
                  });
                  response.body.date = response.body.date.slice(0,4)+'-'+response.body.date.slice(4,6)+'-'+response.body.date.slice(6)
                  this.beforeStory(response.body);
                  this.$dispatch('loading_status',false)
                  this.beforeDate(new Date(response.body.date))
              })
          }
        }

    }
</script>
