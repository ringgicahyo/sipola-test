<template>
    <div class="row">
        <div class = "col-12">
            <b-breadcrumb id="breadcrumb">
                <b-breadcrumb-item :to="{name: 'reimbursement-report'}">
                    Reimbursement Report
                </b-breadcrumb-item>
                <b-breadcrumb-item active>
                    Detail Reimbursement Report
                </b-breadcrumb-item>
            </b-breadcrumb>
            <h3 class="judul"><strong>Detail Reimbursement Report</strong></h3>
            <div class = "row">
            <div class="col-md-10 col-sm-10 col-xs-10 col-12 d-block d-xs-block d-sm-block center">
              <card class="col">
                <b-row>
                    <h5 class = "col-lg-7 col-sm-7 col-6 sub-judul"><strong>Reimbursement Report</strong></h5>                   
                </b-row>
                <b-row class="grup-status">
                      <div class="col-lg-10 col-sm-10 col-md-12 status" >Status</div>
                      <div class="col-lg-10 col-sm-10 col-md-12">
                        <b-badge  v-if="reimbursement.statusReimburse === 1" pill variant="info" size=lg id ="status_reimbursement">
                          On Progress <i class="fas fa-spinner"></i>
                        </b-badge>
                        <b-badge  v-if="reimbursement.statusReimburse === 2" pill variant="warning" size=lg id ="status_reimbursement">
                          Sent <i class="fas fa-paper-plane"></i>
                        </b-badge>
                        <b-badge  v-if="reimbursement.statusReimburse === 3" pill variant="success" size=lg id ="status_reimbursement">
                          Accepted <i class="fas fa-check"></i>
                        </b-badge>
                        <b-badge v-if="reimbursement.statusReimburse === 4" size=lg id ="status_reimbursement" style="background-color:#F89133; color:black">
                          On Revision <i class="fas fa-wrench"></i>
                        </b-badge>
                        <b-badge  v-if="reimbursement.statusReimburse === 5" pill variant="danger" size=lg id ="status_reimbursement">
                          Rejected <i class="fas fa-times"></i>
                        </b-badge>
                      </div>
                </b-row>
                <b-row>
                    <div class = "col-lg-3 col-sm-3 col-6">Created By</div>
                    <div class = "col-lg-5 col-sm-5 col-6">: {{reimbursement.createdBy}}</div>
                </b-row>
                <b-row>
                    <div class = "col-lg-3 col-sm-3 col-6">Created At</div>
                    <div class = "col-lg-5 col-sm-5 col-6">: {{ reimbursement.createdAt.slice(0, 19) | moment('lll')}}</div>
                </b-row>
                <b-row>
                    <div class = "col-lg-3 col-sm-3 col-6">ID Report</div>
                    <div class = "col-lg-5 col-sm-5 col-6">: {{reimbursement.id}}</div>
                </b-row>
                <b-row>
                    <div class = "col-lg-3 col-sm-3 col-6">Project Description</div>
                    <div class = "col-lg-6 col-sm-6 col-6">: {{ reimbursement.projectName}}</div>
                </b-row>
                <b-row>
                    <div class = "col-lg-3 col-sm-3 col-6">Total</div>
                    <div class = "col-lg-5 col-sm-5 col-6">: {{reimbursement.totalReimburse | currency}}</div>
                </b-row>
                <b-row>
                    <div class = "col-lg-5 col-sm-5 col-6"><br>Expense</div>
                    <div class="col-lg-7 col-sm-7 col-6" v-if="reimbursement.statusReimburse == 1 ">
                        <button v-b-modal.modal-send id="send_button" class="btn btn-primary">
                            Send to Finance
                            <i class="fas fa-location-arrow"></i>
                        </button>
                    </div>
                </b-row>

                <b-row>
                    <b-col >
                        <div class="tabel-expense">
                            <div slot="raw-content" class="table-responsive" style="font-size:12px">
                                <b-table
                                :items="reimbursement.listExpense"
                                :fields="fields">
                                <template v-slot:cell(id)="row">
                                    {{reimbursement.listExpense.indexOf(row.item) + 1}}
                                </template>
                                <template v-slot:cell(nominal)="row">
                                    {{row.item.nominal | currency}}
                                </template>
                                 <template v-slot:cell(tanggal)="row">
                                    {{ row.item.tanggal | moment("ll") }}
                                </template>
                                </b-table>

                            </div>
                        </div>
                    </b-col>

                </b-row>

                <div v-if="reimbursement.listAttachment != undefined && reimbursement.listAttachment.length > 0">
                  <b-card class="card-attachment">
                  <b-card-title class="title-attachment" v-b-toggle.collapse-2 style="max-height:50px">
                    <h6>Attachments ( {{reimbursement.listAttachment.length}} )<a @click="downloadAll()" class="download-all"> Download All <i class="fas fa-download"></i></a></h6>
                  </b-card-title>

                  <!-- Element to collapse -->
                  <b-collapse id="collapse-2" v-if="reimbursement.listAttachment != undefined && reimbursement.listAttachment.length > 0">
                    <b-card-body>
                      <b-row>
                        <b-col class="col-xs-12 col-sm-12 col-md-2 grup-attachment" v-bind:key="index" v-for="(file, index) in reimbursement.listAttachment" >
                          <b-img thumbnail fluid v-if="file.type === 'image/jpeg'" :src="untukPreview+file.image" alt="Image" class="image"></b-img>
                          <img v-else thumbnail fluid src="@/assets/img/document.png" alt="Image" class="img-fluid img-thumbnail">
                          <a @click="downloadFile(file)"><i class="fas fa-download"></i></a>
                          <h6> {{file.fileName}}</h6>
                        </b-col>
                      </b-row>
                    </b-card-body>
                  </b-collapse>

                  </b-card>
                </div>

                <b-row v-if="reimbursement.keterangan != undefined && reimbursement.keterangan.length > 0">
                    <div class = "col-lg-3 col-sm-2 col-md-2 col-12"><i class='fas fa-exclamation-triangle' style='color:red'></i>
                      Notes from Finance :
                    </div>
                    <div class = "col-lg-6 col-sm-2 col-md-2 col-12">{{reimbursement.keterangan}}</div>
                </b-row>

                <b-row>
                    <div class="col button-group">
                        <br>
                        <button v-b-modal.modal-delete id ="delete-button" class="btn btn-primary"  v-if="reimbursement.statusReimburse != 2">
                            Delete
                        </button>
                         <router-link :to="{name: 'update-reimbursement'}">
                            <button v-if="reimbursement.edit" id ="edit-button" class="btn btn-primary">
                                Edit
                            </button>
                         </router-link>
                    </div>
                </b-row>
            </card>
            </div>
            </div>
        </div>

        <b-modal
      id="modal-send"
      ref="modal-send"
      centered
      >
      <template v-slot:modal-title>
        <div class="container">
          <h5 id="modal-title-success">Send Reimbursement Report to Finance?</h5>
        </div>
      </template>
      <template v-slot:default>
        <div class="container">
          <b-row>
            <b-col class="modal-icon col-2">
              <img src="@/assets/img/update-confirm-icon.png" alt="" width="50px">
            </b-col>
            <b-col class="col-10">
              <p id="modal-message">You cannot change reimbursement report once you click the send button.</p>
            </b-col>
          </b-row>
        </div>
      </template>
      <template v-slot:modal-footer="{ cancel }">
        <b-col class="button-confirm-group">
          <b-button @click="sentFinance" id="confirm_send_button">
            Send
          </b-button>
          <b-button @click="cancel()" id="cancel_send_button">
            Cancel
          </b-button>
        </b-col>
      </template>
    </b-modal>
    <b-modal
      id="modal-success"
      centered
      ref="success-send"
      v-model="successModalSend"
      @ok="redirectSend()"
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
              <p id="modal-message">Reimbursement Report for {{reimbursement.projectName}} was successfully sent to Finance.</p>
            </b-col>
          </b-row>
        </div>
      </template>
      <template v-slot:modal-footer="{ ok }">
        <b-col class="button-confirm-group">
          <b-button @click="ok()" class="oksend-button">
            OK
          </b-button>
        </b-col>
      </template>
    </b-modal>
    <b-modal
      id="modal-delete"
      ref="modal-delete"
      centered
      >
      <template v-slot:modal-title>
        <div class="container">
          <h5 id="modal-title-delete">Delete Reimbursement Report?</h5>
        </div>
      </template>
      <template v-slot:default>
        <div class="container">
          <b-row>
              <b-col class="modal-icon col-2">
                  <img src="@/assets/img/delete-confirm-icon.png" alt="" width="50px">
              </b-col>
              <b-col class="col-10">
                  <p id="modal-message">Reimbursement report for project {{reimbursement.projectName}} will be removed from the list.</p>
              </b-col>
          </b-row>
        </div>
      </template>
      <template v-slot:modal-footer="{ cancel }">
        <b-col class="button-confirm-group">
          <b-button @click="onSubmit" class ="confirm_delete_button" variant="outline-danger">
            Yes, delete it
          </b-button>
          <b-button @click="cancel()" class="btn btn-danger cancel_delete_button">
            Cancel
          </b-button>
        </b-col>
      </template>
    </b-modal>
    <b-modal
      id="modal-success"
      ref="modal-success"
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
              <p id="modal-message">Reimbursement report for project {{reimbursement.projectName}} was successfully deleted from list.</p>
            </b-col>
          </b-row>
        </div>
      </template>
      <template v-slot:modal-footer="{ ok }">
        <b-col class="button-confirm-group">
          <b-button @click="ok()" class="ok-button">
            OK
          </b-button>
        </b-col>
      </template>
    </b-modal>

    <b-modal title="Failed" v-model="failedModal" centered ok-only>
        Reimbursement was failed to be changed.
    </b-modal>
    </div>
</template>

<script>

import axios from 'axios';
import authHeader from '../../services/auth-header';
import JSZip from 'jszip';

export default {
    data() {
        return {
            previewFile:[],
            reimbursement : {
              createdAt: []
            },
            untukPreview: 'data:image/jpeg;base64, ',
            successModal : false,
            successModalSend : false,
            failedModal : false,
            fields: [
                {key: 'id', label: 'No', sortable: true},
                {key: 'nama', label: 'Expense Item', sortable: true},
                {key: 'tanggal', label: 'Date', sortable: true},
                {key: 'nominal', label: 'Price(IDR)', sortable: true},
            ],
            url_local: "http://localhost:8080/api/reimbursement/",
            url_deploy: "https://sipola-sixab.herokuapp.com/api/reimbursement/",
        };
    },
    beforeMount(){
        this.getDetail();
    },
    methods:{
        onSubmit(evt) {
            evt.preventDefault();
            this.deleteReimbursement(this.reimbursement);
        },

        showMessage(status){
            if(status == 200){
                this.successModal = true;
            }
            else if(status == 500){
                this.failedModal = true;
            }
        },

        getDetail: function(){
            axios.get(this.url_deploy +this.$route.params.id + '/detail', { headers: authHeader() })
            .then(res => {this.reimbursement = res.data})
            .catch(err => this.reimbursement = err.data);
        },

        deleteReimbursement(reimburse){
            axios.delete(this.url_deploy + this.$route.params.id + '/delete', { headers: authHeader() },
            reimburse)
            .then(res => {this.showMessage(res.data.status)});
        },

        redirect(){
            this.$router.push({ name: 'reimbursement-report'});
        },

        hideModal(){
            this.$refs['modal-delete'].hide();
        },

        sentFinance(evt) {
            evt.preventDefault();
            this.sendReimbursement(this.reimbursement);
        },

        hideModalSend(){
            this.$refs['modal-send'].hide();
        },

        sendReimbursement(reimburse){
            axios.put(this.url_deploy + this.$route.params.id + '/send', reimburse, {headers: authHeader()})
            .then(res => {this.reimbursement = res.data.result, this.showMessageSendModal(res.data.status)});
        },

        showMessageSendModal(status){
            if(status == 200){
                this.successModalSend = true;
            }
            else if(status == 500){
                this.failedModal = true;
            }
        },

         downloadFile(file) {
          const linkSource = 'data:' + file.type + ';base64,' + file.image;
          const downloadLink = document.createElement("a");
          const fileName = file.fileName;

          downloadLink.href = linkSource;
          downloadLink.download = fileName;
          downloadLink.click();
        },

        downloadAll() {
          let files = this.reimbursement.listAttachment
          var zip = new JSZip();
          var downloadFile = zip.folder("attachments")
          for (let i = 0; i < files.length; i++) {
            const linkSource = 'data:' + files[i].type + ';base64,' + files[i].image;
            downloadFile.file(files[i].fileName, files[i].image, {base64: true})
          }
          zip.generateAsync({type:"base64"})
          .then(function (content) {
              location.href="data:application/zip;base64,"+content;
          });
        },

        redirectSend(){
            this.hideModalSend();
            this.$refs['success-send'].hide();
        },
  }
};
</script>

<style scoped>
.center{
  margin-left: auto;
  margin-right: auto;
}
#breadcrumb{
  font-size: 12px;
  /* text-decoration: underline; */
  margin: -35px 0 -5px -15px;
  color: #FF3E1D;
  background: none;
}
.label{
  font-weight: 600;
}
.judul{
  text-align: center;
  color: black;
  margin: 11px 0 24px 0;
}
.sub-judul {
  font-size: 16px;
}
#send_button{
    font-size: 12px;
    float:right;
    background-color: green;
    color:white;
    border-color: white;
    margin-bottom: 5px;
}

.ti-download{
    margin-left:10px;
    margin-right: 10px;
}

.fa-warning{
    margin-left:10px;
    margin-right: 10px;
}

.modal-header{
    background-color: #FF3E1D;
}

#edit-button{
  background-color: #109CF1;
  color:white;
  border-color: transparent;
  width: 110px;
  margin-left: 10px;
  line-height: 15px;
  font-size: 12px;
  box-shadow: 3px 3px 15px rgba(16, 156, 241, 0.2);
}

#delete-button{
  background-color: #FF3E1D;
  border-color: #FF3E1D;
  width: 80px;
  font-size: 12px;
  line-height: 15px;
  text-align: center;
}

.button-group{
  margin-top: 20px;
  text-align: center;
  margin-bottom: 10px;
}

#kotakAttachment {
    border: 1px solid gray;
    border-radius: 5px;
    padding: 5px 0 5px 0;
}

.button-confirm-group{
  text-align: right;
}
.confirm_delete_button{
  font-size: 12px;
  width: 110px;
  border-color: #ff3e1d;
  border-width: 1px;
  margin-right: 10px;
  line-height: 15px;
}
.cancel_delete_button{
  font-size: 12px;
  background-color: #ff3e1d;
  color:white;
  border-color: white;
  border-width: 1px;
  line-height: 15px;
}
h5{
  font-weight: bold;
}
#modal-message{
  font-size: 16px;
}
#modal-title-delete{
  color:#FF3E1D;
  font-weight: 1000;
  margin-bottom: -4px;
}
#modal-title-success{
  color: #109CF1;
  font-weight: 1000;
  margin-bottom: -4px;
}
.ok-button{
  color:#109CF1;
  border-color:#109CF1;
  background-color: white;
  font-size: 12px;
  line-height: 15px;
  border-width: 1px;
}

.oksend-button {
  color:#109CF1;
  border-color:#109CF1;
  background-color: white;
  font-size: 12px;
  line-height: 15px;
  border-width: 1px;
}

#cancel_send_button{
  background-color: #109CF1;
  color:white;
  border-color: transparent;
  font-size: 12px;
  margin-left: 10px;
  line-height: 15px;
  width: 110px;
  box-shadow: 3px 3px 15px rgba(16, 156, 241, 0.2);
  text-align: center;
}

#confirm_send_button{
  color:#109CF1;
  border-color:#109CF1;
  background-color: white;
  border-width: 1px;
  width: 80px;
  line-height: 15px;
  text-align: center;
  font-size: 12px;
}

img {
  max-width: 100px;
  max-height: 100px;
}

.grup-attachment {
  padding: 5px 2px 5px 7px;
}

.card-attachment {
  border: solid 1px gray;
}

.grup-status{
  position: absolute;
  right: 16px;
}
.download-all {
  float: right;
  cursor: pointer;
}

@media only screen and (max-width: 768px) {
  /* For mobile phones: */
  .grup-status {
    position: relative;
    float: none;
    right: 0px;
  }
}

</style>
