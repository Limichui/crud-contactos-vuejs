<template>
    <div class="about">
        <h5 class="mb-5 mt-5">Mis contactos</h5>

        <div>
            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="name-search-text"><i class="bi bi-search me-2"></i>Buscar por nombre: </span>
                    <input type="search" class="form-control" id="name-search"
                        @search="this.toSearch = $event.target.value">
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                        <tr>
                            <th colspan="2">
                                <input type="text" class="form-control" id="name" v-model="newLink.name">
                            </th>
                            <th>
                                <input type="email" class="form-control" id="email" v-model="newLink.email">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="address" v-model="newLink.address">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="phone" v-model="newLink.phone">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="country" v-model="newLink.country">
                            </th>
                            <th>
                                <input type="text" class="form-control" id="city" v-model="newLink.city">
                            </th>
                            <th>
                                <button type="submit" class="btn btn-primary">Agregar</button>
                            </th>
                        </tr>
                        <tr>
                            <th scope="col">ID</th>
                            <th scope="col">Nombre</th>
                            <th scope="col">Correo electrónico</th>
                            <th scope="col">Dirección</th>
                            <th scope="col">Teléfono</th>
                            <th scope="col">País</th>
                            <th scope="col">Ciudad</th>
                            <th>Acciones</th>
                        </tr>
                        </thead>

                        <tbody>
                        <tr v-for="(link, index) in getList()" :key="index">
                            <th scope="row">{{link.id}}</th>
                            <td>{{link.name}}</td>
                            <td>{{link.email}}</td>
                            <td>{{link.address}}</td>
                            <td>{{link.phone}}</td>
                            <td>{{link.country}}</td>
                            <td>{{link.city}}</td>
                            <td>
                                <button type="button" class="btn btn-info btn-sm" @click="edit(index)"><i
                                        class="bi bi-pen"></i></button>

                                <button type="button" class="btn btn-danger btn-sm" @click="remove(index)"><i
                                        class="bi bi-trash3"></i></button>
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

        <!-- Modal Edit -->
        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">

                            <div class="mb-3">
                                <label for="updateName" class="form-label">Nombre</label>
                                <input type="text" v-model="itemSelected.name" class="form-control" id="updateName">
                            </div>
                            <div class="mb-3">
                                <label for="updateEmail" class="form-label">Correo electrónico</label>
                                <input type="email" v-model="itemSelected.email" class="form-control" id="updateEmail">
                            </div>
                            <div class="mb-3">
                                <label for="updateAddress" class="form-label">Dirección</label>
                                <input type="text" v-model="itemSelected.address" class="form-control" id="updateAddress">
                            </div>
                            <div class="mb-3">
                                <label for="updatePhone" class="form-label">Teléfono</label>
                                <input type="text" v-model="itemSelected.phone" class="form-control" id="updatePhone">
                            </div>
                            <div class="mb-3">
                                <label for="updateCountry" class="form-label">País</label>
                                <input type="text" v-model="itemSelected.country" class="form-control" id="updateCountry">
                            </div>
                            <div class="mb-3">
                                <label for="updateCity" class="form-label">City</label>
                                <input type="text" v-model="itemSelected.city" class="form-control" id="updateCity">
                            </div>

                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" too>Cancelar</button>
                            <button type="submit" class="btn btn-primary">Guardar cambios</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

        <!-- Modal Message -->
        <div class="modal fade" id="validationModal" tabindex="-1" aria-labelledby="validationModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="validationModalLabel">{{ modalTitle }}</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
                    </div>
                    <div class="modal-body">
                        {{ modalMessage }}
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Confirm -->
        <div class="modal fade" id="confirmModal" tabindex="-1" aria-labelledby="confirmModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="confirmModalLabel">{{ confirmTitle }}</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
                    </div>
                    <div class="modal-body">
                        {{ confirmMessage }}
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                        <button type="button" class="btn btn-danger" @click="confirmDelete">Eliminar</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                modalTitle: '', 
                modalMessage: '',
                confirmTitle: '', 
                confirmMessage: '', 
                itemToDelete: null, 
                newLink: {
                    id: '',
                    name: '',
                    email: '',
                    address: '',
                    phone: '',
                    country: '',
                    city: ''
                },
                linksArray: [
                    {
                        id: 1,
                        name: "Alice Johnson",
                        email: "alice.johnson@example.com",
                        address: "123 Maple Street",
                        phone: "123-456-7890",
                        country: "USA",
                        city: "New York"
                    },
                    {
                        id: 2,
                        name: "Bob Smith",
                        email: "bob.smith@example.com",
                        address: "456 Oak Avenue",
                        phone: "987-654-3210",
                        country: "Canada",
                        city: "Toronto"
                    },
                    {
                        id: 3,
                        name: "Carol White",
                        email: "carol.white@example.com",
                        address: "789 Pine Road",
                        phone: "555-123-4567",
                        country: "UK",
                        city: "London"
                    },
                    {
                        id: 4,
                        name: "David Brown",
                        email: "david.brown@example.com",
                        address: "321 Elm Street",
                        phone: "444-555-6666",
                        country: "Australia",
                        city: "Sydney"
                    },
                    {
                        id: 5,
                        name: "Emily Davis",
                        email: "emily.davis@example.com",
                        address: "654 Spruce Lane",
                        phone: "333-444-5555",
                        country: "USA",
                        city: "Los Angeles"
                    }
                ],
                itemSelected: null,
                indexSelected: null,
                toFilter: '',
                toSearch: ''
            }
        },
        methods: {
            showModal(title, message) {
                this.modalTitle = title;
                this.modalMessage = message;
                const validationModal = new bootstrap.Modal(document.getElementById('validationModal'));
                validationModal.show();
            },
            showConfirmModal(title, message, index) {
                this.confirmTitle = title;
                this.confirmMessage = message;
                this.itemToDelete = index;

                const confirmModal = new bootstrap.Modal(document.getElementById('confirmModal'));
                confirmModal.show();
            },
            confirmDelete() {
                // Eliminar el elemento seleccionado
                if (this.itemToDelete !== null) {
                    this.linksArray.splice(this.itemToDelete, 1);
                    this.showModal('Confirmación', 'Registro eliminado exitosamente.');
                    this.itemToDelete = null; // Limpiar el índice después de eliminar
                }

                // Cerrar el modal de confirmación
                const modalElement = document.getElementById('confirmModal');
                const modalInstance = bootstrap.Modal.getInstance(modalElement);
                if (modalInstance) modalInstance.hide();
            },

            save() {
                // Validar que los campos no estén vacíos

                if (this.newLink.name && this.newLink.email && this.newLink.address && this.newLink.phone && this.newLink.country && this.newLink.city) {    
                    //Obtener el ultimo Id e incrementar
                    const lastId = this.linksArray.length > 0 
                    ? Math.max(...this.linksArray.map(item => item.id)) 
                    : 0; // Si no hay elementos, el último id será 0.
        
                    this.newLink.id = lastId + 1;
                    
                    // Agregar un nuevo objeto al array
                    this.linksArray.push({...this.newLink});
                    // Limpiar los campos del formulario
                    this.newLink.name = '';
                    this.newLink.email = '';
                    this.newLink.address = '';
                    this.newLink.phone = '';
                    this.newLink.country = '';
                    this.newLink.city = '';

                    this.showModal('Confirmación','Registro creado exitosamente.');
                } else {
                    this.showModal('Error de validación', 'Por favor, completa todos los campos.');
                }
            },
            remove(index) {
                this.showConfirmModal('Confirmación de eliminación', '¿Está seguro de eliminar este ítem?', index);
            },
            edit(index) {
                this.itemSelected = {...this.linksArray[index]};
                this.indexSelected = index;
                const editModal = new bootstrap.Modal(document.getElementById('editModal'));
                editModal.show();
            },
            saveEdit() {
                // Validar que los campos no estén vacíos
                if (this.itemSelected.name && this.itemSelected.email && this.itemSelected.address && this.itemSelected.phone && this.itemSelected.country && this.itemSelected.city) {
                    this.linksArray[this.indexSelected] = {...this.itemSelected};

                    const modalElement = document.getElementById('editModal');
                    const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
                    modalInstance.hide();

                    this.itemSelected = null;
                    this.indexSelected = null;
                    this.showModal('Confirmación', 'Registro actualizado exitosamente.');

                } else {
                    this.showModal('Error de validación', 'Por favor, completa todos los campos.');
                }
            },
            getList() {
                let result = this.linksArray.filter((item) => {
                    if (this.toSearch) {
                        alert(this.toSearch);
                        return item.name.includes(this.toSearch);
                    }
                    return true;
                });
                
                return result.filter((item) => {
                    return true;
                });
                // return this.linksArray;
            }

        }
    }
</script>

<style>
    .btn {
        margin-right: 3px;
    }
    .card{
        overflow-x: auto;
    }

</style>