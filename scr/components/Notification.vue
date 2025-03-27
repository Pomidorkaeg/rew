<template>
  <transition name="fade">
    <div class="notification" :class="type" v-if="isVisible">
      {{ message }}
    </div>
  </transition>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'Notification',
  props: {
    message: {
      type: String,
      required: true
    },
    type: {
      type: String,
      default: 'success' // success или error
    }
  },
  setup(props) {
    const isVisible = ref(false);

    onMounted(() => {
      isVisible.value = true;
      // Скрываем через 3 секунды
      setTimeout(() => {
        isVisible.value = false;
      }, 3000);
    });

    return { isVisible };
  }
};
</script>

<style scoped lang="scss">
.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 15px 25px;
  border-radius: 8px;
  color: white;
  z-index: 1000;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.success {
  background: #28a745; // Зеленый для успеха
}

.error {
  background: #dc3545; // Красный для ошибок
}

/* Анимация появления/исчезновения */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>