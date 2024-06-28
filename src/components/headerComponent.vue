<template>
  <div class="headers-bar"> 
      <button class="mode-btn" @click="show =!show"> Mode</button>
      <ul class="modelist-ul">
        <li v-for="(mode,Index) in modeList" 
            :key="Index" 
            v-show="show==true"
            @click="selectMode(Index)"
            class="modelist-li">{{ mode }}</li>
      </ul>
<div class="mode-selected-msg">
 <b>{{modeSelected}}</b> 
</div>
<div class="op-btn" @mousemove="operation()">
  <button class="op" :class="{IsAdditon}" @click="IsAdditon =!IsAdditon; IsSub=!IsSub">Addition</button>
  <button  class="op"  :class="{IsSub}" @click="IsAdditon =!IsAdditon; IsSub=!IsSub">Subtraction</button>
</div>
  </div>
</template>

<script lang="ts">
import {Component,Vue} from "vue-property-decorator"

interface IHeaderViewModel{
modeSelected:string;
modeList:string[];
show:boolean;
IsAdditon:boolean;
IsSub:boolean;
}
@Component({
 
    data(): IHeaderViewModel {
  return {
    modeList:["Easy","Medium","Hard"],
    modeSelected:"Easy",
    show:false,
    IsAdditon:true,
    IsSub:false

  };
  } 
})export default class header extends Vue{
private selectMode(id:number){
  const vm = this.$data as IHeaderViewModel;
vm.modeSelected = vm.modeList[id];
this.$emit('informations',vm.modeSelected);
}
private operation()
{
  const vm = this.$data as IHeaderViewModel;
this.$emit('op',vm.IsAdditon); 
}  
}
</script>

<style  scoped>
.headers-bar{
background: var(--top-backgournd);
width: 100%;
position: fixed;
top: 5px;
height: 10%;
}
.mode-btn{
    left: 27px;
    position: fixed;
    top: 15px;
    /* White */
background: var(--top-backgournd);

/* Shadow/16dp */
box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.08), 0px 4px 48px rgba(0, 0, 0, 0.08);
border-radius: 40px;
width: 60px;
height: 30px;

font-family: var(--normal-font);
font-style: normal;
font-weight: 200;
font-size: 14px;
line-height: 117%;
}
.modelist-ul{
  position: absolute;
  width: 100px;
  top: 25px;
  left: 5px;
  background: var( --top-backgournd);
  list-style: none;
}
.modelist-li{
  background:white;
  border-bottom: black;
}
.mode-selected-msg{
  left: 110px;
  top: 18px;
  position: fixed;
  font-size: 18px;
  font-family: var(--black-font);
}
.IsAdditon{
/* Blue/6 */
background: #4D77FF!important;
border-radius: 12px;
width: 85px;
height: 30px;
color: white;
}
.IsSub{
/* Blue/6 */
background: #4D77FF !important;;
border-radius: 12px;
width: 85px;
height: 30px;
color: white;
}
.op-btn{
      right: 27px;
    position: fixed;
    top: 15px;
}
.op{
  font-family: var(--black-font);
font-style: normal;
font-weight: bold;
font-size: 14px;
line-height: 112%;
background: white;
border: none;
}

</style>