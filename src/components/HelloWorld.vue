<template>  
<h1>순간기억력 테스트</h1>
<div class="title">
  <table class="stage_container" style="width: 25%">
      <tbody class="stage">
        <tr v-for="(y, idx) in level" :key="`y-${idx}`">
          <td v-for="(x, index) in 5" :key="`x-${index}`" :id="`${x + (5 * (y -1))}`" class="check" >
             {{ data ? data[x + (5 * (y -1)) - 1] : "" }}
          </td>
        </tr>
      </tbody>
    </table>  
    <table class="stage_container" style="width: 20%; margin-left: 5em;">
      <tbody class="input">
        <tr v-for="(y, idx) in level" :key="`y-${idx}`">
          <td v-for="(x, index) in 5" :key="`x-${index}`" class="check">
            <input v-model="inputData[x + (5 * (y -1)) - 1]" class="check"/>
          </td>
        </tr>
      </tbody>
    </table>  
</div>
<div class="bottom-title">
  <button class="bottom" @click="check">검사</button>
  <button class="bottom" @click="start">시작</button>
  <select v-model="select" class="bottom">
    <option v-for="x in 10" :key="x" :value="x * 1000">{{ x }}초</option>
  </select>
   <select v-model="level" class="bottom" @change="reset">
    <option v-for="x in 5" :key="x" :value="x ">LEVEL {{ x }} </option>
  </select>
</div>

</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data()  {
    return {
      random: ["A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z","1","2","3","4","5","6","7","8","9"],
      inputData: [],
      data: [],
      select: 1000, 
      level: 1,     
    };
  },
  computed: {
   
  },
  mounted() {
    this.init();    
  },
  methods: {
    async init() {      
      await this.randoms();
      await this.delay(Number(this.select));

      for(let i = 1; i <=25; i++) {
        this.colorChange(i.toString(), "black");        
      }
      
    },
    randoms() {
      for(let i = 0; i < 25; i++) {
        let num = Math.floor(Math.random() * 35);
        this.data.push(this.random[num]);
        this.inputData.push("");
      }      
    },
    delay(ms) {                  
      return new Promise(r => setTimeout(r,ms));
    }, 
    async check() {
      let count = 0;
      this.data.forEach((e,i)=> {
        if(e == this.inputData[i]) {
          count++;
        }
      });
      alert(count + "개 맞추셧습니다");

      for(let i = 0; i < this.data.length; i++) {
        let color = "white";
        if(this.data[i] == this.inputData[i]) {
          color = "green";
        }
        await this.delay(250);
        this.colorChange((i + 1).toString(), color);   
      }   
    },
    start() {      
      this.data.forEach((e,i)=> {
        this.colorChange((i + 1).toString(), "white");
      });      

      this.data = [];
      this.inputData = [];

      this.init();
    },
    async colorChange(id, color) {      
      document.getElementById(id).style.backgroundColor = color;    
    },
    reset() {
      
      for(let i = 0; i < 25; i++) {
         this.colorChange((i + 1).toString(), "black");   
      }
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
  display:flex;  
  justify-content: center;  
  margin-top: 5em;
}
.stage {
  margin: 0 auto;
  outline: 2px solid black;
  background-color: white;
}
.stage > tr {
  width: 100%;
  height: 25px;
}
.stage > tr > td {
  width: 25px;
  height: 25px;
  outline: 1px solid black;
}

.input {
  margin: 0 auto;
  outline: 2px solid black;
  background-color: white;
}

.input > tr {
  width: 100%;
  height: 25px;
}
.input > tr > td {
  width: 25px;
  height: 25px;
  outline: 1px solid black;
}

.check {
  width: 2cm;
  height: 2cm; 
  font-size: 20pt;
  text-align: center;
  color: black;
}
.bottom {
  width: 2cm; 
  height: 1.5cm;
  margin: 1em;
}
.boxs {
  width: 3cm;
  height: 3cm;
}

.bottom-title {
  margin-left: 27em;
  margin-top: 1em;
}
</style>
