<template>
  <div id="community">
    <a-row>
      <a-col :span="24">
        <div style="height:30px"></div>
      </a-col>
    </a-row>
    <a-row>
      <a-col :span="2" />
      <a-col :span="20">
        <div style="height:30px">
          <h1
            style="text-align:left;color:#990000;font-size:30px;line-height:30px;font-family:Arial;"
          >Welcome to Asha Go community!</h1>
        </div>
      </a-col>
      <a-col :span="2" />
    </a-row>

    <a-row>
      <a-col :span="2" />
      <a-col :span="20">
        <div style="height:20px"></div>
        <p
          style="text-align:left;color:#990000;font-size:14px;line-height:15px;font-family:Arial;"
        >Living in a foreign country can be tough. The biggest challenges are loneliness and the lack of a sense of belonging. </p>
        <p
          style="text-align:left;color:#990000;font-size:14px;line-height:15px;font-family:Arial;"
        >However, you are not alone! Join our online support groups and attend our social gatherings.</p>
        <p
          style="text-align:left;color:#990000;font-size:14px;line-height:15px;font-family:Arial;"
        >As always, our aim is to make your life in China easy and exciting!</p>
      </a-col>
      <a-col :span="2" />
    </a-row>

<a-row>
      <a-col :span="24">
        <div style="height:30px"></div>
      </a-col>
    </a-row>

    <a-row>
      <a-col :span="8" />      
      <a-col :span="8">
        <p style="text-align:left;color:#990000;font-size:14px;line-height:15px;font-family:Arial;">
          Scan the QR code to join our Wechat group!
        </p>
        <div
          style="width:100%;height:120px;background:url(https://ashago-resource.oss-cn-zhangjiakou.aliyuncs.com/pic/community/asha4M.jpg)"
        ></div>
      </a-col>
      <a-col :span="8" />
    </a-row>
    <a-row>
      <a-col :span="24">
        <div style="height:60px"></div>
      </a-col>
    </a-row>
  </div>
</template>
<script>
export default {
  layout(context) {
        return context.isMobile ? 'h5' : 'default';
  },
}
</script>
<style>
#community {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #990000;
  margin-top: 10px;
  line-height: 15px;
  font-size: 15px;
}
</style>