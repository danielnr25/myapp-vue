<script setup>
   import {onMounted, ref, watch} from 'vue';
   import Header from './components/Header.vue';
   import Footer from './components/Footer.vue';
   import Product from './components/Product.vue';
   import { database as db } from './data/database';

   const productos = ref([]);
   const carrito = ref([]);
   const cantidadCarrito = ref(0);

   watch(carrito ,()=>{
        guardarLocalStorage();
   },{
    deep:true
   })

    onMounted(()=>{
        productos.value = db;
        const carritoStorage = localStorage.getItem('carrito');
        if(carritoStorage){
            carrito.value = JSON.parse(carritoStorage);
            cantidadCarrito.value = carrito.value.reduce((total,producto) => total + producto.cantidad,0)
        }

    })


    const guardarLocalStorage = () => { 
        localStorage.setItem('carrito',JSON.stringify(carrito.value));
    }

   const agregarCarrito = (producto) =>{
      const existeCarrito = carrito.value.findIndex(prod => prod.id === producto.id)
     
      if(existeCarrito >= 0){
         const producto = carrito.value[existeCarrito];
         producto.cantidad++;
         cantidadCarrito.value++;
         return;
      }else{
         producto.cantidad =1;
         carrito.value.push(producto);
         cantidadCarrito.value++;
      }
      guardarLocalStorage();

   }

   const incrementarCantidad = (id) => {
        const index = carrito.value.findIndex((prod) => prod.id === id)
        if(carrito.value[index].cantidad >= 5) return;
        carrito.value[index].cantidad++;
        cantidadCarrito.value++;
   }

   const decrementarCantidad = (id) =>{
        const index = carrito.value.findIndex((prod) => prod.id === id)
        if(carrito.value[index].cantidad <= 1) return;
        carrito.value[index].cantidad--;
        cantidadCarrito.value--;
   }

   const eliminarProducto = (id) =>{
        const producto = carrito.value.find(prod =>prod.id ===id)
        cantidadCarrito.value -= producto.cantidad;
        carrito.value = carrito.value.filter(prod => prod.id !== id);
   }

   const vaciarCarrito = () => {
    carrito.value = [];
    cantidadCarrito.value = 0;
   }

   const pagarCompra = () => {
    alert('Compra realizada con exito');
    vaciarCarrito();
   }

</script>

<template>
   <div class="container">
       <Header 
            :carrito="carrito"
            :cantidadCarrito="cantidadCarrito"
            @incrementar-cantidad="incrementarCantidad"
            @decrementar-cantidad="decrementarCantidad"
            @eliminar-producto="eliminarProducto"
            @vaciar-carrito="vaciarCarrito"
            @pagar-compra="pagarCompra"
      />

      <div id="lista-productos" class="container">
         <h1 class="titulo">Tienda de productos</h1>
         <div class="row">
            <Product 
                v-for="producto in productos"
                :key = "producto.id"
                :producto="producto"
                @agregar-carrito="agregarCarrito"
            />
         </div>
      </div>

   
      <Footer />
   </div>
</template>


