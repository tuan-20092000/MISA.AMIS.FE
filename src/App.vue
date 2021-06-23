<template>
  <div id="app">
    <div class="wrapper">
        <SideBar/>
        <div class="content">
          <div>
            <Header/>
            <router-view></router-view>
          </div>
        </div>
    </div>
    <EmployeeDetail />
    <MessageBoxDataChange />
    <MessageBoxError/>
    <MessageBoxSuccess/>
    <MessageBoxWarning/>
  </div>
</template>

<script>
import SideBar from './components/layout/the-sidebar/the-sidebar.vue';
import Header from './components/layout/the-header/the-header.vue';
import EmployeeDetail from './components/employee_detail/employee_detail.vue';
import MessageBoxDataChange from './components/messagebox/messagebox_datachange/messagebox_datachange.vue';
import MessageBoxError from './components/messagebox/messagebox_error/messagebox_error.vue';
import MessageBoxSuccess from './components/messagebox/messagebox_success/messagebox_success.vue';
import MessageBoxWarning from './components/messagebox/messagebox_warning/message_warning.vue';

export default {

  components: {
    SideBar, Header, EmployeeDetail, MessageBoxDataChange, MessageBoxError, MessageBoxSuccess, MessageBoxWarning
  },
}
</script>

<style src="./css/app.css"></style>
<style src="./components/common/common_css/googlefont.css"></style>
