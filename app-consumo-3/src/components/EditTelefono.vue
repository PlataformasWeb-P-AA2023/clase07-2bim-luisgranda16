<template>
    <div class="pt-5">
        <form @submit.prevent="create" method="post">
            <div class="form-group">
                <label for="telefono">Numero</label>
                <input
                    type="text"
                    class="form-control"
                    id="telefono"
                    v-model="telefono.telefono"
                    v-validate="'required'"
                    name="telefono"
                    placeholder="Ingres telefono"
                    :class="{'is-invalid': errors.has('telefono.telefono') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid name.
                </div>
            </div>

            <div class="form-group">
                <label for="tipo">Tipo</label>
                <input
                    type="text"
                    class="form-control"
                    id="Tipo"
                    v-model="edificio.tipo"
                    v-validate="'required'"
                    name="tipo"
                    placeholder="Ingrese Tipo"
                    :class="{'is-invalid': errors.has('telefono.tipo') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid apellido.
                </div>
            </div>

            <div class="form-group">
                <label for="estudiante">Estudiante</label>
                <select v-model="telefono.estudiante">
                            <option v-for="e in estudiantesList" :key="e.url" :value="e.url">{{ e.nombre }} {{ e.apellido }}</option>
                        </select>
            </div>

            <button type="submit" class="btn btn-primary">Crear</button>
        </form>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data() {
        return {
            telefono: {
                telefono: '',
                tipo: '',
                estudiante: '',
            },
            estudiantesList: [],
            submitted: false
        }
    },
    mounted() {
        this.getEstudiantesList(),
        axios.get('http://127.0.0.1:8000/api/numerost/' + this.$route.params.id + '/')
            .then( response => {
                console.log(response.data)
                this.telefono = response.data
        });
    },
    methods: {
      getEstudiantesList() {
            axios
            .get('http://127.0.0.1:8000/api/estudiantes/')
            .then(response => {
                this.estudiantesList = response.data
            })
            .catch(error => {
                console.log(error)
            })

        },
        create: function (e) {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                }
                axios.put(`http://127.0.0.1:8000/api/numerost/${this.telefono.id}/`,
                        this.telefono
                    )
                    .then(response => {
                        this.$router.push('/telefonos');
                    })
            });
        }
    },
}
</script>
