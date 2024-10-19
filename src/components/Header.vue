<script setup>
import { computed } from 'vue';


const props = defineProps({
    cantidadCarrito: {
        type: Number,
        required: true
    },
    carrito: {
        type: Array,
        required: true
    }
})

defineEmits(['incrementar-cantidad', 'decrementar-cantidad', 'eliminar-producto','vaciar-carrito','pagar-compra']);

// OTRA OPCION: SI DESEAMOS REALIZAR ALGUNA ACCION ESPECIFICA EN MI COMPONENTE, SIEMPRE Y CUANDO SE UTILICE EL EVENTO EN OTROS COMPONENTES

// const emit = defineEmits(['incrementar-cantidad','decrementar-cantidad'])
// function handleClick(id){
//     if(producto != null){
//         emit('incrementar-cantidad',id)
//     }else{
//         emit('decrementar-cantidad',id)
//     }
// }

{/* <button 
class="btn restar"
@click="handleDecrementar(producto.id)"
>
-</button> */} // LLAMADO A LA FUNCION

// function handleDecrementar(id){
//   emit('decrementar-cantidad',id)
// }

// <div v-show="carrito.length > 0">

const totalPagar = computed( () => {
    return props.carrito.reduce((total,producto) =>
    total + (producto.precio * producto.cantidad),0);
})

</script>

<template>
    <nav>

        <a href="https://github.com/danielnr25" target="_blank" class="logo"><span>ISI</span></a>
        <div class="navbar">
            <a href="#" class="btn-carrito">🛒
            </a>
            <span id="cantidad-carrito">
                {{ cantidadCarrito }}
            </span>
            <div id="carrito">
                <p v-if="carrito.length === 0" class="mesanje text-center">
                    El carrito se encuentra vacío
                </p>

                <div v-else>
                    <table class="mostrar" id="lista-carrito">
                        <thead>
                            <tr>
                                <th>Producto</th>
                                <th>Nombre</th>
                                <th>Precio</th>
                                <th>Cantidad</th>
                                <th>Subtotal</th>
                                <th></th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(producto) in carrito">
                                <td>
                                    <img :src="'/public/img/' + producto.imagen + '.jpg'" :alt="producto.nombre">
                                </td>
                                <td>{{ producto.nombre }}</td>
                                <td>${{ producto.precio }}</td>
                                <td>
                                    <button class="btn restar" @click="$emit('decrementar-cantidad', producto.id)">
                                        -</button>
                                    {{ producto.cantidad }}
                                    <button class="btn aumentar" @click="$emit('incrementar-cantidad', producto.id)">
                                        +
                                    </button>

                                </td>
                                <td>${{ producto.precio * producto.cantidad }}</td>
                                <td>
                                    <button class="borrar-producto" @click="$emit('eliminar-producto', producto.id)"
                                        type="button">
                                        X
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                        <tfoot>
                            <tr>
                                <td colspan="4">Total</td>
                                <td id="total-pagar">$ {{ totalPagar }}</td>
                            </tr>
                        </tfoot>
                    </table>
                    <div class="btns-carrito">
                        <button type="button" id="pagar-compra"  @click="$emit('pagar-compra')">Pagar</button>
                        <button type="button" id="vaciar-carrito" @click="$emit('vaciar-carrito')">Vaciar</button>
                    </div>
                </div>
            </div>
        </div>
    </nav>
</template>
