<template>
    <div>
        <h1>Orders</h1>
        <p v-if="loading">Loading orders...</p>
        <div v-else style="display: flex; gap: 1rem;">
            <OrderCategory :orders="pendingOrders" status="PENDING" />
            <OrderCategory :orders="processingOrders" status="PROCESSING" />
            <OrderCategory :orders="completedOrders" status="COMPLETED" />
        </div>
    </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
import OrderCategory from './OrderCategory.vue';
export default {
    name: "ListOrders",
    props: {
        status: {
            type: String,
            required: true
        }
    },
    components: {
        OrderCategory
    },
    computed: {
        ...mapGetters(['allOrders']), 
        loading() {
            return this.$store.getters.isLoading;
        },
        pendingOrders() {
            return Array.isArray(this.allOrders) 
            ? this.allOrders.filter(order => order.status === 'PENDING') 
            : null;
        },
        processingOrders() {
            return Array.isArray(this.allOrders) 
            ? this.allOrders.filter(order => order.status === 'PROCESSING') 
            : null;
        },
        completedOrders() {
            return Array.isArray(this.allOrders) 
            ? this.allOrders.filter(order => order.status === 'COMPLETED') 
            : null;
        }
    },
    created() {
        this.initializeWebSocket();
        this.fetchOrders();
    },
    methods: {
        ...mapActions(['initializeWebSocket', 'getAllOrders']),
        fetchOrders() {
            this.getAllOrders();
        },
    },
}
</script>