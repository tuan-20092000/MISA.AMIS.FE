<template>
    <div class="container-box">
        <!-- message box cảnh báo lỗi tới người dùng -->
        <div v-if="error" class="wrap-message-form">
            <div class="message-box">
                <div class="div-content">
                    <div class="icon-message">
                        <img src="../../../Resource/img/warning.svg" alt="">
                    </div>
                    <div class="message-content">
                        <div> {{messageContent}}</div>
                    </div>
                    
                </div>
                <div class="space"></div>
                <div class="mess-footer">
                    <button v-on:click="cancelErrorBox()" ref="btnAgree" class="btn-ok">Đồng ý</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import EventBus from '../../../main.js';
export default {
    data(){
        return{
            error: false, // ẩn hiện box báo lỗi
             messageContent: null, // nội dung thông báo cho người dùng
        }
    },


    methods: {
         // tắt form báo lỗi
        // Createdby TuanNV (18/6/2021)
        cancelErrorBox(){
            this.error = false;
            if(this.field !=null){
                EventBus.$emit("focusAndSelectAll", this.field);
                this.field = null;
            }
        },

        // hàm hiển thị message box thông báo lỗi
        // Createdby TuanNV (18/6/2021)
        showError(message, field){
            this.messageContent = message;
            this.error = true;
            this.field = field;
            setTimeout(() =>
                this.cancelErrorBox(), 2500
            )
        },
    },


    mounted(){
        // lắng nghe sự kiện hiện thông báo lỗi
        EventBus.$on("showError", (content ,field)=> {
            this.showError(content, field);
        });
    }
}
</script>

<style scoped src="./messagebox_error.css"> </style>