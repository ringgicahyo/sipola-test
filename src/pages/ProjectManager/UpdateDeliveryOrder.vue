<template>
  <div>
      <b-breadcrumb id="breadcrumb">
        <b-breadcrumb-item :to="{name: 'delivery-order'}">
          Delivery Order
        </b-breadcrumb-item>
        <b-breadcrumb-item :to="{name: 'detail-delivery-order'}">
          Detail Delivery Order
        </b-breadcrumb-item>
        <b-breadcrumb-item active>
         Update Delivery Order
        </b-breadcrumb-item>
      </b-breadcrumb>
    <h3 class="judul"><strong>Update Delivery Order</strong></h3>
    <div class = "row">
        <div class = "col-md-8 col-sm-8 col-xs-8 col-12 d-block d-xs-block d-sm-block isi-form">
            <card class="col">
            <h5 class = "title-form">Update Delivery Order Form</h5>
             <b-form @submit="onModal" v-if="show">
                <b-row>
                    <div class = "col-md-12 col-12">
                        <b-form-group class="required">
                            <label class="label" for="noDeliveryOrder">Delivery Order No</label>
                            <b-form-input
                                id="noDeliveryOrder"
                                v-model="delivery_order.noDeliveryOrder"
                                type="text"
                                required
                                disabled=""
                                placeholder="Delivery Order Number">
                            </b-form-input>
                        </b-form-group>
                    </div>
                </b-row>
                <b-row>
                     <div class = "col-md-12 col-12">
                        <b-form-group class="required">
                            <label class="label" for="poNumber">Purchase Order No</label>
                            <b-form-input
                                id="poNumber"
                                v-model="delivery_order.poNumber"
                                type="text"
                                required
                                placeholder="Purchase Order Number">
                            </b-form-input>
                        </b-form-group>
                    </div>
                </b-row>


             <b-row>
                     <div class = "col-md-6 col-12">
                        <b-form-group>
                            <label class="label" for="qcPassed">QC Passed / Delivery</label>
                            <b-form-input
                                id="qcPassed"
                                v-model="delivery_order.qcPassed"
                                type="text"
                                placeholder="QC Passed / Delivery">
                            </b-form-input>
                        </b-form-group>
                    </div>

                     <div class = "col-md-6 col-12">
                        <div style="color:black">
                            <b-form-group>
                                <label class="label" for="shipBy">Ship By</label>
                                <b-form-input
                                    id="shipBy"
                                    v-model="delivery_order.shipBy"
                                    type="text"
                                    placeholder="Ship By">
                                </b-form-input>
                            </b-form-group>
                        </div>
                    </div>
             </b-row>

                  <b-row>
                    <b-col md="12">
                        <b-form-group class="required">
                            <label class="label" for="companyName" >Company</label>
                            <b-form-select v-model="delivery_order.company" required>
                                <template slot="companyName">
                                    <option :value="null" disabled>-- Choose Company --</option>
                                </template>
                                <option v-for="company in companies" :key="company.id" :value="company">
                                    {{ company.nama }} - {{ company.alamat }}
                                </option>
                            </b-form-select>
                        </b-form-group>
                    </b-col>
                </b-row>

                <div class="d-none d-md-block d-lg-block">
                    <div class="row" style="margin: 0 -20px -12px -15px;">
                        <div class = "col-md-6 required">
                            <label class="label" >Description</label>
                        </div>
                        <div class = "col-md-2 required">
                            <label class="label" >Quantity</label>
                        </div>
                        <div class = "col-md-3">
                            <label class="label" >Project / Service No</label>
                        </div>
                        <div class = "col-md-1">
                        </div>
                        <br>
                    </div>
                </div>

                <b-row class="products" v-bind:key="item.id_product" v-for="item in products">
                    <b-col>
                        <Product v-bind:product="item" v-on:del-product="deleteRow" />
                    </b-col>
                </b-row>

                <b-row style="margin-top: 2px;">
                    <div class ="col-md-4 col-12">
                        <b-button class="btn btn-primary add-button" @click="addRow()">Add Description <span><img src="@/assets/img/add-circle-blue-icon.png" alt="" width="18px" style="margin-top: -4px;"></span></b-button>
                    </div>
                </b-row>


                <b-form-group>
                    <label class="label" for="termsConditions">Terms and Conditions</label>
                    <ckeditor :editor="editor"  v-model="delivery_order.termsCondition"></ckeditor>
                </b-form-group>

                <div class = "button-group">
                    <b-button class = "save-button" type="submit">Save</b-button>
                    <router-link :to="{name: 'detail-delivery-order'}">
                    <b-button class = "cancel-button">Cancel</b-button>
                    </router-link >
                </div>
            </b-form>
            </card>
        </div>
    </div>

     <b-modal
        id="modal-confirmation"
        centered
        v-model="warningModal"
        >
        <template v-slot:modal-title>
        <div class="container">
            <h5 id="modal-title-success">Save Changes?</h5>
        </div>
        </template>
        <template v-slot:default>
        <div class="container">
            <b-row>
            <b-col class="modal-icon col-2">
                <img src="@/assets/img/update-confirm-icon.png" alt="" width="50px">
            </b-col>
            <b-col class="col-10">
                <p id="modal-message">Delivery order no. {{delivery_order.noDeliveryOrder}} will be changed soon once you click the save button.</p>
            </b-col>
            </b-row>
        </div>
        </template>
        <template v-slot:modal-footer="{ cancel }">
        <b-col class="button-confirm-group">
            <b-button @click="onSubmit" class="save-button">
            Save
            </b-button>
            <b-button @click="cancel()" class="cancel-button">
            Cancel
            </b-button>
        </b-col>
        </template>
    </b-modal>

     <b-modal
        id="modal-success"
        centered
        v-model="successModal"
        @ok="redirect()"
        >
        <template v-slot:modal-title>
            <div class="container">
            <h5 id="modal-title-success">Success!</h5>
            </div>
        </template>
        <template v-slot:default>
            <div class="container">
            <b-row>
                <b-col class="modal-icon col-2">
                <img src="@/assets/img/success-icon.png" alt="" width="50px">
                </b-col>
                <b-col class="col-10">
                <p id="modal-message">Delivery order no. {{delivery_order.noDeliveryOrder}} was successfully changed.</p>
                </b-col>
            </b-row>
            </div>
        </template>
        <template v-slot:modal-footer="{ ok }">
            <b-col class="button-confirm-group">
             <b-button @click="cancel()" class="back-button">
                    Back to List
            </b-button>
            <b-button @click="ok()" class="see-button">
                    See Details
            </b-button>
            </b-col>
        </template>
    </b-modal>

  </div>
</template>

<script>

import Product from '@/pages/ProjectManager/Product.vue';
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
import axios from 'axios';
import authHeader from '../../services/auth-header';

export default {
    components : {
      Product
    },

    data() {
      return {
            editor: ClassicEditor,
            products: [],
            timestamp:"",

            delivery_order : {

            },

            new_product : {
                id_product : 0,
                nama : '',
                quantity : '',
                project_no : '',
            },

            show: true,
            successModal : false,
            failedModal : false,
            warningModal : false,
            send : {objects : null},
            companies: [],
            url_local: 'http://localhost:8080/api/delivery-order/',
            url_deploy: 'https://sipola-sixab.herokuapp.com/api/delivery-order/',
            url_local_company: 'http://localhost:8080/api/company/',
            url_deploy_company: 'https://sipola-sixab.herokuapp.com/api/company/'
        }
    },

    beforeMount() {
        this.getAllCompany();
        this.getDetail();

	},

    methods: {
        addRow(){
            this.new_product.id_product++;
            let product = Object.assign({}, this.new_product);
            this.products.push(product)
        },

        deleteRow(id_product){
            if (this.products.length > 1) {
                this.products = this.products.filter(result => result.id_product !== id_product);
            }
        },

        onModal(evt) {
            this.warningModal = true;
        },

        onSubmit(evt) {
            evt.preventDefault();
            this.delivery_order.product = this.products;
            this.updateDeliveryOrder(this.delivery_order);
        },

        showMessage(status){
            if(status == 200){
                this.successModal = true;
            }
            else if(status == 500){
                this.failedModal = true;
            }
        },

        fetchData : function(){
            let product_dev = this.delivery_order.product
            for(let i=0; i< product_dev.length ; i++){
                this.new_product.id_product++;
                this.new_product.nama = product_dev[i].nama;
                this.new_product.projectNo = product_dev[i].projectNo;
                this.new_product.quantity = product_dev[i].quantity;

                let product = Object.assign({}, this.new_product);
                this.products.push(product);

                this.new_product.nama = ''
                this.new_product.projectNo = ''
                this.new_product.quantity = ''
                this.new_product.delivery_order = ''
            }
        },

        getDetail: function(){
            axios.get(this.url_deploy +this.$route.params.id, { headers: authHeader() })
            .then(res => {this.delivery_order = res.data, this.fetchData()})
            .catch(err => this.delivery_order = err.data);
        },

        getAllCompany: function(){
            axios.get(this.url_deploy_company + 'all', { headers: authHeader() })
            .then(result => this.companies = result.data.result);
        },

        updateDeliveryOrder(quot){
            axios.put( this.url_deploy + 'update/' + this.$route.params.id,
            quot,
                { headers:
                   authHeader()

            })
            .then(res => {this.quotation = res.data.result, this.showMessage(res.data.status)});
        },

       redirect(){
            this.$router.push({ name: 'detail-delivery-order',  params: {id:this.quotation.id}});
        },

        cancel(){
            this.$router.push({ name: 'delivery-order'});
        },

        hideModal(){
		  	this.$refs['modal-hide'].hide();
		},
    }
}
</script>

<style scoped>

.required label:after {
    content: " *";
    color: red;
}
.add-button{
  width: 100%;
  color:#109CF1;
  border-color:#109CF1;
  background-color: white;
  border-width: 2px;
  line-height: 20px;
  text-align: center;
  font-size: 14px;
  margin-bottom: 10px;
  border-radius: 5px;
}
.judul{
  text-align: center;
  color: black;
  margin: 11px 0 24px 0;
}
.title-form {
  font-weight: 600;
  margin-bottom: 20px;
}
.isi-form{
  margin-left: auto;
  margin-right: auto;
}

.save-button{
  background-color: #109CF1;
  color:white;
  border-color: transparent;
  font-size: 12px;
  margin-right: 10px;
  line-height: 15px;
  width: 120px;
  box-shadow: 3px 3px 15px rgba(16, 156, 241, 0.2);
  text-align: center;
}

.cancel-button{
  color:#109CF1;
  border-color:#109CF1;
  background-color: white;
  border-width: 1px;
  width: 80px;
  line-height: 15px;
  text-align: center;
  font-size: 12px;
}

.see-button{
  background-color: #109CF1;
  color:white;
  border-color: transparent;
  font-size: 12px;
  margin-left: 10px;
  line-height: 15px;
  width: 120px;
  box-shadow: 3px 3px 15px rgba(16, 156, 241, 0.2);
  text-align: center;
}

.back-button{
  color:#109CF1;
  border-color:#109CF1;
  background-color: white;
  border-width: 1px;
  width: 100px;
  line-height: 15px;
  text-align: center;
  font-size: 12px;
}

.button-group{
  margin-top: 20px;
  text-align: center;
  margin-bottom: 10px;
}
.label{
  font-weight: 600;
}
#modal-message{
  font-size: 16px;
}
#modal-title-success{
  color: #109CF1;
  font-weight: 1000;
}

.button-confirm-group{
  text-align: right;
}
h5{
  margin-bottom: -4px;
}
#breadcrumb{
  font-size: 12px;
  /* text-decoration: underline; */
  margin: -35px 0 -5px -15px;
  color: #FF3E1D;
  background: none;
}
#termsConditions{
    height: 180px;
}
</style>
