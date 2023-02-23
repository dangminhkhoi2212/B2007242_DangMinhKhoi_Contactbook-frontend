<template>
    <div v-if="contact" class="page">
        <h4>Hiệu chỉnh Liên hệ</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="updateContact"
            @delete:contact="deleteContact" />
        <p>{{ message }}</p>
    </div>
</template>
<script>
import ContactForm from '@/components/ContactForm.vue';
import ContactServie from '@/services/contact.service';
export default {
    components: {
        ContactForm,
    },
    props: {
        id: {
            type: String,
            required: true,
        },
    },
    data() {
        return {
            contact: null,
            message: '',
        };
    },
    methods: {
        async getContact(id) {
            try {
                this.contact = await ContactServie.get(id);
            } catch (error) {
                console.log(error);
                this.$router.push({
                    name: 'notfound',
                    params: {
                        pathMatch: this.$route.path.split('/').slice(1),
                    },
                    query: this.$route.query,
                    hash: this.$route.hash,
                });
            }
        },
        async updateContact(data) {
            try {
                await ContactServie.update(this.contact._id, data);
                this.message = 'Liên hệ được cập nhật thành công.';
            } catch (error) {
                console.log(error);
            }
        },
        async deleteContact() {
            if (confirm('Bạn muốn xóa Liên hệ này ?')) {
                try {
                    await ContactServie.delete(this.contact._id);
                    alert('Bạn đã xóa thành công');
                    this.$router.push('/');
                } catch (error) {
                    console.log(error);
                }
            }
        },
    },
    created() {
        this.getContact(this.id);
        this.message = '';
    },
};
</script>
