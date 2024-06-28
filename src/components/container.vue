<template>
  <div class="container" :class="{correct,notCorrect}"> 

   <div class="question">{{formula}}</div> 
   <br>
  <input type="text" value="" class="result-btn" v-model="resp" :disabled="correct == true"> 

  <button class="check-btn" @click="checkAnswer()" v-if="!correct"> OK</button>
  
  <button class="check-btn" @click="nextquestion();questionsID++;" > next</button>

  <div class="pow-screen" v-if="hint==false">
<img src="@/assets/pow.png" alt="" class="pow">
<img src="@/assets/speech-bubble.png" alt="" class="pow-sp">
<div class="pow-txt"> {{currentText}}</div>
  </div>

  <div class="pow-hint" v-if="hint==true">
<img src="@/assets/pow.png" alt="" class="pow">
<img src="@/assets/speech-bubble.png" alt="" class="pow-sp-h">
<div class="pow-txt-h"> {{currentText}} <br> 
<label @click="gotoHint2()" v-if="plus1==true&&plus2==false">...plus</label>
<label @click="getCorrectAnswer();this.hint=true" v-if="plus2==true&&r==false">...plus</label>
</div>


</div>
<div class="hint-container"  v-if="hint==true&& mode !=='Hard'">
 
 <table class="pins1" v-if="plus1==true">
<tr><td v-for="index in firstNumber" :key="index"> <div v-html="HtmlContent"> </div> <br v-if="index%2==0"/> </td></tr>
      
  </table>
<div v-if="op==true">+</div>
<div v-else> -</div>
 <table class="pins2" v-if="plus2==true">
<tr><td v-for="index in secondNumber" :key="index"> <div v-html="HtmlContent" ></div> <br v-if="index%2==0"> </td></tr>

  </table>
<div>= ?</div>

</div>


  <div class="footer">
    <div class="box-score"> <div class="score" >{{score}}/{{questionsID}}</div></div>
     <div class="box-hint" @click="hint=hint=!hint;getHint()"> <div class="hint" >Hint</div></div>

  </div></div>
</template>

<script lang="ts">
import {Component,VModel,Vue} from "vue-property-decorator"

interface IContainerViewModel{
  formula:string;
  max:number;
  result:number;
  firstNumber:number;
  secondNumber:number;
  resp:number| null;
  availableTexts:string[],
  currentText:string,
  correct:boolean | null,
  notCorrect:boolean | null,
  score:number,
  hint:boolean
  HtmlContent:string,
  plus1:boolean,
  plus2:boolean,
  r:boolean,
  questionsID:number
}
@Component({
 
    data(): IContainerViewModel {
  return {
    formula:"",
    max:10,
    result:0,
    firstNumber:0,
    secondNumber:0,
    resp:null,
    availableTexts:
      ["Do you need a hint ?",
      "Do you know the answer ?",
      "If you need help, try the hint!",
      // hint msg
      "How many honey pots can you see ?",
      "Count the honey pots,",
      "How much honey can I eat ?",
      // Correct Answers 
      " Yay! Correct !",
      "Wow! You dit a great job",
      "Keep it up !" ,
      // correction
      "There are xyz honey pots!"
      ],
      currentText:"",
      correct:null,
      notCorrect:null,
      score:0,
      hint:false,
      HtmlContent:`<img src="./pin.png" class="pin" >`,
      plus1:true,
      plus2:false,
      r:true,
      questionsID:0
      
  }
  },
  props:{
    mode: String,
    op: Boolean,
     
  }
})
export default class Container extends Vue{

private gotoHint2(){
  const vm = this.$data as IContainerViewModel;
  this.getCorrectAnswer2();
  vm.hint=true;
  vm.plus2=true;

}
  private nextquestion(){   
    const vm = this.$data as IContainerViewModel;
    vm.correct=null;
    vm.notCorrect=null;
    vm.resp=null;
    vm.hint=false;
     this.generatRandomForumla();
     this.getRandomQuestion();
  }
private getRandomQuestion(){
       const vm = this.$data as IContainerViewModel;

vm.currentText = vm.availableTexts[Math.floor(Math.random()* (3))]
}
private getRandomWrongAnswer(){
   const vm = this.$data as IContainerViewModel;
vm.currentText = vm.availableTexts[Math.floor(Math.random()* (3)+3)]
}private getRandomCorrectAnswer(){
   const vm = this.$data as IContainerViewModel;
vm.currentText = vm.availableTexts[Math.floor(Math.random()* (3)+6)]
}
private getCorrectAnswer(){
   const vm = this.$data as IContainerViewModel;
vm.currentText = vm.availableTexts[9].replace('xxx',vm.result.toString()).replace('xyz',this.doConvert(vm.result));
vm.r = true;
}
private getCorrectAnswer1(){
   const vm = this.$data as IContainerViewModel;
vm.currentText = vm.availableTexts[9].replace('xxx',vm.result.toString()).replace('xyz',this.doConvert(vm.firstNumber));
}
private getCorrectAnswer2(){
   const vm = this.$data as IContainerViewModel;
vm.currentText = vm.availableTexts[9].replace('xxx',vm.result.toString()).replace('xyz',this.doConvert(vm.secondNumber));
vm.r=false;
}
private generatRandomForumla(){
 const vm = this.$data as IContainerViewModel;
 let floor1=10;
  let floor2=10;
  let result = 0;
     vm.plus1=false;
   vm.plus2=false;
   vm.hint=false;
   vm.r=true;
 if (this.$props.mode==="Easy"){
    floor1 = 10;
    floor2 = 10;
    vm.max= 10;
 }
 else if (this.$props.mode==="Medium"){
    floor1 = 20;
    floor2 = 20;
    vm.max = 20;
 }
  else if (this.$props.mode==="Hard"){
    floor1 = 50;
    floor2 = 50;
    vm.max= 50;
 }
 if ( this.$props.op==false){
   do {vm.firstNumber =Math.floor(Math.random()* (floor1)+1);
  vm.secondNumber =Math.floor(Math.random()* (floor2));
  while(vm.secondNumber > vm.firstNumber){
    vm.secondNumber =Math.floor(Math.random()* (floor2));
  }
     vm.result = (this.$props.op==true)? vm.firstNumber + vm.secondNumber: vm.firstNumber - vm.secondNumber;
}
while ( vm.result>  vm.max && vm.firstNumber<vm.secondNumber && vm.result<=0 )

 }else
 {
   do {
 vm.firstNumber =Math.floor(Math.random()* (floor1));
  vm.secondNumber =Math.floor(Math.random()* (floor2));
    vm.result = (this.$props.op==true)? vm.firstNumber + vm.secondNumber: vm.firstNumber - vm.secondNumber;

   }
   while ( vm.result >vm.max && vm.result<=0 ) 
 
 }

 const o = (this.$props.op==true)? "+" : "-";
 vm.formula= vm.firstNumber + o + vm.secondNumber+ "= ?";


}
private doConvert (numberInput:number):string{
    let myDiv=""

    let oneToTwenty = ['','one ','two ','three ','four ', 'five ','six ','seven ','eight ','nine ','ten ',
    'eleven ','twelve ','thirteen ','fourteen ','fifteen ','sixteen ','seventeen ','eighteen ','nineteen '];
    let tenth = ['', '', 'twenty','thirty','forty','fifty', 'sixty','seventy','eighty','ninety'];

if (numberInput <= 20)
myDiv= oneToTwenty[numberInput];
else{
let rightdigit = numberInput/10;
let leftdigit = numberInput-numberInput*10;
if (leftdigit==0){
  myDiv = tenth[rightdigit];
}else
{
    myDiv =tenth[rightdigit]+oneToTwenty[leftdigit];
}

}
    return myDiv
}
private checkAnswer(){
 const vm = this.$data as IContainerViewModel;
if (vm.resp == vm.result ){

  vm.correct= true;
    vm.notCorrect=false;
    this.getRandomCorrectAnswer();
    vm.score++;

}else{
  vm.notCorrect=true;
    vm.correct= false;
    vm.plus1=true;
    this.getCorrectAnswer1();
    //this.getCorrectAnswer();
    vm.hint=true;
    
}
}

private  mounted() {
   this.generatRandomForumla();
   this.getRandomQuestion();
  }
  }
</script>
<style >
.result-btn{
/* White */
background: #FFFFFF;

/* Blue/5 */
border: 1px solid #3363FF;
box-sizing: border-box;
border-radius: 18px;
}
.question{
  /* Nunito Sans / Title 1 */
font-family:var(--black-font);
font-style: normal;
font-weight: bold;
font-size: 60px;
line-height: 120%;

/* or 72px */
text-align: center;
letter-spacing: -0.005em;

/* Gray/1 */
color: #0F0D15;
}
.check-btn{
  background: #FFFFFF;

/* Shadow/16dp */
box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.08), 0px 4px 48px rgba(0, 0, 0, 0.08);
border-radius: 40px;
font-family:var(--black-font);
font-style: normal;
font-weight: 800;
font-size: 20px;
line-height: 117%;

/* identical to box height, or 40px */
align-items: center;
text-align: center;
}
.pow-screen{
position: relative;
position: relative;
top: 100px;
}
.pow-hint{
position: relative;

left: 20%;
width: auto;
}
.pow{
width: 111px;
height: 108px;

}
.pow-sp{
position: relative;
width: 111px;
height: 108px;
bottom: 70px;
}
.pow-sp-h{
position: relative;
width: 111px;
height: 200px;
bottom: 70px;
}
.pow-txt{
  position: relative;
height: 25px;
width: 79px;
left: 50.6%;;
z-index: 9;
bottom: 180px;
font-size: 13px;
}
.pow-txt-h{
  position: relative;
height: 25px;
width: 79px;
left: 50.6%;;
z-index: 9;
bottom: 270px;
font-size: 13px;
}
.container{
  background: #F9F0F3;
  height: 50rem;
}
.correct{
background: #8DC351;
}
.notCorrect{
background: #C910109E;


}
.score{
font-family:var(--black-font);
margin: 50px;
font-size: 25px;
position: relative;
right: 30px;
bottom: 20px;
}
.hint{
 font-family:var(--black-font);
margin: 50px;
position: relative;
right: 20px;
bottom: 17px;
font-size: 25px;

}
.box-hint{
   position: fixed;
  right: 2px;
background: #FFFFFF;
width: 95px;
height: 95px;
text-align: center;
vertical-align: middle;
/* Shadow/16dp */
box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.08), 0px 4px 48px rgba(0, 0, 0, 0.08);
border-radius: 40px;
cursor: hand;
}
.box-score{
   position: fixed;
  left: 2px;
background: #FFFFFF;
width: 95px;
height: 95px;
text-align: center;
vertical-align: middle;
/* Shadow/16dp */
box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.08), 0px 4px 48px rgba(0, 0, 0, 0.08);
border-radius: 40px;
cursor: hand;
}
.footer{

width: 100%;
position: fixed;
bottom: 10px;
height: 20%;


}
.hint-container{
display: inline-flex;
position: inherit;
top: 25%;
font-family:var(--black-font);
font-style: normal;
font-weight: bold;
font-size: 60px;
line-height: 120%;
}
.ping1{
    display: flex;
}
.ping2{
    display: flex;
}
.pin{
margin: 20px;
z-index: 9;
}
</style>