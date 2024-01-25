<script setup>
  const props = defineProps({
    joya: {
      type: Object,
      required: true,
    },
  });

  defineEmits(["agregar-carrito"]);
</script>
<template>
  <div class="col-lg-6 my-4 px-4 row align-items-center">
    <div class="col-4">
      <img
        class="img-fluid"
        :src="'/img/' + joya.imagen + '.jpg'"
        :alt="'imagen joya' + joya.nombre"
      />
    </div>
    <div class="col-8">
      <h3 class="text-black fs-4 fw-bold text-uppercase">
        {{ joya.nombre }}
      </h3>
      <p>
        {{ joya.descripcion }}
      </p>
      <p class="fw-black text-primary fs-3">{{ joya.precio }} €</p>
      <button
        type="button"
        class="btn btn-dark w-100"
        v-on:click="$emit('agregar-carrito', joya)"
      >
        Agregar al Carrito
      </button>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
