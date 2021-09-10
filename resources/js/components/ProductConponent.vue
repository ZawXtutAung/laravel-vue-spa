<template>
 <div class="container my-5">
     <div class="row justify-content-end  mb-3">
         
         <div class="col-4">
             <div class="card">
                 <div class="card-header">{{isEditMode ? 'Edit' : 'Create'}}</div>
                 <div class="card-body">
                     <form @submit.prevent="store">
                         <div class="form-group">
                             <label >Name</label>
                             <input v-model="product.name" class="form-control" type="text">
                         </div>
                         <div class="form-group">
                             <label >Price</label>
                             <input v-model="product.price" class="form-control" type="number">
                         </div>
                         <button  type="submit" class="btn btn-primary">Save</button>
                        <!-- <button  type="submit" class="btn btn-primary">Save</button> -->
                     </form>
                 </div>
             </div>
             <div class="col mb-3"></div>
             <div class="col-4">
                 <button class="btn btn-primary" type="submit" @click="created(product)" >Created</button>
             </div>
             
         </div>
         <div class="col-8">
             <table class="table table-">
                 <thead>
                     <tr>
                         <th>Id</th>
                         <th>Name</th>
                         <th>Price</th>
                         <th>Action</th>
                     </tr>
                 </thead>
                 <tbody>
                     <tr v-for="product in products" :key="product.id" >
                         <td>{{product.id}} </td>
                          <td>{{product.name}} </td>
                           <td>{{product.price}}</td>
                            <td>
                                <button class="btn btn-success" @click="edit(product)" >Edit</button>
                                <button class="btn btn-danger" @click="destroy(product.id)" >Delete</button>
                                
                            </td>
                     </tr>
                 </tbody>
             </table>
        </div>
     </div>
 </div>
</template>

<script>
export default {
name: 'ProductConponent',
data(){
return {
    isEditMode:false,
    products:[],
    product:{
        id:'',
        name: '',
        price: ''
    }
} 
},
methods: {
view(){
axios.get('/api/product')
.then(response => {
    this.products=response.data
});
},
store(){
axios.post('/api/product',this.product).
then(response =>{
    this.view();
    this.product={name:'',price:''};
})
},
edit(product){
this.isEditMode=true
 this.product.id=product.id;
 this.product.name=product.name;
 this.product.price=product.price;
},
created(product){
this.isEditMode=false
 this.product.id=''
 this.product.name='';
 this.product.price='';
},
destroy(id){
axios.delete(`/api/product/${id}`)
.then(response =>this.view());
}
},
created(){
    this.view();
    // axios.get('/api/product')
    // .then( response => {
    //    this.products = response.data
    // })
    
}
};
</script>

<style>

</style>