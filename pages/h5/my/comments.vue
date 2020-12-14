<template>
  <div>
    <van-nav-bar
      title="Your Comment"
      left-text="Back"
      left-arrow
      @click-left="onBack"
      class="navbar"
    />
    <div class="container">
      <div class="title">Your comments</div>
      <a-list
        class="comment-list"
        :header="`${commentsData.length} replies`"
        item-layout="horizontal"
        :data-source="commentsData"
      >
        <a-list-item slot="renderItem" slot-scope="item, index">
          <a-comment :author="item.author" :avatar="item.avatar || imgDataUrl">
            <template slot="actions">
              <span>
                Your comment on<span class="default" @click="goDetail(item.blogId)">{{item.blogTitle}}</span>
              </span>
            </template>
            <p slot="content">
              {{ item.content }}
            </p>
            <!-- <a-tooltip slot="datetime" :title="item.datetime.format('YYYY-MM-DD HH:mm:ss')">
              <span>{{ item.datetime.fromNow() }}</span>
            </a-tooltip> -->
          </a-comment>
        </a-list-item>
      </a-list>
    </div>
  </div>
</template>
<script>
import moment from "moment";
export default {
  layout: "h5",
  data () {
    return {
      imgDataUrl: require("~/assets/img/Asha-Go-dark-circle-logo-no-text.png"),
      commentsData: [],
      moment
    };
  },
  created() {
    this.getComments();
  },
  methods: {
    onBack() {
      if (history.length > 1) {
        history.back();
      }
    },
    goDetail(id) {
      this.$router.push('/blog/detail?blogId=' + id);
    },
    getComments() {
      //
      this.commentsData = [{
          author: "Chloe Wang",
          avatar: "ashago-resource.oss-cn-zhangjiakou.aliyuncs.com/avatar/535917709046882304.jpg",
          blogId: "530862157472243712",
          blogTitle: "All you need to know about Wechat",
          commentId: "535920093571620864",
          content: "Wechat can be so annoying sometimes! so many groups...",
          postAt: "2020-12-14T09:56:13",
          userId: "535917709046882304",
      }];
      this.$Server({
        url: "/comment/list",
        method: "GET",
        params: {
          userId: this.$store.state.userId,
          test: "test"
        }
      }).then(res => {
        if (res.code == 0) {
          this.commentsData = res.data.comments;
        } else {
          this.$message.error(res.msg);
        }
      });
    }
  }
};
</script>
<style scoped lang="less">
.container {
  color: #8d050b;
  //width: 900px;
  padding: 20px;
  .title {
    font-size: 30px;
    line-height: 80px;
    font-weight: 600;
  }
  .comments {
    .item {
      display: flex;
      margin-bottom: 20px;
      align-items: center;
      .delete {
        width: 150px;
        height: 100px;
        line-height: 100px;
        text-align: center;
        border-radius: 5px;
        background: #e7e6e6;
        margin-left: 50px;
        cursor: pointer;
      }
      .item-content {
        flex: 1;
        border: 1px solid #8d050b;
        border-radius: 5px;
        padding: 5px 10px;
        display: flex;
        align-items: center;
        .pic {
          width: 150px;
          height: 100px;
          border: 1px solid #ccc;
          margin-right: 15px;
        }
        .desc {
          flex: 1;
          .title {
            font-size: 18px;
            display: flex;
            justify-content: space-between;
            line-height: 30px;
            font-weight: 500;
          }
          .time {
            font-size: 14px;
            color: black;
            font-weight: 500;
            line-height: 30px;
            border-bottom: 1px solid #ccc;
          }
          .desc-content {
            font-size: 14px;
            color: black;
            .desc-content-item {
              font-weight: 400;
              line-height: 20px;
              .desc-content-item-title {
              }
              .desc-content-item-where {
                color: #8d050b;
              }
            }
          }
        }
      }
    }
  }
}
</style>
