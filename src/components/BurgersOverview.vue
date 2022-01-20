<template>
    <div id="burgers-overview">

        <div class="filter-bar">
        <input type="text"
            placeholder="Escribe algo de las hamburguesas para filtrar por id, nombre o ingredientes"
            v-model="filter" />
        <button class="btn btn-primary btn-sm" @click="openAddBurger()">+ Agregar</button> 
        </div>    


        <table class="table table-hover mt-3" :items="burgers" :fields="fields">
            <thead class="thead-dark">
                <tr>
                    <th scope="col">id</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Ingredientes</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="burger in burgers" v-bind:key="burger.id">
                    <td>{{ burger.id }}</td>
                    <td>{{ burger.nombre }}</td>
                    <td>{{ burger.ingredientes.join(", ") }}</td>
                    <td style="width:32%">
                        <button class="btn btn-outline-secondary btn-sm" @click="openModal(burger)">Ver</button>
                        <button class="btn btn-outline-success btn-sm" @click="openEditBurger(burger)">Editar</button>
                        <button class="btn btn-outline-danger btn-sm" @click="openDeleteBurger(burger)">Eliminar</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <!-- Add New Burger Modal -->
        <AddBurger v-show="add_burger_visible" @close="closeAddBurger"></AddBurger>

        <!-- Burger Detail Modal -->
        <Modal v-show="visible" @close="close">
            <template v-slot:header><h3 class="modal-title">Detalles de la Hamburguesa</h3></template>
            <template v-slot:body>
                <div class="row">
                    <div class="col left"><b>id:</b></div>
                    <div class="col right">{{ burger.id }}</div>
                </div>
                <div class="row">
                    <div class="col left"><b>Nombre:</b></div>
                    <div class="col right">{{ burger.nombre }}</div>
                </div>
                <div class="row">
                    <div class="col left"><b>Ingredientes:</b></div>
                    <div class="col right">{{ burger.ingredientes.join(", ") }}</div>
                </div>
                <div class="row">
                    <div class="col left"><b>Calorias:</b></div>
                    <div class="col right">{{ burger.calorias }}</div>
                </div>
            </template>
            <template v-slot:footer></template>
        </Modal>

        <!-- Edit Burger Modal -->
        <Modal v-show="edit_burger_visible" @close="closeEditBurger">
            <template v-slot:header><h3 class="modal-title">Edicion de la Hamburguesa - {{ burger.id }} {{ burger.nombre }}</h3></template>
            <template v-slot:body>
                <form>
                    <div class="form-group row">
                        <label class="col-sm-2 col-form-label"><b>Nombre:</b></label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control-plaintext" placeholder="Nuevo nombre">
                        </div>
                    </div>
                    <div class="form-group row">
                        <label class="col-sm-2 col-form-label"><b>Ingredientes:</b></label>
                        <div class="col-sm-8">
                            <input type="text" class="form-control-plaintext" placeholder="Nuevos ingredientes">
                        </div>
                    </div>
                    <div class="form-group row">
                        <label class="col-sm-2 col-form-label"><b>Calorias:</b></label>
                        <div class="col-sm-8">
                            <input type="number" class="form-control" placeholder="1000">
                        </div>
                    </div>
                </form>
            </template>
            <template v-slot:footer>
                <button class="btn btn-primary btn-sm" @close="closeEditBurger">Aceptar</button>    
            </template>
        </Modal>

        <!-- De;ete Burger Modal -->
        <Modal v-show="delete_burger_visible" @close="closeDeleteBurger">
            <template v-slot:header><h3 class="modal-title">Borrado de la Hamburguesa</h3></template>
            <template v-slot:body>
                <div class="row">
                    <h4 class="col"><b>¿Borrar la hamburguesa {{ burger.id }} {{ burger.nombre }} de la base de datos?</b></h4>
                </div>
            </template>
            <template v-slot:footer>
                <button class="btn btn-danger btn-sm" @close="closeDeleteBurger">Eliminar</button>    
            </template>
        </Modal>

    </div>
</template>

<script>
import AddBurger from '@/components/AddBurger.vue'
import Modal from '@/components/Modal.vue'

export default {
    name: "burgers-overview",
    components: {
        Modal,
        AddBurger,
    },
    data() {    
        return {
            filter:'',
            visible: false,
            add_burger_visible: false,
            edit_burger_visible: false,
            delete_burger_visible: false,
            fields: ['id', 'nombre', 'ingredientes'],
            id: '',
            nombre: '',
            ingredientes: '',
            burgers: Array,
        };
    },
    methods: {
        getBurgersInformation() {
            this.$http.get('https://prueba-hamburguesas.herokuapp.com/burger/')
            .then((response) =>  {this.burgers = response.data; },
            err => console.log(err));
        },
        openModal(burger) {
            this.burger = burger;
            this.visible = true;
        },
        openAddBurger() {
            this.add_burger_visible = true;
        },
        openEditBurger(burger) {
            this.burger = burger;
            this.edit_burger_visible = true;
        },
        openDeleteBurger(burger) {
            this.burger = burger;
            this.delete_burger_visible = true;
        },
        close() {
            this.visible = false;
        },
        closeAddBurger() {
            this.add_burger_visible = false;
        },
        closeEditBurger() {
            this.edit_burger_visible = false;
        },
        closeDeleteBurger() {
            this.delete_burger_visible = false;
        },
    },
    created() {
        console.log('Informacion de hamburguesas obtenida de API');
        this.getBurgersInformation();
    },
    computed: {
        filteredBurgers() {
            return this.burgers.filter(burger => {
                const id = burger.id.toString().toLowerCase();
                const nombre = burger.nombre.toLowerCase();
                const ingredientes = burger.ingredientes.toLowerCase();
                const searchTerm = this.filter.toLowerCase();

                return id.includes(searchTerm) ||
                    nombre.includes(searchTerm) ||
                    ingredientes.include(searchTerm);
            });
        }
    },
}
</script>

<style scoped>
button {
    margin-left:10px;
}

.modal-title {
    margin-top: 2px;
}

.row {
    font-size: 12pt;
    padding-bottom: 3px;
}

.col-form-label {
    width: 20%;
    text-align: left;
    margin-left: 10%;
}

.col {
    float: left;
    padding: 6px;
}

.left {
    width: 25%;
    text-align: left;
    margin-left: 18%;
}

.right {
    width: 75%;
}

.filter-bar {
    margin: 1% 8%;
}

.filter-bar input {
    float: left;
    width: 78%;
}
</style>