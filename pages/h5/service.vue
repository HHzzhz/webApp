<template>
  <div class="service-container">
    <h1 class="title">Asha Go Services </h1>
    <h1 class="title">(coming soon) </h1>
    <div class="text">
      <p>Asha Go platform aims to address your needs and concerns about living in China. </p>
      <p>We hope to offer services to address your needs ranging from drivers, to cleaners and language instructors.</p>
      <p>What services do you require that would make living in China easier for you?</p>
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
