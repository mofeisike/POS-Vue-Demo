<template>
    <div class="leftnav">
      <ul>
        <li><i class="iconfont icon-wode"></i>收银</li>
        <li><i class="iconfont icon-jishufuwu"></i>收银</li>
        <li><i class="iconfont icon-gouwuche"></i>收银</li>
        <li><i class="iconfont icon-caidan"></i>收银</li>
        <li><i class="iconfont icon-qianbao"></i>收银</li>
      </ul>
    </div>
</template>

<script>
    export default {
        name: "LeftNav",
        data() {
            return {}
        },
        components: {},
        computed: {},
        methods: {}
    }
</script>

<style scoped>
  .leftnav{
    color: #ffffff;
    height: 100%;
    background-color: #1d8ce0;
    float: left;
    width: 5%;
  }
  .iconfont{
    font-size: 24px;
  }
  .leftnav ul{
    padding: 0;
    margin: 0;
  }
  .leftnav li{
    list-style: none;
    text-align: center;
    border-bottom: 2px solid rgba(255, 255, 255, 0.5);
    padding: 10px;
  }
  i{
    display: block;
  }

</style>
