<template>

    <Modal v-show="add_burger_visible" @close="close">
        <template v-slot:header><h3 class="modal-title">Datos de Nueva Hamburguesa</h3></template>

        <template v-slot:body>
            <div id="add-burger">
                <form>
                    <div class="form-group row">
                        <label class="col-sm-2 col-form-label"><b>Nombre:</b></label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control-plaintext" placeholder="Americana">
                        </div>
                    </div>

                    <div class="form-group row">
                        <label class="col-sm-2 col-form-label"><b>Ingredientes:</b></label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control-plaintext" placeholder="Carne, tomate, queso, ...">
                        </div>
                    </div>

                    <div class="form-group row">
                        <label class="col-sm-2 col-form-label"><b>Calorias:</b></label>
                        <div class="col-sm-8">
                            <input type="number" class="form-control" placeholder="850">
                        </div>
                    </div>

                </form>
            </div>
        </template>

        <template v-slot:footer>
            <button class="btn btn-primary btn-sm" @click="close">Agregar</button>    
        </template>
    </Modal>
</template>

<script>
import Modal from '@/components/Modal.vue'

export default {
    name: 'addBurger',
    components: {
        Modal,
    },
    data() {
        return {
            burger: {
                id: '',
                nombre: '',
                ingredientes: '',
                calorias: '',
            },
        }
    },
    methods: {
        close() {
            this.$emit("close");
        },
    },
};
</script>

<style scoped>

#add-burger {
    margin: 1% 8%;
}

.modal-title {
    margin-top: 2px;
}

.row {
    font-size: 12pt;
    padding-bottom: 8px;
}

.col-form-label {
    width: 25%;
    text-align: left;
    margin-left: 5%;
}

</style>