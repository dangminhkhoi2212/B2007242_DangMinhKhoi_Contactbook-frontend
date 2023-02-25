<template>
    <div class="page">
        <h4>Thêm liên hệ</h4>
        <ContactForm :contact="contact" @submit:contact="addContact" />
    </div>
</template>
<script>
import ContactForm from '@/components/ContactForm.vue';
import ContactService from '../services/contact.service';
export default {
    components: {
        ContactForm,
    },
    data() {
        return {
            contact: {},
            message: '',
        };
    },
    methods: {
        async addContact(data) {
            try {
                await ContactService.create(data);
                this.message = 'Đã thêm thành công liên hệ.';
            } catch (error) {
                console.log(error);
            }
        },
    },
};
</script>
