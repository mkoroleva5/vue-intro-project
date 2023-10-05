<script setup lang="ts">
defineProps<{
  onClose: () => void
}>()
</script>

<template>
  <div class="backdrop" @click.self="onClose()">
    <div class="modal">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.backdrop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal {
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #fff;
  border-radius: 10px;
  overflow: hidden;
}
</style>
