<template>
    <div>
          <h1 class="display-4 text-center">Listado de tareas de casa</h1>
          <hr>
          <div class="row">
              <div class="col-lg-8 offset-lg-2">
                  <div class="card mt-4">
                      <div class="card-body">
                          <div class="input-group">
                              <input type="text"  v-model="tarea" class="form-control form-control-lg" placeholder="agregar tareas">
                              <div class="input-group-append">
                                  <button v-on:click="agregartarea()" class="btn btn-success btn-lg" >
                                      ADD</button>
                              </div>
                          </div>
                          <br>
                          <div class="text-center">
                                <div  v-if="Loading" class="spinner-border text-success" role="status">
                                <span class="visually-hidden">Loading...</span>
                                </div>
                          </div>
                          <h5 v-if=" listtarea.length==0">no hay tareas</h5>
                          
                          <ul v-if="!loading" class="list-group">
                              <li  v-for="(tarea,Index ) of listtarea" :key="Index" class="list-group-item d-flex justify-content-between">
                                  <span class="cursor" v-bind:class="{'text-success': tarea.estado}" 
                                  v-on:click="editartarea(tarea,tarea.id)">
                                      <i v-bind:class="[tarea.estado ? 'fas fa-check-circle': 'far fa-circle']" ></i> 
                                  </span>
                                 {{tarea.nombre}}
                                  <span class="text-danger cursor" v-on:click="eliminartarea(tarea.id)">
                                      <i class="fas fa-trash-alt"></i>
                                  </span>
                              </li>
                          </ul>

                      </div>
                  </div>
              </div>
          </div>
    </div>
</template>

<script>
import axios from "axios";
    
    const Url="https://backend-tareas20220210171245.azurewebsites.net/api/tarea/";
    export default {
        name: 'tarea',
        data(){
            return{
                tarea:'',
                listtarea:[],
                Loading: false 
            }
        },
        methods:{
            agregartarea(){
                const tarea={ 
                    nombre: this.tarea,
                    estado: false
                }
                /*this.listtarea.push(tarea);*/
                this.Loading=true;
                axios.post(Url, tarea).then(response => {
                    console.log(response);
                    this.Loading=false;
                    this.obtenerTarea();
                }).catch(error => {
                    console.error(error);
                    this.Loading=false;
                })
                this.tarea='';
            },
            eliminartarea(id){
                /*this.listtarea.splice(index,1)*/
                this.Loading=true;
                axios.delete(Url+id).then(response=>{
                    console.log(response);
                    this.Loading=false;
                    this.obtenerTarea();
                }).catch(error=>
                {
                    console.log(error);
                    this.Loading=false;
                })
            },
            editartarea(tarea,id){
                /*this.listtarea[index].estado =! tarea.estado*/
                this.Loading=true;
                axios.put(Url+id , tarea).then(()=>{
                    this.obtenerTarea()
                    this.Loading=false
                }).catch(()=> this.Loading=false);


            },
            obtenerTarea(){
                this.Loading=true;
               axios.get(Url).then(response=> 
               {
                   console.log(response);
                   this.listtarea=response.data;
                   this.Loading=false;
               }).catch(()=> this.Loading=false)

            }
        },
        created: function (){
           this.obtenerTarea();
        }
    }
</script>

<style scoped>
.cursor{
    cursor: pointer;
}
</style>