<template>
  <div class="container mt-4">
      <div class="row">  
        <div class="col-sm-4 mx-auto">
          <form @submit.prevent="registerUser" novalidate>

              <!-- <pre>{{$v.formReg.name}}</pre> -->

            <div v-show="step === 1" class="step">

              <div class="mb-3">
                <label for="name" class="form-label">Your Name</label>
                <input @blur="$v.formReg.name.$touch()" 
                       v-model="formReg.name" 
                       type="text" 
                       class="form-control" 
                       id="name"
                       :class="status(this.$v.formReg.name)">

                  <!-- Классы можно перечислять :class="{'is-invalid reqIn': $v.formReg.name.$error, 'reqIn': $v.formReg.name.$error}"> -->
              
                  <div v-if="!$v.formReg.name.$required" class="invalid-feedback">
                    Please provide a valid name.
                  </div>

                  <div v-if="!$v.formReg.name.$minLength" class="invalid-feedback">
                    Length of name is short.
                  </div>

                  <div v-if="!$v.formReg.name.alpha" class="invalid-feedback">
                    {{msgNotNum}}
                  </div>

              </div>

              <div class="mb-3">
                <label for="secondname" class="form-label">Your Second Name</label>
                <input @blur="$v.formReg.secondname.$touch()"
                v-model="formReg.secondname" 
                type="text" 
                class="form-control" 
                id="secondname"
                :class="status(this.$v.formReg.secondname)">

                <div v-if="!$v.formReg.secondname.$required" class="invalid-feedback">
                    Please provide a valid second name.
                </div>

                    <div v-if="!$v.formReg.secondname.alpha" class="invalid-feedback">
                    {{msgNotNum}}
                  </div>
              
              </div>

              
              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input 
                       @blur="$v.formReg.email.$touch()"
                       v-model="formReg.email" 
                       type="text" 
                       class="form-control" 
                       id="email"
                       :class="status(this.$v.formReg.email)">
              
              <div v-if="!$v.formReg.email.$required && !$v.formReg.email.email" class="invalid-feedback">
                    Please provide a valid email.
              </div>

              </div>


              <button @click="nextStep" 
                      type="Button" 
                      class="btn btn-primary"
                      :disabled="disabledButton1"
                      >Next Step</button>
            
            </div>


            <transition name="slide-fade">
                <div v-show="step === 2" class="step">

                  <div class="mb-3">
                    <label for="password" class="form-label">Password</label>
                    <input @blur="$v.formReg.password.$touch()"
                    v-model="formReg.password" 
                    type="password" 
                    class="form-control" 
                    id="password"
                    :class="{'is-invalid reqIn': $v.formReg.password.$error}">

                   <div v-if="!$v.formReg.password.required" class="invalid-feedback">
                    Please provide a valid password.
                  </div>

                  <div v-if="!$v.formReg.password.minLength" class="invalid-feedback">
                    At least 6 characters.
                  </div>

                  </div>


                  <div class="mb-3">
                    <label for="pswdconfirm" class="form-label">Pswd Confirm</label>
                    <input @blur="$v.formReg.pswdconfirm.$touch()"
                    v-model="formReg.pswdconfirm" 
                    type="password" class="form-control" 
                    id="pswdconfirm"
                    :class="{'is-invalid reqIn': $v.formReg.pswdconfirm.$error}">

                    <div v-if="!$v.formReg.pswdconfirm.sameAs" class="invalid-feedback">
                    Inputed data has to be same as password
                  </div>      

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

// eslint-disable-next-line no-unused-vars
import { required, minLength, between, email, helpers, sameAs } from 'vuelidate/lib/validators'
const alpha = helpers.regex('alpha', /^[a-zA-Zёа-яЁА-Я]*$/)

export default {
  
  data() {
    return{
      step: 1,
      msgNotNum:"There's should be no numbers in field",
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
  computed:{
    disabledButton1(){
      return this.$v.formReg.name.$invalid ||
      this.$v.formReg.secondname.$invalid ||
      this.$v.formReg.email.$invalid 
    }
  },
  methods:{
    status(validator){
      return{
        'is-invalid reqIn': validator.$error
        }
    },
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
      console.log(`Register completed!`)

      for (let input in this.formReg){
        console.log(this.formReg[input])
      }

      this.step = 1
      for (let input in this.formReg){
        this.formReg[input] = ''
      }
      this.$v.$reset()
    }
  },
  
  validations:{
      formReg:{
        name:{
          required,
          minLength: minLength(3),
          alpha
        },
        secondname:{
          required,
          minLength: minLength(3),
          alpha
        },
          email:{
          required,
          email
        },
        password:{
          required,
          minLength: minLength(6)
        },
        pswdconfirm:{
          sameAs: sameAs('password')
        }
      }
  }
}
</script>
 
<style>

.reqIn{
  background: rgb(238, 129, 78);
}

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
