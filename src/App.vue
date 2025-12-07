<script setup>
import { ref, reactive, onMounted } from 'vue';
import { db } from './data/guitarras';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
    guitarras.value = db;
});

const agregarCarrito = (guitarra) => {
    // Evaluar si el producto ya existe en el carrito
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id); // -1 si no existe, 0 si existe

    // Aumentar la cantidad si el producto ya existe
    if (existeCarrito >= 0) {
        // Aumentar la cantidad del producto
        carrito.value[existeCarrito].cantidad++
    } else {
        // Agregar el producto al carrito
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
}

const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);
    if( carrito.value[index].cantidad === 1) return;
    carrito.value[index].cantidad--;
}

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);
    if( carrito.value[index].cantidad >= 5) return;
    carrito.value[index].cantidad++;
}

</script>

<template>
    <Header
        :carrito="carrito"
        @incrementar-cantidad="incrementarCantidad"
        @decrementar-cantidad="decrementarCantidad"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @agregar-carrito="agregarCarrito" />
        </div>
    </main>

    <Footer />
</template>