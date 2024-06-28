<template>
  <div id="app">
    <Header 
    @informations="getModeInformation"
    @op="getOperation"
     />
    <Container 
    :mode="mode"
    :op="isAddition"
    />


  </div>
</template>

<script lang="ts">
import {Component,Vue} from "vue-property-decorator"
import Header from "@/components/headerComponent.vue";
import Container from "@/components/container.vue";


interface IAppViewModel{
  mode:string;
  isAddition:boolean;
  score:number;
  formula:String;
  result:String;
}
@Component({
  components: {  Header, Container}, 
    data(): IAppViewModel {
  return {
  mode:"",
  isAddition:true,
  score:0,
  formula:"",
  result:""
  };
  } 
})
export default class App extends Vue{
private getModeInformation(value:string){
const vm = this.$data as IAppViewModel;
vm.mode = value;
}
private getOperation(value:boolean){
  const vm = this.$data as IAppViewModel;
vm.isAddition = value;
}

}
</script>
<style src="@/assets/basestyle.css"></style>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: #F2F2F7;
  margin-top: 60px;
}
</style>
