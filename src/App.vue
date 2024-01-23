<script setup>
  import { ref, reactive, onMounted, watch } from "vue";
  import { db } from "./data/guitarras";
  import Guitarra from "./components/Guitarra.vue";
  import Header from "./components/Header.vue";
  import Footer from "./components/Footer.vue";
  /*Ejemplo con reactive
  const state = reactive({
    guitarras: db,
  });
  console.log(state.guitarras);*/

  /*Ejemplo con ref
  const guitarras = ref(db);
  console.log(guitarras.value);*/

  //Ejemplo State con onMounted, ciclo de vida
  const guitarras = ref([]);
  //Creamos un state nuevo
  const carrito = ref([]);
  //Creamos un state para el model del header, es especifico
  const guitarra = ref({});

  watch(
    carrito,
    () => {
      guardarLocalStorage()
    },
    {
      deep: true,
    }
  );

  onMounted(() => {
    guitarras.value = db;
    guitarra.value = db[3];

    //Agregar el localStorage una vez el componente haya cargado
    const carritoStorage = localStorage.getItem("carrito");
    if (carritoStorage) {
      carrito.value = JSON.parse(carritoStorage);
    }
  });

  //Funcion para almacenar datos en el localStorage
  const guardarLocalStorage = () => {
    localStorage.setItem("carrito", JSON.stringify(carrito.value));
  };

  //Funcion para el botton de agregar carrito
  const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(
      (producto) => producto.id === guitarra.id
    );

    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++;
    } else {
      guitarra.cantidad = 1;
      carrito.value.push(guitarra);
    }

    guardarLocalStorage();
  };
  //Funciiones para aumentar o disminuir articulos del boton en el carrito
  const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex((producto) => producto.id === id);
    if (carrito.value[index].cantidad <= 1) return;
    carrito.value[index].cantidad--;
  };

  const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex((producto) => producto.id === id);
    if (carrito.value[index].cantidad >= 5) return;
    carrito.value[index].cantidad++;
  };

  //función para eliminar producto del carrito
  const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter((producto) => producto.id !== id);
  };

  //función para boton vaciar carrito
  const vaciarCarrito = () => {
    carrito.value = [];
  };
</script>

<template>
  <Header
    :guitarra="guitarra"
    :carrito="carrito"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        v-bind:guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>
  <Footer></Footer>
</template>
