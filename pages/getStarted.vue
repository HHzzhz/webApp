<template>
  <a-result
    style="background: #fff"
    status="success"
    title="Registration Successful"
  >
    <template #extra>
      <a-button type="primary">
        <nuxt-link to="/login">Get Started</nuxt-link>
      </a-button>
    </template>
  </a-result>
</template>
<script>
  export default {
    layout(context) {
     return context.isMobile ? 'h5' : 'default';
    },
    created() {
      this.verify();
    },
    data() {
      return {};
    },
    methods: {
      verify() {
        this.$Server({
          url: 'user/email-verify',
          method: 'GET',
          params: {
            token: this.$route.query.token,
            userId: this.$route.query.userId
          }
        }).then(res => {
          if (res.code == 0) {
             this.$message.success(res.msg);
          }
        }).finally(data => {
          this.loading = false;
        });
      }
    }
  };
</script>
