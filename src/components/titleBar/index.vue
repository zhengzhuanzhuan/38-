<template>
  <p class="title-bar"><span></span>{{label}}</p>
</template>
<style lang="less" scoped>
 p {
   color: #fff;
  font-family: PingFangSC;
  font-size: 20px;
  font-weight: 600;
  letter-spacing: 1.4px;
  span {
     width: 6px;
  height: 20px;
  display: inline-block;
  vertical-align: middle;
  margin : 0px 15px 0 5px;
  background-image: linear-gradient(to bottom, #33e6ef, #2e9dff);
  }
 }
</style>
<script>
export default {
  props: ['label']
}
</script>
