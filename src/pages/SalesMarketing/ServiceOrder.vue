<template>
<div>
    <b-row class="service_order_row" v-if="currentUser().roles.includes('ROLE_SALES_MARKETING')" style="margin: 0 -15px 0 -7px;">
        <span class = "col-md-4 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">Description</label>
        </span>
        <div class = "col-md-4 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="deskripsi"
                v-model="service_order.deskripsi"
                type="text"
                required
                placeholder="Description"
                >
            </b-form-input>
        </div>

         <span class = "col-md-2 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">Quantity</label>
        </span>
        <div class= "col-md-2 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="quantity"
                v-model="service_order.quantity"
                type="number"
                required
                placeholder="0"
                >
            </b-form-input>
        </div>

         <span class = "col-md-2 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">UOM</label>
        </span>
         <div class= "col-md-2 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="uom"
                v-model="service_order.uom"
                type="text"
                required
                placeholder="UOM"
                >
            </b-form-input>
        </div>

         <span class = "col-md-3 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">Unit Price (IDR)</label>
        </span>

        <div class= "col-md-3 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="pricePerUnit"
                v-model="service_order.pricePerUnit"
                type="number"
                required
                placeholder="0"
                >
            </b-form-input>
        </div>


        <span class = "col-md-1 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none" v-if="currentUser().roles.includes('ROLE_SALES_MARKETING')">
        </span>
        <div class = "col-md-1 col-sm-8 col-xs-8 col-7" v-if="currentUser().roles.includes('ROLE_SALES_MARKETING')" style="margin-bottom: 10px;">
            <b-button @click="$emit('del-service-order', service_order.id_service_order)" variant="danger"><i class="fas fa-trash-alt"></i></b-button>
        </div>

    </b-row>

    <!-- FOR FINANCE -->
    <b-row class="service_order_row" style="margin: 0 -15px 0 -7px;" v-if="currentUser().roles.includes('ROLE_FINANCE')">
        <span class = "col-md-5 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">Description</label>
        </span>
        <div class = "col-md-5 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="deskripsi"
                v-model="service_order.deskripsi"
                type="text"
                required
                placeholder="Description"
                disabled
                >
            </b-form-input>
        </div>

         <span class = "col-md-2 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">Quantity</label>
        </span>

         <div class= "col-md-2 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="quantity"
                v-model="service_order.quantity"
                type="number"
                required
                placeholder="0"
                disabled
                >
            </b-form-input>
        </div>

         <span class = "col-md-2 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">UOM</label>
        </span>

         <div class= "col-md-2 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="uom"
                v-model="service_order.uom"
                type="text"
                required
                placeholder="UOM"
                disabled
                >
            </b-form-input>
        </div>

         <span class = "col-md-3 col-sm-4 col-xs-4 col-5 d-block d-xs-block d-sm-block d-md-none required">
            <label class="label" style="margin-left: -8px;">Unit Price (IDR)</label>
        </span>

        <div class= "col-md-3 col-sm-8 col-xs-8 col-7">
            <b-form-input
                id="pricePerUnit"
                v-model="service_order.pricePerUnit"
                type="number"
                required
                placeholder="0"
                disabled
                >
            </b-form-input>
        </div>

    </b-row>

</div>
</template>


<script>
export default {
    name: "ServiceOrder",
    props: ["service_order"],
    methods: {
        markComplete() {
            this.todo.completed = !this.todo.completed;
        },
        currentUser() {
            return this.$store.state.auth.user;
        },
    }
}
</script>

<style scoped>
.service_order_row{
    margin-bottom: 10px;
    margin-left: 0;
}
.label{
  font-weight: 600;
}
.required label:after {
  content:" *";
  color: red;
}
</style>
