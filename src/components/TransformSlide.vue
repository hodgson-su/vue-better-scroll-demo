<template>
    <div class="swapper" ref="wrapper">
        <div class="scroll" ref="scroll">
            <div class="item" v-for="(item,index) in items" :key="index" :ref="'a'+index"></div>
        </div>
    </div>
</template>
<script>
import BScroll from 'better-scroll'
export default {
    data(){
        return{
            items:[{},{},{}]
        }
    },
    created(){
      this.$nextTick(()=>{
          this._initScroll()
      })
    },
    methods:{
      _initScroll(){
        let dom = this.$refs.scroll.firstElementChild
        let width = dom.offsetWidth + dom.offsetLeft

        let left = (document.body.offsetWidth - dom.offsetWidth) / 2
        
        this.$refs.scroll.style.width = width * this.items.length + left * 2 + 'px'
        this.$refs.a0[0].style.marginLeft = (document.body.offsetWidth - dom.offsetWidth) / 2 + 'px'
        
        this.$refs.a0[0].style.transform = "scale(1)"
        this.scroll = new BScroll(this.$refs.wrapper,{
            click:true,
            probeType:3,
            scrollX:true,
            startX:0,
            momentumLimitTime:20
        })
        let index = 0
        this.scroll.on('scroll',(pos)=>{
            // let x = Math.abs(Math.round(pos.x))
            let sx = Math.round(pos.x)
            index = parseInt(Math.abs(sx) / width)
            this.index = index

            let item_pre,item_current,item_next

            let x = sx + width * index
            let scale1 = Number(0.12 / width * x) + Number(1) // 从大到小
            let scale2 = - Number(0.12 / width * x) + Number(0.88) //从小到大

            item_current = this.$refs['a' + index][0]
            item_current.style.transformOrigin = "100% 50%"
            item_current.style.transform = "scale("+scale1+")"
            if(index == 0){
                item_next = this.$refs['a' + (index+1)][0]
            }else if(index > 0 && index < this.items.length - 1){
                item_pre = this.$refs['a' + (index - 1)][0]
                item_next = this.$refs['a' + (index+1)][0]
            }else{
                item_pre = this.$refs['a' + (index - 1)][0]
            }
            if(item_next){
                item_next.style.transformOrigin = "0 50%"
                item_next.style.transform = "scale("+scale2+")"
            }
        })
        this.scroll.on('scrollEnd',(pos)=>{
            let x = Math.round(pos.x)
            let toX = - index * width
            if(toX - x > width / 2){
                toX = - (index + 1) * width
            }
            if(x !== toX){
                this.scroll.scrollTo(toX,0,200)
            }
        })
      }
    },
}
</script>
<style scoped>
.swapper{
    width:100%;
    height:648px;
    overflow-x:auto;
}
.swapper .scroll{
    width:1400px;
    height: 100%;
}
.swapper .scroll .item{
    width:590px;
    height:648px;
    background: #74C497;
    box-shadow: 0 0 20px 0 rgba(57, 141, 61, 0.25);
    border-radius: 40px;
    float: left;
    margin-left:30px;
    transform: scale(0.88);
    transform-origin:0 50%;
}
</style>


