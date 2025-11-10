<!-- src/components/ResultModal.vue -->
<template>
  <transition name="overlay-fade" appear>
    <div v-if="show" class="fixed inset-0 z-50 flex items-center justify-center">
      <!-- Fondo oscuro con aparición progresiva -->
      <div
        class="absolute inset-0 bg-black/50 backdrop-blur-sm transition-opacity duration-500"
      ></div>

      <!-- Contenido del modal -->
      <transition name="modal-pop" appear>
        <div
          v-if="show"
          class="relative bg-white/95 backdrop-blur-sm rounded-2xl shadow-2xl p-6 max-w-lg w-full text-gray-800 transform transition-all duration-300"
          role="dialog"
          aria-modal="true"
        >
          <button
            class="absolute top-3 right-3 text-gray-500 hover:text-gray-700 transition-colors"
            @click="$emit('close')"
          >
            ✕
          </button>

          <h2 class="text-2xl font-bold mb-4 text-indigo-600">
            Certificado encontrado 🎓
          </h2>

          <div v-if="result" class="space-y-2">
            <p>
              <strong>Nombre:</strong>
              <span class="font-medium">
                {{ result.full_names || result.name || result.nombre }}
              </span>
            </p>
            <p>
              <strong>DNI:</strong>
              <span class="font-medium">{{ result.dni || result.DNI }}</span>
            </p>
            <p>
              <strong>Código:</strong>
              <span class="font-medium">
                {{ result.certificate_code || result.code || result.codigo }}
              </span>
            </p>
            <p>
              <strong>Fecha:</strong>
              <span class="font-medium">{{ result.date || result.fecha }}</span>
            </p>

            <a
              v-if="result.url"
              :href="result.url"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-block mt-3 text-blue-600 hover:text-blue-700 underline transition-colors"
            >
              Ver certificado
            </a>
          </div>

          <div v-else class="text-gray-500">Sin datos para mostrar</div>

          <div class="mt-6 text-right">
            <button
              @click="$emit('close')"
              class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded-lg shadow transition-all"
            >
              Cerrar
            </button>
          </div>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script setup>
const props = defineProps({
  show: { type: Boolean, default: false },
  result: { type: Object, default: null },
});
</script>

<style scoped>
/* Transición del fondo oscuro (suave, tipo velo) */
.overlay-fade-enter-active,
.overlay-fade-leave-active {
  transition: opacity 0.6s ease;
}
.overlay-fade-enter-from,
.overlay-fade-leave-to {
  opacity: 0;
}

/* Modal suave sin “zoom fuerte” */
.modal-pop-enter-active {
  transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}
.modal-pop-leave-active {
  transition: all 0.25s ease-in;
}
.modal-pop-enter-from {
  opacity: 0;
  transform: translateY(20px) scale(0.98);
}
.modal-pop-leave-to {
  opacity: 0;
  transform: translateY(10px) scale(0.98);
}
</style>
