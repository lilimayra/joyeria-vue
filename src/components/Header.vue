<script setup>
  import { computed } from "vue";

  const props = defineProps({
    carrito: {
      type: Array,
      required: true,
    },
    joya: {
      type: Object,
      required: true,
    },
  });

  defineEmits([
    "decrementar-cantidad",
    "incrementar-cantidad",
    "agregar-carrito",
    "eliminar-producto",
    "vaciarCarrito",
  ]);

  //Definimos el compudet en una funcion para calcular el total del carrito
  const totalPagar = computed(() => {
    return props.carrito.reduce(
      (total, producto) => total + producto.cantidad * producto.precio,
      0
    );
  });
</script>
<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div
        class="row justify-content-center justify-content-md-between align-items-center"
      >
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo3.svg" alt="imagen logo" />
          </a>
        </div>
        <nav
          class="col-md-6 a mt-5 d-flex align-items-start justify-content-end"
        >
          <div class="carrito">
            <img
              class="img-fluid"
              src="/img/carrito.png"
              alt="imagen carrito"
            />

            <div id="carrito" class="bg-white p-3">
              <!--Creamos una condicion en la que si el carrito es cero entonces muestra el parrafo con la directiva v-if-->
              <p v-if="carrito.length === 0" class="text-center m-0">
                El carrito esta vacio
              </p>
              <div v-else>
                <!--Creamos una condicion en que si el carrito tiene lo contrario a la condicion anterior se muestra esta tabla-->
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Imagen</th>
                      <th>Nombre</th>
                      <th>Precio</th>
                      <th>Cantidad</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <!--Sobre este tr vamos a iterar el array de carrito(el state creado), creamos el objeto temporal (producto)que se va a crear mientras se ejecuta este codigo -->
                    <tr v-for="producto in carrito">
                      <td>
                        <!--Para hacer dinamica una imagen usaremos la directiva v-bin o : -->
                        <img
                          class="img-fluid"
                          :src="'/img/' + producto.imagen + '.jpg'"
                          :alt="'imagen joya' + producto.nombre"
                        />
                      </td>
                      <td>{{ producto.nombre }}</td>
                      <td class="fw-bold">${{ producto.precio }}</td>
                      <td class="flex align-items-start gap-4">
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('decrementar-cantidad', producto.id)"
                        >
                          -
                        </button>
                        {{ producto.cantidad }}
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('incrementar-cantidad', producto.id)"
                        >
                          +
                        </button>
                      </td>
                      <td>
                        <button
                          class="btn btn-danger"
                          type="button"
                          @click="$emit('eliminar-producto', producto.id)"
                        >
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total pagar: <span class="fw-bold">{{ totalPagar }} €</span>
                </p>
                <button
                  class="btn btn-dark w-100 mt-3 p-2"
                  @click="$emit('vaciar-carrito')"
                >
                  Vaciar Carrito
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">Modelo {{ joya.nombre }}</h1>
          <p class="mt-5 fs-5 text-white">
            {{ joya.descripcion }}
          </p>
          <p class="text-primary fs-1 fw-black">{{ joya.precio }} €</p>
          <button
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
            @click="$emit('agregar-carrito', joya)"
          >
            Agregar al Carrito
          </button>
        </div>
      </div>
    </div>

    <img class="header-joya" src="/img/portada.png" alt="imagen header" />
  </header>
</template>
