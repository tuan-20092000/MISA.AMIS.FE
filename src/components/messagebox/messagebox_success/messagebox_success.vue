<template>
  <!-- message box thông báo thao tác thành công tới người dùng -->
    <div class="container-box">
        <div v-if="successBox" class="success-box">
            <div style="height: 36px;display:flex;flex-direction:row;margin-bottom:25px;">
                <div style="height:100%; width:36px;">
                    <img src="../../../Resource/img/success.svg" alt="">
                </div>
                <div class="success-content">
                    <span>{{successMode}} thành công</span>
                </div>
            </div>
            <div class="space" style="margin-bottom:10px;"></div>
            <div>
                <button v-on:click="cancelSuccessBox()" class="btn-ok">OK</button>
            </div>
        </div>
    </div>
</template>

<script>
import EventBus from '../../../main.js';
export default {
    data(){
        return {
            successBox: false, // ẩn hiện success box
            successMode: null, // mode của success box (xóa hay sửa, thêm thành công)
        }
    },

    methods:{
        // hàm hiện success box
        // Createdby TuanNV (18/6/2021)
        showSuccessBox(){
            this.successBox = true;
            setTimeout(()=>
                this.successBox = false, 2500
            )
        },

        // hàm tắt success box
        // Createdby TuanNV (18/6/2021)
        cancelSuccessBox(){
            this.successBox = false;
        },
    },

    mounted(){
        // lắng nghe sự kiện hiện success box
        EventBus.$on("showSuccessBox", (mode)=>{
            this.successMode = mode;
            this.showSuccessBox();
        })
    }
}
</script>

<style src="./messagebox_success.css"></style>