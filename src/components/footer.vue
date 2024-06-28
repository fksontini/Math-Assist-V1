<template>
  <div class="footer-bar"> <p> footer</p> </div>
</template>

<script lang='ts'>
import {Component,Vue} from "vue-property-decorator"

interface IFooterViewModel{
modeSelected:string[]
}
@Component({
 
    data(): IFooterViewModel {
  return {
    modeSelected:["Easy","Medium","Hard"]

  };
  } 
})
export default class Footer extends Vue{

  }
</script>

<style scoped>

</style>
