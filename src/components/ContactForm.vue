<template>
    <div class="col-md-6 d-flex">
        <Toaster richColors position="bottom-right" />
        <form id="newsletterForm" method="post" class="bg-light p-5 contact-form" @submit.prevent="handleSubmit">
            <div class="form-group">
                <input type="text" name="name" v-model="form.name" class="form-control" placeholder="Tu Nombre" required>
            </div>
            <div class="form-group">
                <input type="tel" name="phone" v-model="form.phone" class="form-control" placeholder="Teléfono" required>
            </div>
            <div class="form-group">
                <textarea name="message" v-model="form.message" cols="30" rows="4" class="form-control" placeholder="Mensaje" required></textarea>
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
import {
    siteKey,
    secretKey,
    email,
    endPoint
} from '@/services/env.js';
export default {
    data(){
      return {
        isSending:false,
        form:{
            name: '',
            phone: '',
            message: '',
            token: '',
        }
      }
    },
    methods:{
        
        resetForm(){
            this.form=  {
                name: '',
                phone: '',
                message: '',
                token: '',
                secret_key: '',
                addressee: '',
                asunto: ''
            }
            
        },
        handleSubmit(){
            var self = this;
            if(!this.isSending){
                this.isSending = true;
                self.form.secret_key = secretKey;
                self.form.addressee = email;
                self.form.asunto = "Contacto desde la web - de: " + self.form.name;
                grecaptcha.ready(function() {
                    grecaptcha.execute(siteKey, { action: 'contacto' }).then(function(token) {
                        self.form.token = token;
                        //console.log(self.form)
                        self.sendForm();
                    });
                });
            }
        },
        sendForm(){
            var self = this;
            axios.post(endPoint, self.form, {
                headers: {
                  'Content-Type': 'multipart/form-data'
                }
            })
            .then(response => {
                toast.success('Tu mensaje fue enviado correctamente')
                self.resetForm()
            })
            .catch(error => {
                //console.log(error.response.data)
                if (error.response.data.errors) {
                    const formErrors = error.response.data.errors.message;
                    for (let field in formErrors) {
                        if (formErrors.hasOwnProperty(field)) {
                            toast.warning(formErrors[field]);
                            break; // Detener el bucle después de mostrar el primer mensaje de error
                        }
                    }
                }else if(error.response.data.message){
                    toast.error(error.response.data.message)
                }
            })
            .finally(() => {
                self.isSending = false
            })
        }
    }
}
</script>
