<template lang="">
    <div>
        <h1>Componente barra de busqueda</h1>
                  <select v-model="selected_datalist_first" style="background-color:white;" >
                             <option v-for="(elements) in datalist_first" :key="elements.val" :value="elements.val" >{{elements.label}}</option>
                         </select>
                           <select v-model="selected_datalist_second" style="background-color:white;" >
                             <option v-for="(elements) in datalist_second" :key="elements.val" :value="elements.val" >{{elements.label}}</option>
                         </select>
                 <input  v-model="search_element" type="text" style="background-color:white;" >
                 <p></p>
                 <div>
                <input  v-model="checkbx" type="checkbox" value="nahuat_orthography" >
                <label for="nahuat_orthography">Activar flexibilidad ortográfica</label>
                <input  v-model="checkbx" type="checkbox"  value="bilingual" >
                <label  for="bilingual">Activar búsqueda bilingüe</label>
                <input  v-model="checkbx" type="checkbox"  value="es_thesaurus_lookup" >
                <label for="es_thesaurus_lookup">Activar tesauro</label>
                    <br>
                    <span>opciones seleccionadas: {{ checkbx }}</span>
                </div>
      <div v-for="(find, index) in extraFilters" :key="index" :v-bind="index" >
          <p>   
              <select v-model="find.exclude" style="background-color:white;" >
                             <option v-for="(elements) in datalist_condition" :key="elements.val" :value="elements.val" >{{elements.label}}</option>
                         </select>
              <select v-model="find.type_tag" style="background-color:white;" >
                             <option v-for="(elements) in datalist_first" :key="elements.val" :value="elements.val" >{{elements.label}}</option>
                         </select>
                           <select v-model="find.filter_type" style="background-color:white;" >
                             <option v-for="(elements) in datalist_second" :key="elements.val" :value="elements.val" >{{elements.label}}</option>
                         </select>
                            <input  v-model="find.value" style="background-color:white;" >
                                    <p>
                                     <input  v-model="find.modifiers[0].name" type="checkbox" value="nahuat_orthography" >
                                     <label for="nahuat_orthography">Activar flexibilidad ortográfica</label>
                                     <input  v-model="find.modifiers[1].name" type="checkbox"  value="bilingual" >
                                    <label  for="bilingual">Activar búsqueda bilingüe</label>
                                    <input  v-model="find.modifiers[2].name" type="checkbox"  value="es_thesaurus_lookup" >
                                     <label for="es_thesaurus_lookup">Activar tesauro</label>
                                     </p>
                            </p>
  </div>
                <p><button style="background-color:blue;" @click="addFilter" >Añadir filtro</button><button style="background-color:red;" @click="deleteFilter" >Eliminar filtro</button></p>
                 <button style="background-color:green"  @click="prueba_axios()" >Search</button>.
                      <h3>demo query build:{{demodata}}</h3>
                 <h1>test data:{{testData}}</h1>
                 <h2>{{axios_response}}</h2>
                 <h3>test extra filters:{{extraFilters}}</h3>
                 <h4>Functions test {{functionTester}}</h4>
            
    </div>
</template>
<script>
export default {
      data(){
    return{
        testData:"",
      axios_response:"",
      selected_datalist_first:"lemma",
      selected_datalist_first_val:"",
      selected_datalist_second:"begins_with",
      selected_datalist_second_val:"",
      search_element:"",
      checkbx:[] ,
      datalist_first:[
          {label:"Entrada",val:"lemma"},
          {label:"Raiz",val:"root"},
          {label:"Campo semántico",val:"category"},
          {label:"Categoría gramatical",val:"part_of_speech"},
          {label:"Inflexion",val:"inflectional_type"},
          {label:"Significado preciso",val:"precise_meaning"},
          {label:"Significado extendido",val:"extended_meaning"},
          {label:"Frase Ilustrativa",val:"illustrative_phrases"},
          {label:"Búsqueda exhaustiva",val:"complete_search"}
          ],
      datalist_second:[
          {label:"empieza con",val:"begins_with"},
          {label:"termina con",val:"ends_with"},
          {label:"contiene secuencia",val:"contains"},
          {label:"contiene palabra",val:"contains_word"},
          {label:"es exactamente igual a",val:"exactly_equals"},
          {label:"expresión regular",val:"regex"},
          ],
        datalist_condition:[
            {label:"Y",val:false},
            {label:"Y no",val:true},
        ],
        demodata:{"dataset":"azz",
        "query":[[{"type_tag":"lemma","filter_type":"begins_with","value":"ojtli","exclude":false,"modifiers":[{"name":"nahuat_orthography"}]}]],"global_modifiers":[]},
        extraFilter:[{"type_tag":"lemma","filter_type":"begins_with","value":"ou","exclude":false,"modifiers":[{"name":"nahuat_orthography"}]},{"type_tag":"lemma","filter_type":"begins_with","value":"s","exclude":false,"modifiers":[]}]
        ,
        extraFilters: [],
        extraChekrs:[{"name":"nahuat_orthography"},{"name":"bilingual" },{"name":"es_thesaurus_lookup" }],
        functionTester:""
    }
  },
  watch:{
     search_element(){
         this.set_values()
     },
     selected_datalist_first(){
         this.set_values()
     },
     selected_datalist_second(){
         this.set_values()
     },
     checkbx(){
         this.set_values()
     }
},
   methods: { 
  async prueba_axios() {
      this.set_values()
    const resp = await this.$axios.$post(process.env.API_HOST,this.demodata)
    this.axios_response = resp
  },
  watchExtraModifTrue(){
      for(let i=0;i<this.extraFilters.length;i++){
                if(this.extraFilters[i].modifiers[0].name===true){
                  this.extraFilters[i].modifiers[0].name="nahuat_orthography"
                  this.functionTester=this.extraFilters[i].modifiers[0].name
                }
                if(this.extraFilters[i].modifiers[1].name===true){
                  this.extraFilters[i].modifiers[1].name="bilingual"
                  this.functionTester=this.extraFilters[i].modifiers[0].name
                }
                 if(this.extraFilters[i].modifiers[2].name===true){
                  this.extraFilters[i].modifiers[2].name="es_thesaurus_lookup"
                  this.functionTester=this.extraFilters[i].modifiers[0].name
                }
          }
  },
  watchExtraModifFalse(){
       for(let i=0;i<this.extraFilters.length;i++){
               if(this.extraFilters[i].modifiers[0].name===false){
                  this.extraFilters[i].modifiers[0].name=""
                  this.functionTester=this.extraFilters[i].modifiers[0].name
                }
                 if(this.extraFilters[i].modifiers[1].name===false){
                  this.extraFilters[i].modifiers[1].name=""
                  this.functionTester=this.extraFilters[i].modifiers[0].name
                }
                 if(this.extraFilters[i].modifiers[2].name===false){
                  this.extraFilters[i].modifiers[2].name=""
                  this.functionTester=this.extraFilters[i].modifiers[0].name
                }
          }
  },
  setChkBox(){
      const newArrJson=[]
      this.checkbx.forEach(element => {
          newArrJson.push({"name":element})
      });
      return newArrJson
  },
  set_values(){
          this.demodata={"dataset":"azz",
        "query":[[{"type_tag":`${this.selected_datalist_first}`,"filter_type":`${this.selected_datalist_second}`,"value":`${this.search_element}`,"exclude":false,"modifiers":this.setChkBox()}]],"global_modifiers":[]}
       // this.demodata.modifiers.push(this.checkbx)
       this.testData = JSON.stringify(Object.assign({},this.checkbx))
      // this.demodata.query[0][0].modifiers.push(JSON.stringify(this.checkbx))
      this.testData = this.setChkBox()
        
      if(this.extraFilters.length>0){
            this.watchExtraModifTrue()
            this.watchExtraModifFalse()
            this.demodata.query.push(this.extraFilters)
      }
      
  },
  addFilter(){
      this.extraFilters.push({"exclude":false ,"value": '',"type_tag":`lemma`, "filter_type":'begins_with', "modifiers":this.extraChekrs});
  },
  deleteFilter(){
      this.extraFilters.pop()
  }

},

}
</script>
<style lang="">
    
</style>