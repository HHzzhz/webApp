<template>
  <div id="comments">
    <van-nav-bar
      title="Post New Comment"
      left-text="Back"
      left-arrow
      @click-left="onBack"
      class="navbar"
    />
    <div class="comment-content">
      <a-comment>
        <div v-if="$store.state.userInfo.avatar" slot="content">
          <div class="comment-avatar">
            <a-avatar
              slot="avatar"
              :src="$store.state.userInfo.avatar || avatarImg"
              style="backgroundColor:#ac4448; margin:5px"
            />
          </div>
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
          <a>
            <nuxt-link to="/login">Login to leave a comment</nuxt-link>
          </a>
        </div>
      </a-comment>
    </div>
  </div>
</template>
<script>
const commentsData = {};
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
      commentsData,
      avatarImg: require("~/assets/img/Asha-Go-dark-circle-logo-no-text.png")
    };
  },
  mounted() {
    this.blogId = this.$route.query.blogId;
  },
  methods: {
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
          console.log(res, "Handle submit res successfuly~");
          if (history.length > 1) {
            history.back();
          }
        })
        .catch(err => {
          console.log(err, "err");
        });
    },
    handleChange(e) {
      this.value = e.target.value;
    },
    onBack() {
      if (history.length > 1) {
        history.back();
      }
    }
  }
};
</script>

<style scoped lang="less">
.login-tips {
  font-size: 18px;
  line-height: 18px;
  color: #ac4448;
  padding-bottom: 20%;
}
.comment-content {
  margin-right: 12px;
  margin-left: 0px;
}
.comment-avatar {
  margin-top: -5px;
  margin-bottom: 12px;
}
</style>