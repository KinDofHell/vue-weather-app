<template>
  <div class="modal-overlay">
    <div class="modal">
      <h2>Confirmation</h2>
      <p>Are you sure you want to delete this city?</p>
      <button class="confirm-button" @click="confirm">Confirm</button>
      <button class="cancel-button" @click="cancel">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    confirm() {
      this.$emit("confirm");
    },
    cancel() {
      this.$emit("cancel");
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
}

.confirm-button,
.cancel-button {
  margin-top: 10px;
}
</style>
