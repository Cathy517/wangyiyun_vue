<template>
  <div id="MVDetail" class="area">
      <div class="container" v-if="mvdetailJson">
          <div class="containerLeft">
              <div class="songarea">
                    <h3 class="mvtitle">
                        <span>{{mvdetailJson.name}}</span>
                        <span class="name">{{mvdetailJson.artistName}}</span>
                    </h3>
                </div>
                <div class="songlistbox">
                    <video controls>
                        <source :src="mvdetailJson.brs[1080] | fiterVideo">
                    </video>
                </div>
          <Comment :commentData="mvcommentJson.topComments" name="最热评论"></Comment>
          <Comment :commentData="mvcommentJson.hotComments" name="精彩评论"></Comment>
          <Comment :commentData="mvcommentJson.comments" name="评论"></Comment>
          </div>
          
          <div class="containerRight">
              <div class="margin">
                    <div class="simisongbox">
                        <div class="smallTitle">mv简介</div>
                        <p>播放次数：{{mvdetailJson.playCount}}</p>
                        <p>发布时间：{{mvdetailJson.publishTime}}</p>
                        <p class="mvdesc">{{mvdetailJson.desc}}</p>
                    </div>
                    <div class="simimvbox">
                        <div class="smallTitle">相似mv</div>
                        <MV :mvData="simimvJson" title="search" class="simimvstyle"></MV>
                    </div>
                </div>
          </div>
      </div>
  </div>
</template>
<script>
import { mapActions, mapState } from "vuex";
import Comment from "@/components/Comment/Comment";
import MV from "@/components/MV/MV";
export default {
  name: "MVDetail",
  components: {
    Comment,
    MV
  },
  data() {
    return {
      pagename: "mv详情"
    };
  },
  methods: {
    ...mapActions(["getMVDetailData", "getMVCommentData", "getSimiMVData"])
  },
  computed: {
    ...mapState({
      mvdetailJson: state => state.mv.mvdetail.data,
      mvcommentJson: state => state.mv.mvcomment,
      simimvJson: state => state.mv.simimv.mvs
    })
  },
  mounted() {
    let id = this.$route.query.id;
    this.getMVDetailData({ id }); //mv详情
    this.getMVCommentData({ id }); //mv评价
    this.getSimiMVData({ id }); //相似mv
  }
};
</script>
<style lang="scss">
@import "~@/assets/scss/variables.scss";
#MVDetail {
  .mvtitle {
    span {
      font-size: $font-size * 2;
      &.name {
        font-size: $font-size;
        font-weight: normal;
        margin-left: 10px;
      }
    }
  }
  .simisongbox {
    p {
      line-height: 25px;
    }
  }
  .songlistbox {
    margin-top: 10px;
    video {
      width: 100%;
      height: 360px;
      background-color: $black;
    }
  }
  .simimvstyle {
    li {
      width: 100%;
      margin-right: 0;
      a {
        display: flex;
      }
      .mvcover {
        width: 140px;
      }
      .mvcontent {
        flex: 1;
        margin-left: 10px;
      }
    }
  }
}
</style>
