<template>
  <div
    class="wrap-form"
    v-if="showFormDetail"
    @keyup.esc="preCancelForm()"
    @keydown.ctrl.83.prevent.stop.exact="save()"
    @keydown.ctrl.shift.83.prevent.stop.exact="saveAndAdd()"
    @click="clickForm($event)"
  >

    <!-- button help and cancel form -->
    <div class="form-detail" id="form-detail">
      <div class="div-tool">
        <img src="../../Resource/img/support.svg" alt="" title="Giúp (F1)" />
        <div class="inline" v-on:click="preCancelForm()">
          <img src="../../Resource/img/x.svg" alt="" title="Đóng (ESC)" />
        </div>
      </div>

      <!-- title-formdetail -->
      <div class="form-header">
        <div class="form-title">Thông tin nhân viên</div>
      </div>
      <div class="form-content">
        <div class="form-content-scroll">
          <div class="form-content-top">
            <div class="form-content-left">
              <div class="row-input">
                <div class="employeeCode flex-col">
                  <button
                    ref="btnFirst"
                    class="btn-focus"
                    id="btn-first"
                    @keyup.shift.tab.exact="shiftTab()"
                  ></button>

                  <!-- main-content-formdetail -->
                  <!-- input mã nhân viên -->
                  <label for="EmployeeCode"
                    >Mã<span style="color: red"> *</span></label
                  >
                  <input
                    v-model="employee.employeeCode"
                    id="employeeCode"
                    @blur="handleBlur($event)"
                    class="uppercase input-normal"
                    ref="EmployeeCode"
                    type="text"
                    name="EmployeeCode"
                    minlength="4"
                    maxlength="8"
                    required="true"
                  />
                </div>

                <!-- input họ tên -->
                <div class="employeeName flex-col">
                  <label for="EmployeeName"
                    >Tên<span style="color: red"> *</span></label
                  >
                  <input
                    v-model="employee.employeeName"
                    @blur="handleBlur($event)"
                    id="employeeName"
                    ref="EmployeeName"
                    class="capitalize input-normal"
                    type="text"
                    name="EmployeeName"
                    required="true"
                  />
                </div>
              </div>

              <!-- input tên đơn vị -->
              <div class="row-input flex-col">
                <label for="DepartmentName"
                  >Đơn vị<span style="color: red"> *</span></label
                >
                <Combobox
                  :departmentName="employee.departmentName"
                  @selectDepartment="selectDepartment"
                  :data="dataCbx"
                  :width="392"
                  :height="32"
                />
              </div>

              <!-- input chức danh nhân viên -->
              <div class="row-input flex-col">
                <label for="EmployeePosition">Chức danh</label>
                <input
                  v-model="employee.employeePosition"
                  class="input-normal"
                  ref="EmployeePosition"
                  type="text"
                  name="EmployeePosition"
                />
              </div>
            </div>

            <!-- input ngày sinh -->
            <div class="form-content-right">
              <div class="row-input">
                <div class="DateOfBirth flex-col" id="picker_dateofbirth">
                  <label for="DateOfBirth">Ngày sinh</label>
                  <div class="input-date">
                    <input
                      v-model="employee.dateOfBirth"
                      type="text"
                      name="DateOfBirth"
                      ref="DateOfBirth"
                      autocomplete="off"
                    />
                    <div
                      @click="() => {showDateOfBirth = !showDateOfBirth;}"
                      class="div-icon-datepicker"
                    >
                      <img src="../../Resource/img/date_picker.svg" alt="" />
                    </div>
                  </div>
                  <div v-if="showDateOfBirth" class="date__picker">
                    <v-date-picker
                      v-model="dateOfBirth_PK"
                      no-title
                      locale="vi-VN"
                      @click:date="chooseDateOfBirth()"
                    >
                      <div class="date_picker_footer">
                        <a href="#" @click="() => {employee.dateOfBirth = getCurrentDay();showDateOfBirth = false;}">
                          Hôm nay</a>
                      </div>
                    </v-date-picker>
                  </div>
                </div>

                <!-- input giới tính -->
                <div class="Gender flex-col">
                  <div class="label-gender" style="line-height: 16px">
                    Giới tính
                  </div>
                  <div class="radio-gender">
                    <label class="label-radio-gender">Nam
                      <input v-model="employee.gender" type="radio" name="radio" value="0">
                      <span class="checkmark"></span>
                    </label>
                    <label class="label-radio-gender">Nữ
                      <input v-model="employee.gender" type="radio" name="radio" value="1">
                      <span class="checkmark"></span>
                    </label>
                    <label class="label-radio-gender">Khác
                      <input v-model="employee.gender" type="radio" name="radio" value="2">
                      <span class="checkmark"></span>
                    </label>
                  </div>
                </div>
              </div>

              <!-- input số CMND -->
              <div class="row-input">
                <div class="EntityCard flex-col">
                  <label for="IdentityNumber">Số CMND</label>
                  <input
                    v-model="employee.identityNumber"
                    ref="IdentityNumber"
                    class="input-normal"
                    type="number"
                    name="IdentityNumber"
                  />
                </div>

                <!-- input ngày cấp -->
                <div class="DateSupply flex-col" id="picker_identitydate">
                  <label for="IdentityDate">Ngày cấp</label>
                  <div class="input-date" style="margin-top: 5px">
                    <input
                      v-model="employee.identityDate"
                      type="text"
                      name="IdentityDate"
                      ref="IdentityDate"
                      autocomplete="off"
                    />
                    <div
                      @click="() => {showIdentityDate = !showIdentityDate;}"
                      class="div-icon-datepicker"
                    >
                      <img src="../../Resource/img/date_picker.svg" alt="" />
                    </div>
                  </div>
                  <div
                    v-if="showIdentityDate"
                    class="date__picker"
                    style="right: 0px"
                  >
                    <v-date-picker
                      v-model="identityDate_PK"
                      no-title
                      locale="vi-VN"
                      @click:date="chooseIdentityDate()"
                    >
                      <div class="date_picker_footer">
                        <a
                          href="#" @click="() => {  employee.identityDate = getCurrentDay();  showIdentityDate = false;}"
                        >
                          Hôm nay</a
                        >
                      </div>
                    </v-date-picker>
                  </div>
                </div>
              </div>

              <!-- input nơi cấp CMND -->
              <div class="row-input flex-col">
                <label for="IdentityPlace">Nơi cấp</label>
                <input
                  v-model="employee.identityPlace"
                  ref="IdentityPlace"
                  class="input-normal"
                  type="text"
                  name="IdentityPlace"
                />
              </div>
            </div>
          </div>

          <!-- input địa chỉ -->
          <div class="form-content-bottom">
            <div class="row-input flex-col">
              <label for="addresss">Địa chỉ</label>
              <input
                v-model="employee.address"
                class="input-normal"
                ref="addresss"
                type="text"
                name="addresss"
              />
            </div>

            <!-- input ĐT di động -->
            <div class="row-input">
              <div class="PhoneNumber div-normal">
                <label for="PhoneNumber">ĐT di động</label>
                <input
                  v-model="employee.phoneNumber"
                  class="input-normal"
                  ref="PhoneNumber"
                  type="number"
                  name="PhoneNumber"
                />
              </div>

              <!-- input ĐT cố định -->
              <div class="TelephoneNumber div-normal">
                <label for="TelephoneNumber">ĐT cố định</label>
                <input
                  v-model="employee.telephoneNumber"
                  class="input-normal"
                  ref="TelephoneNumber"
                  type="number"
                  name="TelephoneNumber"
                />
              </div>

              <!-- input email -->
              <div class="Email div-normal">
                <label for="email">Email</label>
                <input
                  v-model="employee.email"
                  class="input-normal"
                  ref="Email"
                  type="email"
                  name="Email"
                />
              </div>
            </div>

            <!-- input tài khoản ngân hàng -->
            <div class="row-input">
              <div class="BankAccountNumber div-normal">
                <label for="bankAccountNumber">Tài khoản ngân hàng</label>
                <input
                  v-model="employee.bankAccountNumber"
                  class="input-normal"
                  ref="bankAccountNumber"
                  type="number"
                  name="bankAccountNumber"
                />
              </div>

              <!-- input tên ngân hàng -->
              <div class="BankName div-normal">
                <label for="BankName">Tên ngân hàng</label>
                <input
                  v-model="employee.bankName"
                  class="input-normal"
                  ref="BankName"
                  type="text"
                  name="BankName"
                />
              </div>

              <!-- input tên chi nhánh -->
              <div class="BankBranchName div-normal">
                <label for="BankBranchName">Tên chi nhánh</label>
                <input
                  v-model="employee.bankBranchName"
                  class="input-normal"
                  ref="BankBranchName"
                  type="text"
                  name="BankBranchName"
                />
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- footer form detail -->
      <div class="form-footer">
        <div class="div-line"></div>
        <button v-on:click="preCancelForm()" class="btn-cancel btn-primary">
          Hủy
        </button>
        <div class="float-right">
          <button
            v-on:click="save()"
            class="btn-save btn-primary"
            title="Cất (Ctrl+S)"
          >
            Cất
          </button>
          <button
            v-on:click="saveAndAdd()"
            class="btn-saveAndadd btn-secondary"
            title="Cất và Thêm (Ctrl + Shift + S)"
            ref="saveAndAdd"
            
          >
            Cất và Thêm
          </button>
          <button class="btn-focus" id="btn-last" @keyup.tab="tab()"></button>
        </div>
      </div>
    </div>

    <!-- div loading -->
    <div v-if="loading" class="wrap-loading">
      <div class="loading" style="width: fit-content; height: 32px">
        <img style="" src="../../Resource/img/loading.svg" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");
import Combobox from "../common/combobox/combobox.vue";
import EventBus from "../../main.js";
export default {
  data() {
    return {
      // ẩn hiện form thông tin
      showFormDetail: false,

      // mode của form (thêm mới hay update ...)
      formMode: null,

      // thông tin nhân viên binding lên form
      employee: {
        employeeCode: "",
        employeeName: "",
        gender: 0,
        dateOfBirth: "",
        departmentName: "",
        identityDate: "",
        identityPlace: "",
        employeePosition: "",
        address: "",
        bankAccountNumber: "",
        bankName: "",
        bankBranchName: "",
        bankProvinceName: "",
        phoneNumber: "",
        telephoneNumber: "",
        email: "",
      },

      cEmployee: null, // clone employee để ktra xem dữ liệu thay đổi hay không

      fieldMissingData: null, // trường bắt buộc nhập còn trống đầu tiên
      messageContent: null, // nội dung tin nhắn cần show cho người dùng nếu cần

      showDateOfBirth: false, // ẩn hiện date-picker của ngày sinh

      dateOfBirth_PK: null, // ngày sinh để v-model với date picker

      showIdentityDate: false, // ẩn hiên date-picker của ngày cấp

      identityDate_PK: null, // ngày sinh để v-model với date picker

      loading: false, // ẩn hiện loading


      // dữ liệu truyền vào combobox
      dataCbx: [
                { text: 'Phòng nhân sự', value: 0 },
                { text: 'Phòng kinh doanh', value: 1 },
                { text: 'Phòng marketing', value: 2 },
                { text: 'Phòng ngoại giao', value: 3 },
              ]
    };
  },

  components: {
    Combobox,
  },

  methods: {
    // hàm mở form
    // Createdby TuanNV (18/6/2021)
    showForm() {
      // mở form
      this.showFormDetail = true;
      this.cEmployee = { ...this.employee };
      // auto focus vào input đầu và bôi đen hết text bên trong nếu có
      this.focusAndSelectAll();
    },

    // hàm focus và bôi đen text trong input nếu có
    // Createdby TuanNV (18/6/2021)
    focusAndSelectAll(index) {
      if (!index || index == "EmployeeCode") {
        this.$nextTick(function () {
          if (this.$refs.EmployeeCode != undefined) {
            this.$refs.EmployeeCode.focus();
            this.$refs.EmployeeCode.select();
          }
        });
      } else {
        switch (index) {
          case "EmployeeName":
            this.$nextTick(function () {
              if (this.$refs.EmployeeName != undefined) {
                this.$refs.EmployeeName.focus();
                this.$refs.EmployeeName.select();
              }
            });
            return;
          case "DateOfBirth":
            this.$nextTick(function () {
              if (this.$refs.DateOfBirth != undefined) {
                this.$refs.DateOfBirth.focus();
                this.$refs.DateOfBirth.select();
              }
            });
            return;
          case "IdentityDate":
            this.$nextTick(function () {
              if (this.$refs.IdentityDate != undefined) {
                this.$refs.IdentityDate.focus();
                this.$refs.IdentityDate.select();
              }
            });
            return;
          case "DepartmentName":
            this.$nextTick(function () {
              if (this.$children[0] != undefined) {
                this.$children[0].$refs.DepartmentName.focus();
                this.$children[0].$refs.DepartmentName.select();
              }
            });
            return;
          case "Email":
            this.$nextTick(function () {
              if (this.$refs.Email != undefined) {
                this.$refs.Email.focus();
                this.$refs.Email.select();
              }
            });
            return;
          case "PhoneNumber":
            this.$nextTick(function () {
              if (this.$refs.PhoneNumber != undefined) {
                this.$refs.PhoneNumber.focus();
                this.$refs.PhoneNumber.select();
              }
            });
            return;
          case "TelephoneNumber":
            this.$nextTick(function () {
              if (this.$refs.TelephoneNumber != undefined) {
                this.$refs.TelephoneNumber.focus();
                this.$refs.TelephoneNumber.select();
              }
            });
            return;
        }
      }
    },

    // hàm tắt form detail
    // Createdby TuanNV (18/6/2021)
    cancelFormDetail() {
      this.showFormDetail = false;
      this.showIdentityDate = false;
      this.showDateOfBirth = false;
      this.resetForm();
      this.increaseCode();
      EventBus.$emit("hideFormDetail");
    },

    // kiểm tra xem dữ liệu đã thay đổi hay chưa, nếu đã thay đổi thì hiện message thông báo,
    // nếu chưa thì thực hiện đóng form
    // Createdby TuanNV (18/6/2021)
    preCancelForm() {
      let employee1 = JSON.stringify(this.cEmployee);
      let employee2 = JSON.stringify(this.employee);
      if (employee1 != employee2) {
        EventBus.$emit("showWarningDataChange");
      } else {
        this.cancelFormDetail();
      }
    },

    //hàm reset form
    // Createdby TuanNV (18/6/2021)
    resetForm() {
      // null employee
      let nullEmployee = {
        employeeCode: "",
        employeeName: "",
        gender: 0,
        dateOfBirth: "",
        departmentName: "",
        identityDate: "",
        identityPlace: "",
        employeePosition: "",
        address: "",
        bankAccountNumber: "",
        bankName: "",
        bankBranchName: "",
        bankProvinceName: "",
        phoneNumber: "",
        telephoneNumber: "",
        email: "",
      };
      this.employee = nullEmployee;
      this.$children[0].selectValue = {};
    },

    // hàm lưu và tắt form
    // Createdby TuanNV (18/6/2021)
    save() {
      let me = this;
      if (me.checkInputRequired() && me.validateObject()) {
        me.handleRequest("save");
      } else {
        // hiên message box thông báo lỗi
        let field = me.fieldMissingData;
        let message = me.messageContent;
        EventBus.$emit("showError", message, field);
      }
    },

    // hàm lưu và thêm mới
    // Createdby TuanNV (18/6/2021)
    saveAndAdd() {
      let me = this;
      if (me.checkInputRequired() && me.validateObject()) {
        me.handleRequest("saveAndAdd");
      } else {
        // hiên message box thông báo lỗi
        let field = me.fieldMissingData;
        let message = me.messageContent;
        EventBus.$emit("showError", message, field);
      }
    },

    // chuyển đôi các trường cần thiết trước khi gửi tới server
    // Createdby TuanNV (18/6/2021)
    convertPreSend() {
      let me = this;
      let employee = { ...me.employee };
      // chuyển date hiển thị thành date server
      if (
        employee.dateOfBirth == null ||
        employee.dateOfBirth == undefined ||
        employee.dateOfBirth == ""
      ) {
        employee.dateOfBirth = null;
      } else {
        employee.dateOfBirth = me.dateOfBirth_PK;
      }

      if (
        employee.identityDate == null ||
        employee.identityDate == undefined ||
        employee.identityDate == ""
      ) {
        employee.identityDate = null;
      } else {
        employee.identityDate = me.identityDate_PK;
      }
      let employeeCode = employee.employeeCode.toUpperCase();
      // chèn thêm số 0 vào trước số mã nhân viên nếu mã chưa đủ 8 kí tự
      employeeCode =
        employeeCode.substring(0, 3) + employeeCode.substr(3).padStart(5, "0");
      employee.employeeCode = employeeCode;
      return employee;
    },

    // xử lý request tới server (thêm mới hay sửa chữa?)
    // Createdby TuanNV (18/6/2021)
    handleRequest(e) {
      let me = this;
      // xử lý (ngày tháng) trước khi gửi
      let employee = me.convertPreSend(me.employee);
      // trường hợp thêm mới
      me.loading = true;
      if (me.formMode == "add") {
        axios
          .post("http://localhost:8080/api/v1/Employees", employee)
          .then(async (response) => {
            console.log(response);
            // reset form và load lại dữ liệu
            await EventBus.$emit("loadDataServer");
            // nếu là ấn save thì ẩn form
            let mode = "Thêm mới";
            // nếu là lưu thì thự hiện đóng form
            if (e == "save") {
              me.cancelFormDetail();
              this.loading = false;
            }
            // nếu là lưu và thêm mới thì reset form, tăng mã nhân viên lớn nhất thêm 1
            else {
              me.resetForm();
              await me.increaseCode();
              me.loading = false;
              me.focusAndSelectAll();
              me.cEmployee = {...me.employee}
            }
            // hiện thông báo thành công
            EventBus.$emit("showSuccessBox", mode);
          })
          .catch((error) => {
            // nếu có lỗi xảy ra thì hiện message thông báo lỗi và focus vào trường sai nếu có
            console.log(error);
            this.loading = false;
            let message = error.response.data.messengers[0],
              field = error.response.data.eFieldError;
            console.log(field);
            EventBus.$emit("showError", message, field);
          });
      }

      // trường hợp chỉnh sửa
      else if (me.formMode == "edit") {
        // let url = "https//localhost:8080/api/v1/Employees";
        axios
          .put("http://localhost:8080/api/v1/Employees", employee)
          .then(async (response) => {
            // reset form và load lại dữ liệu
            await EventBus.$emit("loadDataServer");
            let mode = "Sửa";
            // nếu là ấn save thì ẩn form
            if (e == "save") {
              me.cancelFormDetail();
              this.loading = false;
            }
            // nếu là lưu và thêm mới thì reset form và tăng mã nhân viên lớn nhất thêm 1
            else {
              me.resetForm();
              await me.increaseCode();
              this.loading = false;
              me.focusAndSelectAll();
              me.cEmployee = {...me.employee}
            }
            // hiện thông báo thành công
            EventBus.$emit("showSuccessBox", mode);
          })
          .catch((error) => {
            // nếu có lỗi thì hiện message thông báo, focus vào ô nhập liệu sai nếu có
            this.loading = false;
            let message = error.response.data.messengers[0],
              field = error.response.data.eFieldError;
            EventBus.$emit("showError", message, field);
          });
      }
    },

    // xử lý sự kiên blur trên các input required
    // Createdby TuanNV (18/6/2021)
    handleBlur(e) {
      let value = e.target.value;
      let id = e.target.id;
      if (!value) {
        document.getElementById(id).classList.add("bgc-red");
        document
          .getElementById(id)
          .setAttribute("title", "Trường này không được để trống!");
      } else {
        document.getElementById(id).classList.remove("bgc-red");
        document.getElementById(id).removeAttribute("title");
      }
    },

    // hàm kiểm tra các input required có dữ liệu hay chưa
    // trả về true nếu đã đủ, false nếu có ít nhất 1 input chưa có
    // Createdby TuanNV (18/6/2021)
    checkInputRequired() {
      let check = true;
      let me = this;
      document.querySelectorAll("input[required]").forEach(function (el) {
        if (check) {
          if(!el.value){
             me.fieldMissingData = el.getAttribute("name");
            me.messageContent =
              me.convertToVNese(me.fieldMissingData) +
              " không được phép để trống.";
            me.focusAndSelectAll(me.fieldMissingData);
            check = false;
          }
        }
      });
      return check;
    },

    // hàm chuyển đổi tên trường thành tiếng việt
    // Createdby TuanNV (18/6/2021)
    convertToVNese(fieldName) {
      switch (fieldName) {
        case "EmployeeCode":
          return "Mã nhân viên";
        case "EmployeeName":
          return "Họ tên";
        case "DepartmentName":
          return "Đơn vị";
        case "EmployeePosition":
          return "Chức danh";
        case "DateOfBirth":
          return "Ngày sinh";
        case "IdentityNumber":
          return "Số CMND";
        case "IdentityDate":
          return "Ngày cấp";
        case "IdentityPlace":
          return "Nơi cấp";
        case "Address":
          return "Địa chỉ";
        case "PhoneNumber":
          return "ĐT di động";
        case "TelephoneNumber":
          return "ĐT cố định";
        case "Email":
          return "Email";
        case "BankAccountNumber":
          return "Số tài khoản";
        case "BankName":
          return "Tên ngân hàng";
        case "BranchName":
          return "Tên chi nhánh";
        default:
          return "";
      }
    },

    // hàm validate object trước khi thực hiện lưu
    // Createdby TuanNV (18/6/2021)
    validateObject() {
      let me = this;
      // validate mã nhân viên
      var code_regex = /^NV-/i;
      var obligatory = "NV-";
      let employeeCode = me.employee.employeeCode.trim();
      var numberCode_regex = /^-?\d+$/;
      me.fieldMissingData = "EmployeeCode";
      // trường hợp mã nhân viên ít hơn 4 kí tự
      if(employeeCode.length < 4){
        me.messageContent = "Mã nhân viên quá ngắn, vui lòng nhập lại.";
        return false;
      }

      // trường hợp mã nhân viên lớn hơn 8 kí tự
      else if(employeeCode.length > 8){
        me.messageContent = "Mã nhân viên quá dài, vui lòng nhập lại.";
        return false;
      }

      // trường hợp mã nhân viên không chứa kí tự bắt buộc
      else if (!code_regex.test(employeeCode)) {
        me.messageContent = "Mã nhân viên phải chứa kí tự " + obligatory;
        return false;
      } 
      
      // trường hợp mã nhân viên không đúng định dạng
      else {
        let numberInCode = employeeCode.substr(3);
        if (!numberCode_regex.test(numberInCode)) {
          me.messageContent =
            "Mã nhân viên không đúng định dạng (NV-xxxxx), vui lòng nhập lại.";
          return false;
        }
      }

      // validate đơn vị
      var isValidDepartment = false;
      for(let i=0; i<me.dataCbx.length; i++){
        if(me.employee.departmentName == me.dataCbx[i].text){
          isValidDepartment = true;
        }
      }
      if(!isValidDepartment){
        me.fieldMissingData = "DepartmentName";
        me.messageContent =
            "Đơn vị không hợp lệ, vui lòng nhập lại.";
          return false;
      }

      // validate ngày sinh
      if (!me.validateDate(me.employee.dateOfBirth)) {
        me.fieldMissingData = "DateOfBirth";
        me.messageContent = "Ngày sinh không hợp lệ, vui lòng nhập lại.";
        return false;
      }

      // validate ngày cấp
      if (!me.validateDate(me.employee.identityDate)) {
        me.fieldMissingData = "IdentityDate";
        me.messageContent = "Ngày cung cấp không hợp lệ, vui lòng nhập lại.";
        return false;
      }

      // validate email
      if (!me.validateEmail(me.employee.email)) {
        me.fieldMissingData = "Email";
        me.messageContent = "Email không hợp lệ, vui lòng nhập lại.";
        return false;
      }

      return true;
    },

    // hàm validate email
    // Createdby TuanNV (18/6/2021)
    validateEmail(email) {
      if (email == "" || email == null) return true;
      var reg_email =
        /^[a-zA-Z]{2}[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return reg_email.test(email);
    },

    // hàm validate ngày tháng
    // Createdby TuanNV (18/6/2021)
    validateDate(date) {
      if (date == "" || date == null) return true;
      var date_regex =
        /^(0[1-9]|1\d|2\d|3[01]|[0-9])\/(0[1-9]|1[0-2]|[0-9])\/(19|20)\d{2}$/;
      return date_regex.test(date);
    },

    // chuyển kiểu ngày từ yyyy-mm-dd sang dd/mm/yyyy
    // Createdby TuanNV (18/6/2021)
    formatDate(e) {
      if (!this.employee.dateOfBirth) return null;

      const [year, month, day] = this.employee.dateOfBirth.split("-");
      this.employee.dateOfBirth = `${day}/${month}/${year}`;
    },

    // hàm trả về ngày hiện tại dd/mm/yyyy
    // Createdby TuanNV (18/6/2021)
    getCurrentDay() {
      let today = new Date(),
        dd = String(today.getDate()).padStart(2, "0"),
        mm = String(today.getMonth() + 1).padStart(2, "0"),
        yyyy = today.getFullYear();
      return dd + "/" + mm + "/" + yyyy;
    },

    // sự kiện khi ấn tab
    // Createdby TuanNV (18/6/2021)
    tab() {
      this.$refs.EmployeeCode.focus();
    },

    // sự kiện khi ấn shift tab
    // Createdby TuanNV (18/6/2021)
    shiftTab() {
      this.$refs.saveAndAdd.focus();
    },

    // sự kiện khi chọn date ở date-picker ngày sinh
    // Createdby TuanNV (18/6/2021)
    chooseDateOfBirth() {
      let [year, month, day] = this.dateOfBirth_PK.split("-");
      this.employee.dateOfBirth = `${day}/${month}/${year}`;
      this.showDateOfBirth = false;
    },

    // sự kiện khi chọn date ở date-picker ngày cấp
    // Createdby TuanNV (18/6/2021)
    chooseIdentityDate() {
      let [year, month, day] = this.identityDate_PK.split("-");
      this.employee.identityDate = `${day}/${month}/${year}`;
      this.showIdentityDate = false;
    },

    // sự kiện click vào form để ẩn các popup
    // Createdby TuanNV (18/6/2021)
    clickForm(e) {
      var picker_dateofbirth = document.getElementById("picker_dateofbirth");
      if (picker_dateofbirth != null) {
        if (!picker_dateofbirth.contains(e.target)) {
          this.showDateOfBirth = false;
        }
      }

      var picker_identityDate = document.getElementById("picker_identitydate");
      if (picker_identityDate != null) {
        if (!picker_identityDate.contains(e.target)) {
          this.showIdentityDate = false;
        }
      }
    },

    // binding đơn vị khi người dùng chọn ở combobox
    // Createdby TuanNV (18/6/2021)
    selectDepartment(departmentName) {
      this.employee.departmentName = departmentName;
    },

    // chuyển từ dd/mm/yyyy sang yyyy-mm-dd để gửi tới server
    // Createdby TuanNV (18/6/2021)
    convertToDateServer(date) {
      var [day, month, year] = date.split("/");
      if (
        day != undefined &&
        month != undefined &&
        year != undefined &&
        year != ""
      )
        return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },

    // hàm tăng mã nhân viên lớn nhất trong csdl thêm 1
    // Createdby TuanNV (18/6/2021)
    async increaseCode() {
      let url = "http://localhost:8080/api/v1/Employees/GetMaxCode";
      let maxCode = "";
      let me = this;
      await axios
        .get(url)
        .then((res) => {
          maxCode = res.data.substr(3);
          maxCode = parseInt(maxCode) + 1;
          maxCode = maxCode.toString().padStart(5, "0");
          if (maxCode < 100000)
            me.employee.employeeCode = res.data.substr(0, 3) + maxCode;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },

  // khởi tạo xong thì thực hiện trước tăng mã nhân viên lớn nhất lên 1
  created() {
    this.increaseCode();
  },

  mounted() {
    let me = this;
    // bắt sự kiện nhấn phím sửa thông tin nhân viên
    // Createdby TuanNV (18/6/2021)
    EventBus.$on("editEmployee", (employee) => {
      if (employee != null) {
        me.employee = { ...employee };
        me.formMode = "edit";
        me.showForm();
      }
    });

    // bắt sự kiên thêm mới hoặc clone nhân viên nhân viên
    // nếu là clone thì nhận được 1 employee đầu vào
    // Createdby TuanNV (18/6/2021)
    EventBus.$on("addEmployee", async (employee) => {
      if (employee != undefined) {
        let newCode = me.employee.employeeCode;
        me.employee = { ...employee };
        me.employee.employeeCode = newCode;
      }
      me.formMode = "add";
      me.showForm();
    });

    // bắt sự kiện focus và chọn hết nếu có trong ô nhập liệu
    // Createdby TuanNV (18/6/2021)
    EventBus.$on("focusAndSelectAll", (field) => {
      me.focusAndSelectAll(field);
    });

    // bắt sự kiện đóng form detail
    // Createdby TuanNV (18/6/2021)
    EventBus.$on("cancelFormDetail", () => {
      this.cancelFormDetail();
    });
  },

  // cập nhật ngày tháng trong input date và date picker nếu 1 trong 2 thay đổi
  // Createdby TuanNV (18/6/2021)
  updated: function () {
    this.$nextTick(function () {
      if (!this.employee.dateOfBirth) return null;
      this.dateOfBirth_PK = this.convertToDateServer(this.employee.dateOfBirth);
      // return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`

      if (!this.employee.identityDate) return null;
      this.identityDate_PK = this.convertToDateServer(
        this.employee.identityDate
      );
    });
  },
};
</script>

<style scope src="./employee_detail.css"></style>
<style src="../common/common_css/googlefont.css"></style>
