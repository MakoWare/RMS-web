<template>
  <div id="loginModal" class="modal login-modal">
    <div class="modal-content">
      <h5>Login</h5>
      <div class="row">
        <form class="col s12">
          <div class="row">
            <div class="input-field col s12">
              <input id="email" type="email" class="validate" v-model="email" data-error="wrong" data-success="right">
              <label for="email">Email</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input id="password" type="password" class="validate" v-model="password">
              <label for="password">Password</label>
            </div>
          </div>
        </form>
      </div>
      <p v-if="hasError" class="login-error">Invalid Login</p>
    </div>
    <div class="modal-footer">
      <a href="#" class="modal-action modal-close waves-effect waves-red btn-flat sign-up-button" @click.stop="openSignUpModal">Sign Up</a>
      <a href="#" class="modal-action waves-effect waves-green btn-flat" @click.stop="login">Submit</a>
    </div>
  </div>
</template>

<script>
 import UserModel from '../../models/UserModel.js'
 import notifications from '../../services/NotificationService'

 export default {
   name: 'LoginModal',
   el: '#loginModal',
   data(){
     return {
       email: "",
       password: "",
       hasError: false
     }
   },
   ready(){

   },

   methods: {
     openSignUpModal(){
       notifications.notify('SignUpModal.open');
     },

     login(){
       this.hasError = false;
       notifications.notify("Overlay.setVisible", true);
       UserModel.login(this.email, this.password).then(results => {
         notifications.notify("Overlay.setVisible", false);
         $("#loginModal").closeModal();
       }, error => {
         this.hasError = true;
         notifications.notify("Overlay.setVisible", false);
       });
     }
   }
 }
</script>

<style lang="sass">

 #loginModal {
   .login-error {
     text-align: center;
     color: red;
     margin: 0px;
   }

   .sign-up-button {
     float: none !important;
   }

   .row {
     margin-bottom: 0px;
   }
 }


</style>
