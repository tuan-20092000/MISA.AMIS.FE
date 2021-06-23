<template>
    <div class="container-box">
        <!-- message cảnh báo dữ liệu đã thay đổi tới người dùng trước khi đóng form detail -->
        <div v-if="warningDataChange" class="wrap-message-form">
            <div class="message-box">
                <div class="div-content">
                    <div class="icon-message">
                        <img src="../../../Resource/img/warning.svg" alt="">
                    </div>
                    <div class="message-content">
                        <div>Dữ liệu đã thay đổi, bạn có chắc muốn đóng không?</div>
                    </div>
                </div>
                <div class="space"></div>
                <div class="mess-footer">
                    <button v-on:click="()=>{this.warningDataChange = false}" ref="btnNo" class="btn-no">Hủy</button>
                    <button v-on:click="cancelFormDetail()" ref="btnOk" class="btn-ok">Có</button>
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
            warningDataChange: false, //ẩn hiện message box cảnh báo khi tắt form detail nếu dữ liệu thay đổi
        }
    },

    methods:{
        // hàm hiện warning box
        // Createdby TuanNV (18/6/2021)
        showWarningDataChange(){
            this.warningDataChange = true;
            setTimeout(() =>
                this.warningDataChange = false, 5000
            )
        },

        // hàm emit tắt form detail
        // Createdby TuanNV (18/6/2021)
        cancelFormDetail(){
            this.warningDataChange = false;
            EventBus.$emit("cancelFormDetail");
        },
    },

    mounted(){
        // lắng nghe sự kiện hiện form cảnh báo dữ liệu thay đổi
        EventBus.$on("showWarningDataChange", ()=>{
            this.showWarningDataChange();
        });
    }
}
</script>

<style scoped src="./messagebox_datachange.css"></style>