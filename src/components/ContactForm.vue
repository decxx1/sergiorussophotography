<template>
    <div class="col-md-6 d-flex">
        <Toaster richColors position="bottom-right" />
        <form id="newsletterForm" method="post" class="bg-light p-5 contact-form" @submit.prevent="handleSubmit">
            <div class="form-group">
                <input type="text" name="nombre" v-model="form.nombre" class="form-control" placeholder="Tu nombre" required>
            </div>
            <div class="form-group">
                <input type="tel" name="telefono" v-model="form.telefono" class="form-control" placeholder="Teléfono" required>
            </div>
            <div class="form-group">
                <textarea name="mensaje" v-model="form.mensaje" cols="30" rows="4" class="form-control" placeholder="Mensaje" required></textarea>
            </div>
            <div class="form-group text-center">
                <input :disabled="isSending" type="submit" value="Enviar Mensaje" class="btn btn-primary py-3 px-5">
            </div>
        </form>
    </div>
</template>

<script setup>
import { Toaster, toast } from 'vue-sonner'
</script>

<script>
import axios from 'axios';

export default {
    data(){
      return {
        isSending:false,
        form:{
            nombre: '',
            telefono: '',
            mensaje: '',
            action: 'subscribe_newsletter',
            token: '',
        }
      }
    },
    methods:{
        
        resetForm(){
            this.form=  {
                nombre: '',
                telefono: '',
                mensaje: '',
                action: 'subscribe_newsletter',
                token: '',
            }
            
        },
        handleSubmit(){
            var self = this;
            if(!this.isSending){
                this.isSending = true;
                grecaptcha.ready(function() {
                    grecaptcha.execute('6LdO_mckAAAAAP0RgsLqjupdrSwP3hMNoxFlyqod', { action: 'subscribe_newsletter' }).then(function(token) {
                        self.form.token = token;
                        //console.log(self.form)
                        self.sendForm();
                    });
                });
            }
        },
        sendForm(){
            var self = this;
            axios.post('https://sergiorussophotography.com.ar/ajax.php', self.form, {
                headers: {
                  'Content-Type': 'multipart/form-data'
                }
            })
            .then(response => {
                toast.success('Tu mensaje fue enviado correctamente')
                self.resetForm()
            })
            .catch(error => {
                console.error(error)
                toast.error('No se pudo enviar el mensaje vuelva a intentarlo más tarde.')
            })
            .finally(() => {
                self.isSending = false
            })
        }
    }
}
</script>

<style>

</style>