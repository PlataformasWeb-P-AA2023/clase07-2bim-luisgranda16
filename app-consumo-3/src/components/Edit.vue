<template>
    <div class="pt-5">
        <form @submit.prevent="update" method="post">
            <div class="form-group">
                <label for="nombre">Nombre</label>
                <input
                    type="text"
                    class="form-control"
                    id="nombre"
                    v-model="estudiante.nombre"
                    v-validate="'required'"
                    name="nombre"
                    placeholder="Ingrese nombre"
                    :class="{'is-invalid': errors.has('edificio.nombre') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid name.
                </div>
            </div>
            <div class="form-group">
                <label for="direccion">Dirección</label>
                <textarea
                    name="direccion"
                    class="form-control"
                    id="direccion"
                    v-validate="'required'"
                    v-model="edificio.direccion"
                    cols="30"
                    rows="2"
                    :class="{'is-invalid': errors.has('edificio.direccion') && submitted}">
                  </textarea>
                <div class="invalid-feedback">
                    Please provide a valid description.
                </div>
            </div>
            <div class="form-group">
                <label for="ciudad">Ciudad</label>
                <textarea
                    name="ciudad"
                    class="form-control"
                    id="ciudad"
                    v-validate="'required'"
                    v-model="edificio.ciudad"
                    cols="30"
                    rows="2"
                    :class="{'is-invalid': errors.has('edificio.ciudad') && submitted}">
                  </textarea>
                <div class="invalid-feedback">
                    Please provide a valid description.
                </div>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    data() {
        return {
            estudiante: {
                nombre: '',
                direccion: '',
                ciudad: '',
                url: '',
            },
            submitted: false
        }
    },
    mounted() {
        axios.get('http://127.0.0.1:8000/api/estudiantes/' + this.$route.params.id + '/')
            .then( response => {
                console.log(response.data)
                this.edificio = response.data
            });
    },
    methods: {
        update: function (e) {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                }
                axios.put(`http://127.0.0.1:8000/api/estudiantes/${this.edificio.id}/`,
                        this.edificio
                    )
                    .then(response => {
                        this.$router.push('/');
                    })
            });
        }
    },
}
</script>
