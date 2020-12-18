<template>
  <div class="service-container">
    <h1 class="title">Asha Go Services </h1>
    <h1 class="title">(coming soon) </h1>
    <div class="text">
      <p>Asha Go aims to make your life in China easy and exciting </p>
      <p>We will soon be advertising reliable, competent, and high-quality service providers to address your everyday needs.
          For example: drivers, cleaners, tour guides, and language instructors.
      </p>
      <p>What services do you require?</p>
      <p>Please drop us an email to let us better help you: <b>info@ashago.com</b></p>
    </div>
  </div>
</template>
<script>
export default {
  layout(context) {
    return context.isMobile ? "h5" : "default";
  },
};
</script>
<style lang="less" scoped>
.service-container {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #990000;
  padding: 2rem;
  .title {
    color: #990000;
    font-size: 2rem;
    text-align: center;
  }
  .text {
    margin-top: 2rem;
    line-height: 2.2rem;
    p {
      margin-bottom: 1rem;
    }
  }
}
</style>
