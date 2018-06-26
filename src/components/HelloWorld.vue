<template>
  <div class="hello">
    <div class="slide">
      <slide></slide>
    </div>
  </div>
</template>

<script>
import slide from './TransformSlide';
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
    }
  },
  components:{
    slide:slide
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.box{
  width:100%;
  height: 120px;
  font-size: #000;
}
</style>
