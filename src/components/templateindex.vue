<template>
  <div class="pb-3">
      <div class="row">
          <div class="col-lg-5 col-12 border p-4">
            <h2 class="text-success" v-if="this.update==false">Agregar Usuarios</h2>
            <h2 class="text-primary" v-if="this.update==true">Edtiar Usuario</h2>
            <form class="pt-4" @submit="onSubmit">
            <div class="form-group row">
              <label for="name" class="col-sm-2 col-form-label">Nombre:</label>
              <div class="col-sm-10">
                <input type="text" class="form-control" id="name" v-model="name">
              </div>
            </div>
            <div class="form-group row pt-3">
              <label for="email" class="col-sm-2 col-form-label">Email:</label>
              <div class="col-sm-10">
                <input type="email"  class="form-control" id="email" placeholder="email@example.com" v-model="email">
              </div>
            </div>
              <button type="submit" class="btn btn-success mt-4" v-if="this.update==false">Submit</button>
              <button type="button" @click="updateUser()" class="btn btn-primary mt-4" v-if="this.update==true">Editar</button>
               <p v-if="message !=''" v-bind:class="{'text-danger mt-2' : errActive, 'text-success mt-2': !errActive}" >{{message}}</p>
          </form>
          </div>
           <div class="col-lg-6 col-12">
            <table class="table">
              <thead class="thead-dark">
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Nombre</th>
                  <th scope="col">Email</th>
                  <th scope="col">Acciones</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item,index) in arrayUser" :key="item.id">
                  <th scope="row">{{index+1}}</th>
                  <td>{{item.name}}</td>
                  <td>{{item.email}}</td>
                  <td> <button type="button" class="btn btn-danger " @click="deleteUser(item.id)">Eliminar</button>
                  <button type="button" class="btn btn-primary ml-10" @click="getUserById(item.id)">Editar</button></td>
                </tr>
              </tbody>
            </table>
          </div>
      </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
   data(){
    return {
      email:'',
     name:'',
      message:'',
      errActive:false,
      arrayUser:[],
      index:null,
      update:false,
    }
  },
  methods:{
    onSubmit(e){
      e.preventDefault()
      if(this.name==''||this.email==''){
        this.errActive=true;
        this.message='Se require llenar todos los campos'
      }else{
        this.createUser();
        this.cleanForm();
        this.errActive=false
        this.message='Registro exitoso'
      }
    },
    async createUser(){
      try {
        const payload={
          name:this.name,
          email:this.email
        }
         await axios.post("http://localhost:3000/users",payload);
        this.getUser();
      } catch (error) {
          console.log(error);
      }
    },
    async getUser(){
      try {
        const response=await axios.get("http://localhost:3000/users");
        this.arrayUser=response.data;
      } catch (error) {
          console.log(error);
      }
    },
    async deleteUser(id=0){
      try {
        await axios.delete(`http://localhost:3000/users/${id}`);
        this.getUser();
      } catch (error) {
          console.log(error);
      }
    },
    async getUserById(id=0){
      try {
        const response = await axios.get(`http://localhost:3000/users/${id}`);
        if(response.data.length>0){
          this.update=true;
          this.message=''
          this.name=response.data[0].name;
          this.email=response.data[0].email;
          this.index=response.data[0].id;
        }
      } catch (error) {
          console.log(error);
      }
    },
    async updateUser(){
      try {
        const payload={
          name:this.name,
          email:this.email
        }
        if(this.index!=null){
               await axios.patch(`http://localhost:3000/users/${this.index}`,payload);
                this.getUser();
                this.cleanForm();
                this.message='Actulización exitosa'
                this.index=null;
                this.update=false;
              }
            } catch (error) {
              this.update=false;
                console.log(error);
            }
    },
    cleanForm(){
      this.name='';
      this.email='';
    }
  },
  mounted(){
    this.getUser();
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
.table .thead-dark th {
    color: #fff;
    background-color: #212529;
    border-color: #32383e;
}
button{
  width: 100px;
}
.ml-10{
margin-left: 10px;;
}
label{
   font-weight: bold;

}
</style>
