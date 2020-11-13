<template>
  <div class="detail-container">
    <div>
      <a-spin tip="Loading..." v-show="loadingFlag"></a-spin>
      <div v-show="!loadingFlag" v-if="latestData">
        <a-card class="card-body">
          <div class="author-description">
            <a-avatar style="margin:10px;" size="small" :src="latestData.avatar"/>
            {{latestData.author}}
            <a-divider type="vertical"/>
            {{latestData.time}} read
          </div>
          <div class="article-body" v-html="legacySystemHTML"></div>
          <div>
            <a-button
              class="tag"
              v-for="(tagName, index) in tagList"
              :key="'lastest'+ index"
              v-bind:href="'/category/bloglist?tag='+tagName"
            >{{tagName}}</a-button>
          </div>
          <div class="socialmedia">
            <a-divider/>
            <a-row>
              <a-col :span="12">
                <a
                  target="_blank"
                  v-bind:href="'http://www.facebook.com/sharer.php?u='+ encodeURIComponent(currentUrl)"
                >
                  <a-icon type="facebook"/>
                </a>
                <a
                  target="_blank"
                  v-bind:href="'http://twitter.com/share?url='+ encodeURIComponent(currentUrl)"
                >
                  <a-icon type="twitter"/>
                </a>
                <a
                  target="_blank"
                  v-bind:href="'https://www.linkedin.com/shareArticle?url='+ encodeURIComponent(currentUrl)"
                >
                  <a-icon type="linkedin"/>
                </a>
                <a @click="copyUrl">
                  <a-icon type="link"/>
                </a>
              </a-col>
              <div style="float: right;">
                <a-col style="float: right;">
                  <p>{{latestData.city}}</p>
                </a-col>
              </div>
            </a-row>
          </div>
          <a-divider/>
          <div class="viewsCard" style="margin-bottom: 48px">
            <a-row>
              <a-col :span="10">{{latestData.views}} views</a-col>
              <div style="float: right;">
                <a-col style="float: right;">
                  {{latestData.likes}}
                  <a id="like">
                    <a-icon type="heart" v-show="likeIcon" theme="filled" @click="cancelLike"/>
                    <a-icon type="heart" v-show="!likeIcon" theme="outlined" @click="postLike"/>
                  </a>
                </a-col>
              </div>
            </a-row>
          </div>
          <div class="comment">
            <a-comment>
              <a-avatar
                slot="avatar"
                :src="$store.state.userInfo.avatar || avatarImg"
                style="backgroundColor:#ac4448; margin:5px"
              />
              <div v-if="$store.state.userInfo.avatar" slot="content">
                <a-form-item>
                  <a-textarea :rows="4" :value="value" @change="handleChange"/>
                </a-form-item>
                <a-form-item>
                  <a-button
                    html-type="submit"
                    :loading="submitting"
                    style="float: right;"
                    type="primary"
                    @click="handleSubmit"
                  >Add Comment</a-button>
                </a-form-item>
              </div>
              <div v-else slot="content" class="login-tips">
                <nuxt-link to="../login">Login to leave a comment</nuxt-link>
              </div>
            </a-comment>
            <a-divider/>
            <a-comment v-for="(item, index) in commentsData" :key="'related'+ index">
              <a slot="author">{{item.author}}</a>
              <a-avatar slot="avatar" :src="item.avatar" alt="Han Solo"/>
              <p slot="content">{{item.content}}</p>
            </a-comment>
            <div>
              <a-list
                v-if="comments.length"
                :data-source="comments"
                :header="`${comments.length} ${comments.length > 1 ? 'replies' : 'reply'}`"
                item-layout="horizontal"
              >
                <a-list-item slot="renderItem" slot-scope="item, index">
                  <a-comment
                    :author="item.author"
                    :avatar="item.avatar"
                    :content="item.content"
                    :datetime="item.datetime"
                  />
                </a-list-item>
              </a-list>
            </div>
          </div>

          <div class="related-post">
            <div class="normal-content">
              <p>Related Posts</p>
            </div>
            <a-row :gutter="[8,40]">
              <a-col :md="6" v-for="(item, index) in latestRealatedBlog" :key="'related'+ index">
                <a v-bind:href="'/blog/detail?blogId='+ item.blogId">
                  <a-card>
                    <img slot="cover" alt="example" :src="item.img">
                    <a-card-meta>
                      <div style="height: 54px; white-space: normal;" slot="title">{{item.title}}</div>
                    </a-card-meta>
                  </a-card>
                </a>
              </a-col>
            </a-row>
          </div>
        </a-card>
      </div>
    </div>
    <van-tabbar v-model="active">
      <nuxt-link to="/h5" class="tab-item">
        <van-tabbar-item icon="home-o">Home</van-tabbar-item>
      </nuxt-link>

      <div class="tab-item">
        <van-tabbar-item v-show="likeIcon" theme="filled" @click="cancelLike" icon="like">Like</van-tabbar-item>
        <van-tabbar-item v-show="!likeIcon" theme="outlined" @click="postLike" icon="like-o">Like</van-tabbar-item>
      </div>

      <nuxt-link to="/h5/blog/comments" class="tab-item">
        <van-tabbar-item icon="comment-o">Comment</van-tabbar-item>
      </nuxt-link>
      <div class="tab-item">
        <van-tabbar-item icon="share-o" @click="showShare = true">Share</van-tabbar-item>
      </div>
    </van-tabbar>
    <van-share-sheet
      class="share"
      v-model="showShare"
      title="Share to"
      :options="options"
      @select="onSelect"
    />
  </div>
</template>

<script>
import { constants } from "zlib";
import { connect } from "tls";
import { log } from "util";
import { parse } from "querystring";
import copy from "copy-to-clipboard";
import { Toast } from "vant";
import { thistle } from "color-name";

const Cookie = process.client ? require("js-cookie") : undefined;
const latestData = {};
const commentsData = {};
const latestRealatedBlog = [];
const legacySystemHTML = "";
const blogId = "";
const currentUrl = "";
export default {
  layout(context) {
    return context.isMobile ? "h5" : "default";
  },
  watch: {
    "$route.query"(newValue) {
      console.log(newValue, "Newvalue");
      this.loadingFlag = true;
      this.getData(newValue);
    }
  },
  created() {},
  data() {
    return {
      loadingFlag: true,
      latestData,
      latestRealatedBlog,
      legacySystemHTML,
      tagList: [],
      comments: [],
      submitting: false,
      value: "",
      blogId,
      active: 0,
      commentsData,
      likeIcon: false,
      currentUrl,
      avatarImg: require("~/assets/img/Asha-Go-dark-circle-logo-no-text.png"),
      showShare: false,
      isPC: true,
      options: [
        {
          name: "Facebook",
          icon:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1604062907063&di=d6063a74f0edd572bfce1d9a735a1b9b&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20171012%2F71f50c6aba864c9ababebb866abb964c.png"
        },
        {
          name: "Twitter",
          icon:
            "https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1102075851,2526986960&fm=26&gp=0.jpg"
        },
        {
          name: "Linkedin",
          icon:
            "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2810718230,2808086288&fm=26&gp=0.jpg"
        },
        { name: "Copy link", icon: "link" }
      ]
    };
  },
  mounted() {
    this.currentUrl = location.href;
    this.getData(this.$route.query.blogId, function(data) {
      console.log(data, "DA");
    });
    this.getComments(this.$route.query.blogId);
    this.getLike(this.$route.query.blogId);
  },
  methods: {
    getData(key, callback) {
      this.$Server({
        //url: "http://localhost:8080/blog/get-blog-info",
        url: "/blog/get-blog-info",
        method: "post",
        data: {
          blogId: key
        },
        transformRequest: [
          function(data) {
            let ret = "";
            for (let it in data) {
              ret +=
                encodeURIComponent(it) +
                "=" +
                encodeURIComponent(data[it]) +
                "&";
            }
            ret = ret.substring(0, ret.lastIndexOf("&"));

            console.log(ret, "---");
            return ret;
          }
        ],
        headers: {
          "Content-Type": "application/x-www-form-urlencoded"
        }
      })
        .then(res => {
          this.loadingFlag = false;

          this.latestData = res.data;
          this.latestRealatedBlog = res.dataList;
          var initHTML = res.data.html;
          var reg = /width="([ ]*[0-9])\w+" height="([ ]*[0-9])\w+"/g; //
          this.legacySystemHTML = initHTML.replace(
            reg,
            'width="100%" height="100%"'
          );
          this.tagList = res.data.tag.split(",");
          this.blogId = key;
          console.log(res.data, "DATA");
          console.log(res.dataList, "DATALIST");
        })
        .finally(() => {
          this.loadingFlag = false;
        });
    },
    getComments(key) {
      this.$Server({
        //url: 'http://localhost:8080/comment/list',
        url: "/comment/list",
        method: "get",
        params: {
          blogId: key
        }
      })
        .then(res => {
          if (res.code == 0) {
            this.commentsData = res.data.comments;
            console.log(res.data.comments, "comment");
          }
        })
        .catch(err => {});
    },
    getLike(key) {
      if (this.$store.state.userId) {
        this.$Server({
          //url: 'http://localhost:8080/like/like-or-not',
          url: "/like/like-or-not",
          method: "get",
          params: {
            likeTargetId: key,
            likeTargetType: "BLOG",
            userId: this.$store.state.userId
          }
        })
          .then(res => {
            console.log(res, "get like res");
            this.likeIcon = res.data.likeOrNot;
          })
          .catch(err => {
            console.log(err, "err");
          });
      }
    },
    cancelLike() {
      this.$Server({
        //url: 'http://localhost:8080/comment/post',
        url: "/like/cancel",
        method: "post",
        data: {
          likeTargetId: this.blogId,
          likeTargetType: "BLOG",
          userId: this.$store.state.userId
        }
      })
        .then(res => {
          console.log(res, "cancel like res");
          this.likeIcon = false;
        })
        .catch(err => {
          console.log(err, "err");
        });
    },
    postLike() {
      this.$Server({
        //url: 'http://localhost:8080/comment/post',
        url: "/like/post",
        method: "post",
        data: {
          likeTargetId: this.blogId,
          likeTargetType: "BLOG",
          userId: this.$store.state.userId
        }
      })
        .then(res => {
          console.log(res, "post like res");
          this.likeIcon = true;
        })
        .catch(err => {
          console.log(err, "err");
        });
    },
    handleSubmit() {
      if (!this.value) {
        return;
      }
      this.loading = true;
      this.$Server({
        //url: 'http://localhost:8080/comment/post',
        url: "/comment/post",
        method: "post",
        data: {
          blogId: this.blogId,
          userId: this.$store.state.userId,
          content: this.value
        }
      })
        .then(res => {
          console.log(res, "handle submit res");
          this.getComments(this.blogId);
        })
        .catch(err => {
          console.log(err, "err");
        });
    },
    copyUrl() {
      copy(this.currentUrl);
    },
    handleChange(e) {
      this.value = e.target.value;
    },
    onSelect(option) {
      switch (option.name) {
        case "Facebook":
          window.open(
            "http://www.facebook.com/sharer.php?u=" +
              encodeURIComponent(currentUrl)
          );
          break;
        case "Twitter":
          window.open(
            "http://twitter.com/share?url=" + encodeURIComponent(currentUrl)
          );
          break;
        case "Linkedin":
          window.open(
            "https://www.linkedin.com/shareArticle?url=" +
              encodeURIComponent(currentUrl)
          );
          break;
        case "Copy link":
          this.$toast("You just copied the link.");
          copy(this.currentUrl);
          break;
      }
      this.showShare = false;
      console.log(this.showShare);
    }
  }
};
</script>
<style scoped lang="less">
.detail-container {
  margin: 0 15%;
  @media (max-width: 992px) {
    margin: 0;
    padding: 0;
    display: inline;
  }
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  //overflow: auto;
  overflow: hidden;
}
.van-dropdown-menu {
  position: fixed;
  bottom: 0px;
  z-index: 200;
  width: 100%;
}
.article-body {
  overflow: hidden;
  @media (max-width: 992px) {
    font-size: 12px !important;
  }
  img {
    width: 100% !important;
    height: 100% !important;
  }
}
.tag {
  margin-right: 10px;
  margin-bottom: 3px;
  margin-top: 10px;
}
.normal-content {
  align-items: center;
  text-align: center;
  display: inline-block;
  text-align: left;
  p {
    font-size: 20px;
    line-height: 20px;
    margin-bottom: 20px;
    text-align: left;
  }
}
.related-post {
  padding-top: 3%;
  padding-bottom: 5%;
}
.login-tips {
  font-size: 20px;
  line-height: 20px;
  color: #ac4448;
}
.card-body {
  margin-top: 24px;
  margin-bottom: 60px;
  padding: 60px;
  @media (max-width: 992px) {
    margin: 0;
    padding: 0;
  }
}
.author-description {
  margin-bottom: 60px;
  @media (max-width: 992px) {
    margin-bottom: 10%;
  }
}
.blog-date {
  @media (max-width: 992px) {
    overflow: hidden;
  }
}
.socialmedia {
  font-size: 32px;
  line-height: 32px;
  a-icon {
    margin-right: 10%;
  }
}
.comment {
  @media (max-width: 992px) {
    padding-bottom: 15%;
  }
}
.van-tabbar {
  display: flex;
  justify-content: space-around;
  .tab-item {
    color: #000;
    padding-top: 0.4rem;
    .van-tabbar-item__text {
      font-size: 1.2rem;
    }
  }
}
.van-tabbar-item--active {
  color: #8d050b;
  .tab-item {
    color: #8d050b;
  }
}
.viewsCard {
  font-size: 20px;
  line-height: 20px;
}
</style>
