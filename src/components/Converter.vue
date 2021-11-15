<template>
  <div class="hello">
    <div class="row pad-10">
        <div class="col-lg-4"></div>
        <div class="col-lg-4">
            <div class="cstm-panel">
                <div class="cstm-panel-header">
                    <div class="mb-3 text-left title-panel">USD - United States Dollars</div>
                    <div class="row">
                        <label class="col-sm-4 col-form-label text-left">USD</label>
                        <div class="col-sm-8">
                            <input id="amountUsd" class="form-control col-sm-10" type="number" value="10" v-on:keyup.enter="changeData()">
                        </div>
                    </div>
                </div>
                <div class="mb-3">
                    <div v-for="(itemLocal, index) in listLocal" :key="itemLocal.id">
                        <div class="row cstm-item">
                        <div class="col-sm-10 row">
                            <div class="col-sm-4 text-left">{{itemLocal.to}}</div>
                            <div class="col-sm-8 text-right">{{formatPrice(itemLocal.resultNormal * convertValue)}}</div>
                            <div class="col-sm-12 text-left">{{itemLocal.toDetail}}</div>
                            <div class="col-sm-12 text-left">1 {{itemLocal.from}} = {{itemLocal.to}} {{formatPrice(itemLocal.resultNormal)}}</div>
                        </div>

                        <div class="col-sm-2 text-right button-del">
                            <button class="btn btn-danger " @click="deleteItem(index)"><i class="fas fa-trash-alt"></i></button>
                        </div>
                    </div>
                    </div>
                    
                </div>
                <div class="row">
                    <div class="col-sm-12 mb-3">
                        <button class="btn button-color col-sm-12">(+) Add More Currencies</button>
                    </div>
                    <div class="col-sm-9 mb-3">
                        <select id="listCountry" class="form-select" aria-label="Default select example" >
                            <option v-for="comment in comments" :key="comment[0]" selected>
                                {{comment[0]}} - {{comment[1]}}
                            </option>
                            
                        </select>    
                    </div>
                    <div class="col-sm-3 text-right">
                        <button v-on:click="doConvert" class="btn btn-success">Submit</button>
                    </div>
                </div>
                
                
            </div>
        </div>
        <div class="col-lg-4"></div>
    </div>  
  </div>
</template>

<script>
import axios from 'axios';
let url = 'https://apilayer.net/api/list?access_key=78043916d97d6372736025516e52e69a';
export default {
  name: 'Converter',
  props: {
    msg: String
  },
  data(){
    return{
        comments        :[],
        convertValue    :'',
        convertResult   :'', 
        listLocal       :[],
    }
  },
  async mounted(){
      const response = await axios.get(url);
      let listCurrencies = response.data.currencies;
      this.comments = Object.entries(listCurrencies);
      
  },
  methods:{
      doConvert(){
         let getSelectedData    = document.getElementById('listCountry').value; 
         let splitData          = getSelectedData.split(' - ');
         let amountUSD          = document.getElementById('amountUsd').value; 
         //alert(splitData[0]) 
          axios.get('https://api.currencylayer.com/convert', {
                params: {
                    'access_key'    : '78043916d97d6372736025516e52e69a',
                    'from'          : 'USD',
                    'to'            : splitData[0],
                    'amount'        : amountUSD

                }
            }).then(response => {
                let resultConvert = response.data.result;
                this.convertValue = amountUSD;

                this.listLocal.push({
                    from            : 'USD',
                    to              : splitData[0],
                    result          : resultConvert,
                    toDetail        : getSelectedData,
                    resultNormal    : resultConvert/amountUSD,
                })

            }).catch(e => {
                console.log(e)
            })
      },
      deleteItem(index){
         this.listLocal.splice(index, 1); 
      },
      changeData(){
          this.convertValue = document.getElementById('amountUsd').value; 
      },
      formatPrice(value) {
        let val = (value/1).toFixed(2).replace('.', ',')
        return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
.cstm-panel{
    border: solid 1px #f1f1f1;
    border-radius: 5px;
    box-shadow: 0px 5px 10px -5px #444444;
    padding: 10px;
    color: #525252;
    background: white;
}
.cstm-panel-header{
    border-bottom: dashed 1px #c9c9c9;
    padding-bottom: 15px;
}
.pad-10{padding: 10px;}
.text-left{text-align: left;}
.text-right{text-align: right;}
.cstm-item{
    padding: 10px 0px;
    border: solid 1px #eaeaea;
    margin: 10px 0px;
    border-radius: 5px;
    box-shadow: 0px 3px 10px -5px #ababab;
}
.button-del{
    line-height: 65px;
}
.button-color{
    background: #7d7d7d;
    background: rgb(131,108,159);
background: -moz-linear-gradient(128deg, rgba(131,108,159,1) 0%, rgba(117,177,163,1) 53%);
background: -webkit-linear-gradient(128deg, rgba(131,108,159,1) 0%, rgba(117,177,163,1) 53%);
background: linear-gradient(128deg, rgba(131,108,159,1) 0%, rgba(117,177,163,1) 53%);
filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#836c9f",endColorstr="#75b1a3",GradientType=1);
    color: white;
}
.button-color:hover{
    color:white;
}
.title-panel{
    background: #414141;
    padding: 10px;
    border-radius: 5px;
    color: white;
}
</style>
