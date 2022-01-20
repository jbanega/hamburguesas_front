<template>
    <transition name="modal-fade">
        <div class="modal-backdrop">
            <div
            class="modal"
            role="dialog"
            aria-labelledby="modalTitle"
            aria-describedby="modalDescription">
                <header class="modal-header" id="modalTitle">
                    <slot name="header"></slot>
                    <button
                    type="button"
                    class="btn btn-outline-danger btn-sm"
                    @click="close"
                    aria-label="Close Modal">
                    x
                    </button>
                </header>

                <main class="modal-body" id="modalDescription">
                    <slot name="body"></slot>
                </main>

                <footer class="modal-footer">
                    <slot name="footer"></slot>
                    <button
                    type="button"
                    class="btn btn-secondary btn-sm"
                    @click="close"
                    aria-label="Close Modal">
                    Cerrar
                    </button>
                </footer>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
  name: "Modal",
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>
<style>
.modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    background: #FFFFFF;
    top: 25%;
    left: 25%;
    box-shadow: 2px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    border-radius: 20px;
    width: 50%;
    height: 50%;
}

.modal-fade-enter,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.5s ease;
}
</style>