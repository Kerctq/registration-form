<template>
  <div class="container mt-4">
      <div class="row">  
        <div class="col-sm-4 mx-auto">
          <form @submit.prevent="registerUser" novalidate>

            <div v-show="step === 1" class="step">

              <div class="mb-3">
                <label for="name" class="form-label">Your Name</label>
                <input v-model="formReg.name" type="text" class="form-control" id="name">
              </div>

              <div class="mb-3">
                <label for="secondname" class="form-label">Your Second Name</label>
                <input v-model="formReg.secondname" type="text" class="form-control" id="secondname">
              </div>

              
              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input v-model="formReg.email" type="text" class="form-control" id="email">
              </div>

             

              <button @click="nextStep" type="Button" class="btn btn-primary">Next Step</button>
            
            </div>


            <transition name="slide-fade">
                <div v-show="step === 2" class="step">

                  <div class="mb-3">
                    <label for="password" class="form-label">Password</label>
                    <input v-model="formReg.password" type="password" class="form-control" id="password">
                  </div>

                  <div class="mb-3">
                    <label for="pswdconfirm" class="form-label">Pswd Confirm</label>
                    <input v-model="formReg.pswdconfirm" type="password" class="form-control" id="pswdconfirm">
                  </div>             

                  <button @click="nextStep" type="Button" class="btn btn-primary mr-2">Next Step</button>
                  <button @click="prevStep" type="Button" class="btn btn-light">Prev Step</button>
                
                </div>
            </transition>

            <transition name="slide-fade">
              <div v-show="step === 3" class="step">

              <div class="mb-3">
                <label for="country" class="form-label">Country</label>
                <input v-model="formReg.country" type="text" class="form-control" id="Country">
              </div>

              <div class="mb-3">
                <label for="city" class="form-label">City/Town</label>
                <input v-model="formReg.city" type="text" class="form-control" id="city">
              </div>             

              <button type="submit" class="btn btn-primary mr-2">Register</button>
              <button @click="prevStep" type="Button" class="btn btn-light">Prev Step</button>
            
            </div>
          </transition>

          </form>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  
  data() {
    return{
      step: 1,
      formReg:{
        name:'',
        secondname:'',
        email:'',
        password:'',
        pswdconfirm:'',
        country: '',
        city:''
      }
      } 
  },
  methods:{
    nextStep(){
      if (this.step < 3) {
        this.step++
      }
    },
    prevStep(){
      if (this.step > 1) {
      this.step--
      }
    },
    registerUser(){
      console.log("Register completed!")
    }
  }
  
}
</script>
 
<style>

.slide-fade-enter-active {
  transition: all .8s ease;
}
.slide-fade-leave-active {
  transition: all .01s cubic-bezier(1.0, 0.2, 0.1, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active до версии 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}

</style>
