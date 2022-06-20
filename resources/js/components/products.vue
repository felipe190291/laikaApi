<template >
   <div  >
       <h1 class="text-center">
           Gestionar productos

       </h1>

      <!-- Button trigger modal -->
<button @click="modificar=false;openModal()" type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#staticBackdrop">
Nuevo producto
</button>

<!-- Modal -->
<div class="modal fade" :class="{mostrar:modal}" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="staticBackdropLabel">{{tituloModal}}</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div>
            <label for="name"> </label>
            <input v-model="articulo.name" type="text" class="form-control" id="name " placeholder="Nombre"/>
            <label for="descripcion"> </label>
            <input v-model="articulo.descripcion" type="text" class="form-control" id="descripcion " placeholder="Descripcion"/>
            <label for="precio"> </label>
            <input v-model="articulo.precio" type="number" class="form-control" id="precio " placeholder="Precio"/>
            <label for="puntuacion"> </label>
            <input v-model="articulo.puntuacion" type="number" class="form-control" id="puntuacion " placeholder="Puntuación"/>
            <label for="gramos"> </label>
            <input v-model="articulo.gramos" type="number" class="form-control" id="gramos " placeholder="Gramos"/>
            <label for="imagen"> </label>
            <input v-model="articulo.imagen" type="text" class="form-control" id="imagen " placeholder="Url imagen"/>
            <label for="descuento"> </label>
            <input v-model="articulo.descuento" type="number" class="form-control" id="descuento " placeholder="Descuento"/>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
        <button @click="guardar_producto()" type="button" class="btn btn-success">Guardar</button>
      </div>
    </div>
  </div>
</div>
       <div class="table-response">
       <table class="table table-dark table-hover align-middle" 
  >
   <thead class="thead-dark text-center">
    <tr>
      <th scope="col">#</th>
      <th scope="col">Nombre</th>
      <th scope="col">descripcion</th>
      <th scope="col">precio</th>
      <th scope="col">puntuacion</th>
      <th scope="col">gramos</th>
      <th scope="col">imagen</th>
      <th scope="col">descuento</th>
      <th scope="col" colspan="2" class="text-center">Accion</th>
    </tr>
  </thead>
  <tbody >
    <tr v-for="product in articulos" :key="product.id">
      <th scope="row">{{product.id}}</th>
      <td >{{product.name}}</td>
      <td >{{product.descripcion}}</td>
      <td >{{product.precio}}</td>
      <td >{{product.puntuacion}}</td>
      <td >{{product.gramos}}</td>
      <td   style="  max-width: 200px; overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;">{{product.imagen}}</td>
      <td >{{product.descuento}}</td>
      <td >
          <button class="btn btn-warning" @click="modificar=true;openModal(product)" data-bs-toggle="modal" data-bs-target="#staticBackdrop">Editar</button>
        
      </td>
      <td >
          
          <button @click="eliminar(product.id)" class="btn btn-danger">Eliminar</button>
      </td>
      
    </tr>
   
   
  </tbody>
</table>
  </div>
   </div>
       
</template>

<script>
    export default {
        data() {
            return {
                articulo:{
                    name:"",
                    descripcion:"",
                    precio:0,
                    puntuacion:0,
                    gramos:0,
                    imagen:"",
                    descuento:0,
                },
                modificar:true,
                tituloModal:"",
                articulos:[],}
        },
        methods:{
async listar(){
    const res= await axios.get("/api/articulos");
    this.articulos =res.data
},
async eliminar(id){
    const res= await axios.delete("/api/articulos/"+id);
    this.listar()
  
},async guardar_producto(id){
if(this.modificar){
const res= await axios.put("/api/articulos/"+this.id,this.articulo);
}else{
const res= await axios.post("/api/articulos",this.articulo);
}

  this.listar();
  var myModal = document.querySelector('.modal-backdrop ')
  var myModal1 = document.getElementById('staticBackdrop')
  myModal.style="display:none"
  myModal1.style="display:none"
},
openModal(data={}){
if(this.modificar){
    this.tituloModal="Modificar producto",
    this.id=data.id,
     this.articulo.name=data.name,
                    this.articulo.descripcion=data.descripcion,
                     this.articulo.precio=data.precio,
                     this.articulo.puntuacion=data.puntuacion,
                    this.articulo.gramos=data.gramos,
                    this.articulo.imagen=data.imagen,
                     this.articulo.descuento=data.descuento
}else{
this.tituloModal="Crear producto",
 this.id=0,
 this.articulo.name="",
                    this.articulo.descripcion="",
                     this.articulo.precio=0,
                     this.articulo.puntuacion=0,
                    this.articulo.gramos=0,
                    this.articulo.imagen="",
                     this.articulo.descuento=0
}
}
        },
        created(){
            this.listar()
        }
    }
   
</script>
