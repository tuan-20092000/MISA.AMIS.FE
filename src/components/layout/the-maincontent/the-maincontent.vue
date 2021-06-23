<template>
  <div
    class="main"
    @click="clickMain($event)"
  >

    <!-- title main content -->
    <div class="main-title">
      <div class="main-title-content">
        <div class="title">Nhân viên</div>
        <div class="main-title-btn">
          <button v-on:click="addEmployee()" class="btn-add" title="Thêm mới (F1)"><span>Thêm mới nhân viên</span></button>
        </div>
      </div>
    </div>

    <!-- controller -->
    <div class="layout" id="layout-scroll">
      <div class="grid">
        <div class="l_excution">
          
        </div>
        <div class="right-control">
          <div class="search-name-id">
            <input
              id="search-name-id"
              v-model="keySearch"
              ref="inputSearch"
              type="text"
              v-on:keyup.enter="searchKeyWord()"
              @keydown.tab.prevent.stop="edit()"
              placeholder="Tìm theo mã, tên nhân viên"
              autocomplete="off"
            />
            <div style="background-color: #fff;width:32px;32px;">
              <img src="../../../Resource/img/find.svg" alt="search" />
            </div>
          </div>
          <div class="three-btn">
            <img
              v-on:click="refresh()"
              src="../../../Resource/img/refresh.svg"
              alt=""
            />
            <export-excel
              class   = "btn btn-default"
              :data   = "json_data"
              :fields = "json_fields"
              worksheet = "My Worksheet"
              :fetch = "getAllData"
              name    = "filename.xls">
              <img src="../../../Resource/img/excel.svg" alt="" />
            </export-excel>
          </div>
        </div>
      </div>

      <!-- table employee -->
      <div class="" id="ms-component-table">
        <div class="wrap-table" id="wrap_table">
          <div class="ms-table">
            <table>
              <caption></caption>
              <thead>
                <tr>
                  <th id="" class="out-left-white bgc-white"></th>
                  <th style="left: 16px" class="check-all" id="check-all">
                    <input type="checkbox" name="" id="" />
                  </th>
                  <th class="align-left" style="min-width: 150px; width: 150px" id="">
                    MÃ NHÂN VIÊN
                  </th>
                  <th class="align-left" style="min-width: 250px; width: 250px" id="">
                    TÊN NHÂN VIÊN
                  </th>
                  <th class="align-left" style="min-width: 120px; width: 120px" id="">
                    GIỚI TÍNH
                  </th>
                  <th style="min-width: 150px; width: 150px" id="">
                    NGÀY SINH
                  </th>
                  <th class="align-left" style="min-width: 200px; width: 200px" id="">SỐ CMND</th>
                  <th class="align-left" style="min-width: 250px; width: 250px" id="">
                    CHỨC DANH
                  </th>
                  <th class="align-left" style="min-width: 250px; width: 250px" id="">
                    TÊN ĐƠN VỊ
                  </th>
                  <th class="align-left" style="min-width: 150px; width: 150px" id="">
                    SỐ TÀI KHOẢN
                  </th>
                  <th class="align-left" style="min-width: 250px; width: 250px" id="">
                    TÊN NGÂN HÀNG
                  </th>
                  <th class="align-left" 
                    style="min-width: 250px; width: 250px; border-right: none"
                    id=""
                  >
                    CHI NHÁNH TK NGÂN HÀNG
                  </th>
                  <th
                    style="width: 120px; min-width: 120px; right: 30px"
                    class="wiget border-left"
                    id=""
                  >
                    <div>CHỨC NĂNG</div>
                  </th>
                  <th style="right: 0px" id="" class="out-right bgc-white"></th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(employee, index) in employeeList"
                  :key="index"
                  v-on:click="active(index)"
                  v-on:dblclick="edit(index)"
                >
                  <td id="" class="out-left-white bgc-white"></td>
                  <td
                    v-bind:class="{ selected: index == selectedRow }"
                    class="col-check"
                  >
                    <input type="checkbox" name="" id="" />
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.employeeCode }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.employeeName }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ convertGender(employee.gender) }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    <div class="center">
                      {{ convertDate(employee.dateOfBirth) }}
                    </div>
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.identityNumber }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.employeePosition }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.departmentName }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.bankAccountNumber }}
                  </td>
                  <td v-bind:class="{ selected: index == selectedRow }">
                    {{ employee.bankName }}
                  </td>
                  <td
                    style="border-right: none"
                    v-bind:class="{ selected: index == selectedRow }"
                  >
                    {{ employee.bankBranchName }}
                  </td>
                  <td
                    style="right: 30px"
                    v-bind:class="{ selected: index == selectedRow }"
                    class="sticky border-left"
                  >
                    <div class="col-edit">
                      <span v-on:click="edit(index)">Sửa</span>
                      <div class="div-more-icon">
                        <img
                          src="../../../Resource/img/more.svg"
                          alt=""
                          @click="showMoreOption($event, index)"
                        />
                      </div>
                    </div>
                  </td>
                  <td style="right: 0px" class="out-right bgc-white"></td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- footer table -->
          <div class="wrap-footer">
            <div class="footer-content">
              <div class="count-record">
                <span
                  >Tổng số: <b>{{ countEmployee }}</b> bản ghi</span
                >
              </div>
              <div class="right-footer">
                <div class="div-select-count">
                  <div style="position: sticky; left: 60%; right: 40%">
                    <div
                      style="
                        max-width: 215px;
                        max-height: 32px;
                        display: flex;
                        top: -17px;
                        border: 1px solid #babec5;
                        margin: 0 16px;
                      "
                      data-app
                    >
                      <v-select
                        :items="optionCountPerPage"
                        v-model="countEmployeePerPage"
                        @change="changeCountPerPage"
                        solo
                      ></v-select>
                    </div>
                  </div>
                </div>
                <div class="wrap-paging">
                  <v-container class="max-width">
                    <v-pagination
                      v-model="selectedPage"
                      :length="countPage"
                      @input="getDataPage()"
                      :total-visible="7"
                    ></v-pagination>
                  </v-container>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="div-left-30"></div>

    <!-- popup option -->
    <div
      v-if="showOption"
      :style="{ left: xClick + 'px', top: yClick + 'px' }"
      class="tool-tip"
      id="popupOption"
    >
      <div class="option" @click="cloneEmployee()">Nhân bản</div>
      <div class="option" @click="showWarning()">Xóa</div>
    </div>
    <div v-if="loading" class="wrap-loading">
        <div class="loading" style="width:fit-content;height:32px;">
          <img style="" src="../../../Resource/img/loading.svg" alt="">
        </div>
    </div>
  </div>
</template>

<script>
import EventBus from "../../../main.js";
const axios = require("axios");
 export default {
  data() {
    return {
      employeeList: {}, // những nhân viên hiển thị lên màn hình
      countEmployee: 0, // số nhân viên lấy/tìm kiếm được
      selectedRow: 0, // hàng đang chọn
      countEmployeePerPage: 10, // số lượng bản ghi / trang
      countPage: 1, // số trang
      selectedPage: 1, // trang đang chọn
      optionCountPerPage: [
        { value: 10, text: "10 bản ghi trên 1 trang" },
        { value: 20, text: "20 bản ghi trên 1 trang" },
        { value: 30, text: "30 bản ghi trên 1 trang" },
        { value: 50, text: "50 bản ghi trên 1 trang" },
        { value: 100, text: "100 bản ghi trên 1 trang" },
      ],

      showOption: false, // ẩn hiện option xóa, nhân bản
      xClick: 0, // tọa độ x click chuột
      yClick: 0, // tọa độ y click chuột
      employee: null, // thông tin nhân viên
      keySearch: "", // từ khóa tìm kiếm
      loading: true, // ẩn hiện loading
      formDetail: false, // trạng thái hiện tại form detail (ẩn hoặc hiện)

      // header excel export
      json_fields:{
        'Mã nhân viên': 'employeeCode',
        'Tên nhân viên': 'employeeName',
        'Giới tính': 'gender',
        'Ngày sinh': 'dateOfBirth',
        'Số CMND': 'identityNumber',
        'Chức danh': 'employeePosition',
        'Đơn vị': 'departmentName',
        'Số tài khoản': 'bankAccountNumber',
        'Tên ngân hàng': 'bankName',
        'Chi nhánh ngân hàng': 'bankBranchName'
      },

      // data excel export
      json_data: null,
    }
  },

  methods: {
    // lấy số lượng bản ghi theo từ khóa tìm kiếm
    // Createdby TuanNV (18/6/2021)
    async getTotalRecord() {
      let me = this;
      this.loading = true;
      // bỏ khoảng trắng 2 đầu từ khóa
      let keySearch = me.keySearch.trim();
      // axios request
      await axios
        .request({
          method: "post",
          url: "http://localhost:8080/api/v1/Employees/GetTotalRecord",
          data: '"' + keySearch + '"',
          headers: { "Content-Type": "application/json" },
        })
        .then((res) => {
          // nếu số lượng lớn hơn 0 thì thực hiện gán cho các biến
          if (res.data != "") me.countEmployee = res.data;
          else me.countEmployee = 0;
          me.selectedPage = 1;
          me.selectedRow = 0;
          me.getDataServer();
        })
        .catch((err) => {
          // nếu có lỗi thì hiện thông báo lỗi
          console.log(err);
          this.loading - false;
          let message =
            "Lấy dữ liệu thất bại, vui lòng liên hệ Misa để được trợ giúp.";
          EventBus.$emit("showError", message);
        });
    },

    // hàm lấy dữ liệu từ server
    // Createdby TuanNV (18/6/2021)
    async getDataServer() {
      this.loading = true;
      let me = this;
      // bỏ khoảng trắng 2 đầu 
      let keySearch = me.keySearch.trim();
      // axios request
      await axios
        .request({
          method: "post",
          url:
            "http://localhost:8080/api/v1/Employees/GetPage?page=" +
            me.selectedPage +
            "&count=" +
            me.countEmployeePerPage,
          data: '"' + keySearch + '"',
          headers: { "Content-Type": "application/json" },
        })
        .then((res) => {
          // nếu thành công thì thực hiện gán 
          me.employeeList = res.data;
          me.countPage = Math.ceil(me.countEmployee / me.countEmployeePerPage);
          this.loading = false;
        })
        .catch((err) => {
          // nếu có lỗi thì hiện thông báo lỗi
          console.log(err);
          this.loading = false;
          let message =
            "Lấy dữ liệu thất bại, vui lòng liên hệ Misa để được trợ giúp.";
          EventBus.$emit("showError", message);
        });
    },

    // hàm xóa 1 nhân viên
    // Createdby TuanNV (18/6/2021)
    async deleteEmployee(employee) {
      this.loading = true;
      let url = "http://localhost:8080/api/v1/Employees/" + employee.employeeId;
      await axios
        .delete(url)
        .then((res) => {
          // nếu thành công thì hiện message box thông báo xóa thành công
          console.log(res);
          this.loading = false;
          let mode = "Xóa";
          this.getTotalRecord();
          EventBus.$emit("showSuccessBox", mode);
        })
        .catch((err) => {
          // nếu có lỗi xảy ra thì hiện thông báo lỗi
          this.loading = false;
          console.log(err);
          // let message =
          //   "Có lỗi xảy ra khi xóa bản ghi, vui lòng liên hệ Misa để được trợ giúp.";
          // EventBus.$emit("showError", message);
        });
    },

    // hàm thực hiện lấy hết dữ liệu trước khi xuất ra excel
    // Createdby TuanNV (18/6/2021)
    async getAllData(){
      let me = this;
      // bỏ khoảng trắng 2 đầu 
      let keySearch = me.keySearch.trim();
      // axios request
      await axios
        .request({
          method: "post",
          url:
            "http://localhost:8080/api/v1/Employees/GetAllData",
          data: '"' + keySearch + '"',
          headers: { "Content-Type": "application/json" },
        })
        .then((res) => {
          // nếu thành công thì thực hiện gán 
          me.json_data = res.data;
        })
        .catch((err) => {
          // nếu có lỗi thì hiện thông báo lỗi
          console.log(err);
          let message =
            "Xuất excel thất bại";
          EventBus.$emit("showError", message);
        });
      return me.json_data;
    },

    // hàm thêm mới nhân viên
    // Createdby TuanNV (18/6/2021)
    addEmployee(){
      EventBus.$emit("addEmployee");
      this.formDetail = true;
    },

    // sự kiện khi ấn vào các hàng
    // Createdby TuanNV (18/6/2021)
    active(index) {
      this.selectedRow = index;
    },

    // sự kiện khi ấn sửa
    // Createdby TuanNV (18/6/2021)
    edit(index) {
      let employee = null;
      if(index == undefined){
        employee = this.employeeList[this.selectedRow];
      }
      else employee = this.employeeList[index];
      employee.dateOfBirth = this.convertDate(employee.dateOfBirth);
      employee.identityDate = this.convertDate(employee.identityDate);
      EventBus.$emit("editEmployee", employee);
    },

    // hàm lấy dữ liệu từ server cho trang được chọn
    // Createdby TuanNV (18/6/2021)
    getDataPage() {
      this.selectedRow = 0;
      this.getDataServer();
    },

    // hàm khi thay đổi số lượng nhân viên hiển thị trên 1 trang
    // Createdby TuanNV (18/6/2021)
    changeCountPerPage() {
      this.selectedRow = 0;
      this.selectedPage = 1;
      this.getDataServer();
    },

    // hàm hiện popup option
    // Createdby TuanNV (18/6/2021)
    showMoreOption(e, index) {
      this.xClick = e.clientX - 280;
      this.yClick = e.clientY - 40;
      this.selectedRow = index;
      this.showOption = true;
    },

    // hàm reload lại dữ liệu
    // Createdby TuanNV (18/6/2021)
    refresh() {
      this.keySearch = "";
      this.getTotalRecord();
    },

    // hàm tìm kiếm từ
    // Createdby TuanNV (18/6/2021)
    searchKeyWord(){
      this.getTotalRecord();
    },

    // sự kiện ấn mũi tên xuống
    // Createdby TuanNV (18/6/2021)
    arrowDown() {
      if(this.selectedRow < this.countEmployeePerPage -1){
        this.selectedRow = this.selectedRow + 1;
        document.getElementById("layout-scroll").scrollTop += 47;
      }
    },

    // sự kiện ấn mũi tên lên
    // Createdby TuanNV (18/6/2021)
    arrowUp(){
      if(this.selectedRow > 0){
        this.selectedRow = this.selectedRow - 1;
        document.getElementById("layout-scroll").scrollTop -= 47;
      }
    },

    // quay lại page trước
    // CreatedBy TuanNV (20/6/2021)
    prevPage(){
      let me = this;
      if(me.selectedPage > 1){
        me.selectedPage --;
        me.getDataServer();
        me.selectedRow = 0;
        document.getElementById("layout-scroll").scrollTop = 0;
      }
    },

    // next sang page tiếp theo
    // CreatedBy TuanNV (20/6/2021)
    nextPage(){
      let me = this;
      if(me.selectedPage < me.countPage){
        me.selectedPage ++;
        me.getDataServer();
        me.selectedRow = 0;
        document.getElementById("layout-scroll").scrollTop = 0;
      }
    },

    // show form cảnh báo trước khi xóa
    // Createdby TuanNV (18/6/2021)
    showWarning() {
      let employee = {...this.employeeList[this.selectedRow]};
      EventBus.$emit("showWarning", employee);
      this.showOption = false;
    },

    // ẩn popup option nếu click ra ngoài
    // Createdby TuanNV (18/6/2021)
    clickMain(e) {
      let component = document.getElementById("ms-component-table");
      if (component != null && this.showOption) {
        if (!component.contains(e.target)) {
          this.showOption = false;
        }
      }
    },

    // chuyển đổi ngày tháng năm từ server thành dd/mm/yyyy để hiển thị
    // Createdby TuanNV (18/6/2021)
    convertDate(dateSrc) {
      if (dateSrc == null) return;
      let date = new Date(dateSrc),
        year = date.getFullYear().toString(),
        month = (date.getMonth() + 1).toString().padStart(2, "0"),
        day = date.getDate().toString().padStart(2, "0");
      return `${day}/${month}/${year}`;
    },

    // chuyển đổi kiểu giới tính để hiển thị
    // Createdby TuanNV (18/6/2021)
    convertGender(gd) {
      switch (gd) {
        case 0:
          return "Nam";
        case 1:
          return "Nữ";
        case 2:
          return "Khác";
        default:
          return "";
      }
    },

    // hàm nhân bản nhân viên
    // Createdby TuanNV (18/6/2021)
    cloneEmployee() {
      let employee = { ...this.employeeList[this.selectedRow] };
      employee.dateOfBirth = this.convertDate(employee.dateOfBirth);
      employee.identityDate = this.convertDate(employee.identityDate);
      EventBus.$emit("addEmployee", employee);
    },

  },

  // khởi tạo xong thì lấy dữ liệu
  created() {
    this.getTotalRecord();
  },

  mounted() {
    let me = this;
    //hàm lắng nghe sự kiện khi các component khác gọi loadData
    // Createdby TuanNV (18/6/2021)
    EventBus.$on("loadDataServer", () => {
      me.getTotalRecord();
    });

    // lắng nghe sự kiên xóa nhân viên
    // Createdby TuanNV (18/6/2021)
    EventBus.$on("deleteEmployee", employee => {
      me.deleteEmployee(employee);
    });

    EventBus.$on("hideFormDetail", ()=>{
      me.formDetail = false;
    })

    // lắng nghe sự kiện ấn các phím
    // Createdby TuanNV (18/6/2021)
    document.addEventListener("keydown", function (event){
      if(!me.formDetail){
        // ấn mũi tên lên
        // Createdby TuanNV (18/6/2021)
        if(event.code == "ArrowUp"){
          event.preventDefault();
          me.arrowUp();
        }

        // ấn mũi tên xuống
        // Createdby TuanNV (18/6/2021)
        if(event.code == "ArrowDown"){
          event.preventDefault();
          me.arrowDown();
        }

        // ấn mũi tên sang trái
        // Createdby TuanNV (18/6/2021)
        if(event.code == "ArrowLeft"){
          // event.preventDefault();
          me.prevPage();
        }

        // ấn mũi tên sang phải
        // Createdby TuanNV (18/6/2021)
        if(event.code == "ArrowRight"){
          // event.preventDefault();
          me.nextPage();
        }

        // ấn F1 thì xuất hiện form thêm mới
        // Createdby TuanNV (18/6/2021)
        if(event.code == "F1"){
          event.preventDefault();
          me.addEmployee();
        }

        // ấn F2 thì xuất hiện form edit
        // Createdby TuanNV (18/6/2021)
        if(event.code == "F2"){
          event.preventDefault();
          me.edit();
        }

        // ấn F3 thì xuất hiện message box cảnh báo trước khi xóa
        // Createdby TuanNV (18/6/2021)
        if(event.code == "F3"){
          event.preventDefault();
          me.showWarning();
        }

        // ấn F4 thì clone nhân viên đang được chọn
        // Createdby TuanNV (18/6/2021)
        if(event.code == "F4"){
          event.preventDefault();
          me.cloneEmployee();
        }
      }
    })
  },

  
};
</script>

<style src="../../common/common_css/googlefont.css"></style>
<style src="./the-maincontent.css"></style>
