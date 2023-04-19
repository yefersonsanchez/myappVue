<template>
  <div>
    <!--<h1>{{ msg }}</h1>  -->
    <h2>Employee Registation</h2>



    <form @submit.prevent="save"><!--Enviamos el formularios a la funcion save() de este documento Vue-->
      <div class="form-group">
        <label >Employee name</label>
        <input type="text" v-model="employee.nombre"  class="form-control" id="exampleInputEmail1"  placeholder="Enter name employee">
       </div>

       <div class="form-group">
        <label >Employee address</label>
        <input type="text" v-model="employee.address" class="form-control" id="exampleInputEmail1"  placeholder="Enter address employee">
       </div>

       <div class="form-group">
        <label >Mobile</label>
        <input type="text" v-model="employee.mobile" class="form-control" id="exampleInputEmail1"  placeholder="Enter mobile employee">
       </div>
    
      <button type="submit" class="btn btn-primary">Save</button>
    </form>




<br>

    <h2>Employee View</h2>
    <table class="table table-dark">
      <thead>
        <tr>
          <!-- <th scope="col">#</th>
      <th scope="col">First</th>
      <th scope="col">Last</th>
      <th scope="col">Handle</th> -->
          <th scope="col">ID</th>
          <th scope="col">Employee Name</th>
          <th scope="col">Address</th>
          <th scope="col">Mobile</th>
          <th scope="col">Option</th>
        
        </tr>
      </thead>

      <tbody>
        <tr v-for="employee in result" v-bind:key="employee.id">

          <td>{{ employee.id }}</td>
          <td>{{ employee.nombre }}</td>
          <td>{{ employee.address }}</td>
          <td>{{ employee.mobile }}</td>
          <th>
            <button type="button" class="btn btn-warning" @click="edit(employee)">Edit</button>
            <button type="button" class="btn btn-danger"  @click="remove(employee)">Delete</button>
          </th>

        </tr>
      </tbody>

      <!-- <tbody>
        <tr>
          <th scope="row">1</th>
          <td>Mark</td>
          <td>Otto</td>
          <td>@mdo</td>
        </tr>
        <tr>
      <th scope="row">2</th>
      <td>Jacob</td>
      <td>Thornton</td>
      <td>@fat</td>
    </tr>
    <tr>
      <th scope="row">3</th>
      <td>Larry</td>
      <td>the Bird</td>
      <td>@twitter</td>
    </tr>  
      </tbody>-->

    </table>
  </div>
</template>
  
<script>

import Vue from 'vue';
import axios from 'axios';
Vue.use(axios)

export default {
  name: 'EmployeeView',
  data() {
    return {
      // msg: 'Welcome to Your Vue.js App' 
      result: {},
      employee:{
                id: '',
                nombre: '',
                address: '',
                mobile: ''

      }
    }
  },

  created() {
    //this.loadTaks();
    this.EmployeeLoad();
  },

  methods: {
    // loadTaks() {
    //    console.log("LoadTasks() calling....");
    EmployeeLoad() {
      var page = "http://127.0.0.1:8000/api/employees";
      axios.get(page)
        .then(
          ({ data }) => {

            console.log(data);
            this.result = data;
          }


        );
    },




    save() //Recivimos el formulario con los datos del frontend que se van a guardar en la base de datos.
    {
      if(this.employee.id =='')//si el registro id esta vacio o no exite entonces
      {
        this.saveData();// PARA GUARDAR REGISTROS EN LA BASE DE DATOS
      }
      else
      {
        this.updateData();//PARA EDITAR O ACTUALIZAR REGISTROS EN LA BASE DE DATOS
      }

  },
    saveData(){
         axios.post("http://127.0.0.1:8000/api/save",this.employee)
      .then(

      ({data})=>{

        alert("saveddddd");
      }
      )

    },
    edit(employee){

      this.employee= employee;


    },

   
    updateData()
    {
       var editrecords = 'http://127.0.0.1:8000/api/update/'+this.employee.id;
       axios.put(editrecords,this.employee)

       .then(
           ({data})=>{
            this.employee.nombre = '';
            this.employee.address = '',
            this.employee.mobile = '',
            this.employee.id = ''

            alert('updated !!!');
            this.EmployeeLoad();
           }
        );

    },
     remove(employee)
     {
      var url = 'http://127.0.0.1:8000/api/delete/'+employee.id;
      axios.delete(url);
      alert('employee deleted');
      this.EmployeeLoad();

     }


  }



}
</script>