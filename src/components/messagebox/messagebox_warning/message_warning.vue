<template>
  <div class="container-box">
        <!-- message box cảnh báo trước khi xóa -->
        <div v-if="warning" class="wrap-message-form">
            <div class="message-box">
                <div class="div-content">
                    <div class="icon-message">
                        <img src="../../../Resource/img/warning.svg" alt="">
                    </div>
                    <div class="message-content">
                        <div>Bạn có chắc muốn xóa nhân viên "{{employee.employeeName}}"
                            , mã nhân viên là "{{employee.employeeCode}}" không?</div>
                    </div>
                </div>
                <div class="space"></div>
                <div class="mess-footer">
                    <button v-on:click="cancelMessageBox()" ref="btnNo" class="btn-no">Hủy</button>
                    <button v-on:click="deleteEmployee()" ref="btnOk" class="btn-ok">Có</button>
                </div>
            </div>
        </div>
  </div>    
</template>

<script>
import EventBus from '../../../main.js';
export default {
    data(){
        return {
            warning: false,  // ẩn hiện form cảnh báo

            // nhân viên hiển thị trên form warning
            employee : {
              employeeCode : null,
              employeeName : null,
              gender : 0,
              dateOfBirth : null,
              departmentName: null,
              identityDate : null,
              identityPlace : null,
              employeePosition : null,
              address : null,
              bankAccountNumber : null,
              bankName : null,
              bankBranchName : null,
              bankProvinceName : null,
              phoneNumber : null,
              telephoneNumber : null,
              email : null,
            },
        }
    },

    methods: {
        // hàm hiện hộp cảnh báo
        // Createdby TuanNV (18/6/2021)
        showWarning(employee){
            this.employee = {...employee};
            this.warning = true;
        },

        // hàm đóng message box
        // Createdby TuanNV (18/6/2021)
        cancelMessageBox(){
            this.warning = false;
        },

        // hàm emit xóa nhân viên
        // Createdby TuanNV (18/6/2021)
        deleteEmployee(){
            EventBus.$emit("deleteEmployee", this.employee);
            this.warning = false;
        },
    },

    mounted(){
        // lắng nghe sự kiện hiện form cảnh báo
        EventBus.$on("showWarning", (employee) => {
            this.showWarning(employee);
        });
    }
}
</script>

<style src="./messagebox_warning.css"> </style>