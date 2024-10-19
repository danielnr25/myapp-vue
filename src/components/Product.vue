<script setup>
const props = defineProps({
   producto:{
      type:Object,
      required:true
   }
})
// Define emits me permite emitir eventos personalizados
defineEmits(['agregar-carrito'])

</script>

<template>
   <div class="card">
      <img :src="'/public/img/'+producto.imagen+'.jpg'" :alt="producto.nombre">
      <div class="card-header">
         <h3>{{producto.nombre}}</h3>
         $<span>{{ producto.precio }}</span>
         <button 
            @click="$emit('agregar-carrito',producto)"
            class="btn-add agregar-carrito">
            Agregar carrito
         </button>
      </div>
   </div>
</template>


