<template>
  <div class="hello">
    <x-header fixed title="新增">
        <!--<router-link to="/" slot="left">-->
          <!--<mt-button icon="back">返回</mt-button>-->
        <!--</router-link>-->
    </x-header>
     <group>
      <x-input title="商户名称" placeholder="请输入商户名称" v-model="store"></x-input>
      <x-input title="负责人姓名" placeholder="请输入姓名" v-model="name"></x-input>
      <x-input title="负责人电话" placeholder="请输入电话" type="tel" v-model="phone"></x-input>
      <x-textarea title="详细地址" :placeholder="location_placeholder" v-model="address"></x-textarea>
      <a class='location-a' @click.native="getlocation">重新定位</a>
     </group>
    <x-button class="submit-btn" type="primary" @click.native="submitData">提交</x-button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      lat:'',
	    lon:'',
      address:'',
      name:'',
      phone:'',
      store:'',
      location_placeholder:'点击定位获取位置',
    }
  },
  mounted(){
    this.getlocation();
  },
  methods:{
    submitData:function(){

    },
    getlocation:function(){
      var map = new AMap.Map('container');
      const self = this;
      map.plugin('AMap.Geolocation', function() {
        var geolocation = new AMap.Geolocation({
          // 是否使用高精度定位，默认：true
          enableHighAccuracy: true,
          // 设置定位超时时间，默认：无穷大
          timeout: 10000
        });
        self.location_placeholder = '正在获取位置信息...';
        geolocation.getCurrentPosition();
        AMap.event.addListener(geolocation, 'complete', onComplete);
        AMap.event.addListener(geolocation, 'error', onError);
        function onComplete (data) {
          // data是具体的定位信息
          console.log(data);
          self.address = data.formattedAddress;
          self.lat = data.position.lat;
          self.lon = data.position.lng;
        }
        function onError (data) {
          // 定位出错
          console.log(data);
          self.location_placeholder = '定位失败请手动输入';
        }
      })
    },
    showPosition:function (position) {
      this.lat = position.coords.latitude;
      this.lon = position.coords.longitude;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.submit-btn {
    float: right;
    font-size: 13px;
    margin-right: 20px;
    margin-top: 60px;
}
.location-a {
  font-size: 13px;
  float: right;
  margin-right: 15px;
  color: #2C8CD9;
}
.weui-cell {
  font-size: 15px;
}

</style>
