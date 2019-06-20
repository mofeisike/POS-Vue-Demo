<template>
  <div class="home">
    <LeftNav></LeftNav>
    <div class="main" id="main">
      <Pos></Pos>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Pos from '@/components/Pos'
import LeftNav from '@/components/LeftNav'

export default {
  name: 'home',
  components: {
    Pos,
    LeftNav
  }
}
</script>


<style scoped>
  .home{
    height: 700px;
  }
  .main{
    float: left;
    width: 95%;
    background-color: #eff2f7;
    height: 100%;
    overflow: hidden;
  }
</style>
