<template>
  <div id="app">
    <h1 class="bg-primary text-white text-center p-2">Lista de Tareas de {{usuario}}</h1>
    <div class="container-fluid p-4">

      <div class="row" v-if="filtroTareas.length==0">
        <div class="col text-center">
          <b>Bravo!!! has completado todas tus tareas</b>
        </div>
      </div>
      
      <template v-else>
        <div class="row">
          <div class="col font-weight-bold">Tarea</div>
          <div class="col-2 font-weight-bold">Terminada</div>
        </div>
        <div class="row" v-for="t in filtroTareas" v-bind:key="t.descripcion">
          <div class="col">{{t.descripcion}}</div>
          <div class="col-2 text-center">
            <input type="checkbox" v-model="t.terminada" class="form-check-input" />
            {{t.terminada}}
          </div>
        </div>
      </template>
      
      <div class="row py-2">
        <div class="col">
          <input class="form-control" v-model="nuevoItem">
        </div>
        <div class="col-2">
          <button class="btn btn-primary" v-on:click="addTarea">Agregar</button>
        </div>
      </div>

      <div class="row bg-dark py-2 mt-2 text-white">
        <div class="col text-center">
          <input type="checkbox" class="form-check-input" v-model="banderaCompletadas">
          <label class="form-check-label font-weight-bold">
            Ocultar las tareas completas
          </label>
        </div>

        <div class="col text-center">
          <button class="btn btn-sm btn-warning" v-on:click="deleteTareas">
            Eliminar Tareas Completadas
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data(){
    return{
      usuario: 'Andre',
      tareas:[],
      banderaCompletadas:true,
      nuevoItem:""
    }
  },
  computed:{
    filtroTareas(){
      return this.banderaCompletadas ? this.tareas.filter(t => !t.terminada) : this.tareas;
    }
  },
  methods:{
    addTarea(){
      this.tareas.push(
        {
          descripcion: this.nuevoItem,
          terminada: false
        }
      );
      this.guardarTareas();
      this.nuevoItem = "";
    },
    guardarTareas(){
      localStorage.setItem("tareas", JSON.stringify(this.tareas));
    },
    deleteTareas(){
      this.tareas = this.tareas.filter(t => !t.terminada);
      this.guardarTareas();
    }
  },
  created(){
    let data = localStorage.getItem("tareas");
    if(data != null){
      this.tareas = JSON.parse(data);
    }
  }
};
</script>
