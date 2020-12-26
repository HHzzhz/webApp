<template>
  <div id="service">
    <a-row>
      <a-col :span="24">
        <div style="height:60px"></div>
      </a-col>
      </a-row>
    <a-row>
      <a-col :span="4" />
      <a-col :span="16">
        <div style="height:60px">
        <h1 style="color:#990000;font-size:40px;line-height:30px;font-family:Helvetica;">Asha Go Service (coming soon)</h1>
      </div>
      </a-col>
      <a-col :span="4" />
    </a-row>
    <a-row>
      <a-col :span="2" />
      <a-col :span="20">
        <div style="height:40px"></div>
      <p style="color:#990000;font-size:18px;line-height:30px;font-family:Helvetica;">Asha Go aims to make your life in China easy and exciting </p>
      <p style="color:#990000;font-size:18px;line-height:30px;font-family:Helvetica;">We will soon be advertising reliable, competent, and high-quality service providers to address your everyday needs.
          For example: drivers, cleaners, tour guides, and language instructors.
      </p>
      <p style="color:#990000;font-size:18px;line-height:30px;font-family:Helvetica;">What services do you require?</p>
      <p style="color:#990000;font-size:18px;line-height:30px;font-family:Helvetica;">Please drop us an email to let us better help you: <b>info@ashago.com</b></p>

      </a-col>
      <a-col :span="2" />
    </a-row>
    <a-row>
      <a-col :span="24">
        <div style="height:180px"></div>
      </a-col>
      </a-row>
  </div>
</template>
<script>
export default {
  layout(context) {
    return context.isMobile ? 'h5' : 'default';
  },
  asyncData(context) {
     if (context.isMobile) {
       context.redirect('/h5/service')
     }
  },
}
</script>

<style>
#service {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #990000;
  margin-top: 10px;
  line-height: 15px;
  font-size: 15px;
}
</style>
