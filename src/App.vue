<script setup>
  import { ref, reactive, onMounted, watch } from "vue";
  import { db } from "./data/joyas";
  import Joya from "./components/Joya.vue";
  import Header from "./components/Header.vue";
  import Footer from "./components/Footer.vue";
  /*Ejemplo con reactive
  const state = reactive({
    joyas: db,
  });
  console.log(state.joyas);*/

  /*Ejemplo con ref
  const joyas = ref(db);
  console.log(joyas.value);*/

  //Ejemplo State con onMounted, ciclo de vida
  const joyas = ref([]);
  //Creamos un state nuevo
  const carrito = ref([]);
  //Creamos un state para el model del header, es especifico
  const joya = ref({});

  watch(
    carrito,
    () => {
      guardarLocalStorage();
    },
    {
      deep: true,
    }
  );

  onMounted(() => {
    joyas.value = db;
    joya.value = db[3];

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
  const agregarCarrito = (joya) => {
    const existeCarrito = carrito.value.findIndex(
      (producto) => producto.id === joya.id
    );

    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++;
    } else {
      joya.cantidad = 1;
      carrito.value.push(joya);
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
    :joya="joya"
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
      <Joya
        v-for="joya in joyas"
        v-bind:joya="joya"
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>
  <Footer></Footer>
</template>
