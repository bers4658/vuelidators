<template>
  <div class="container">
    <form class="pt-3" @submit.prevent="onSubmit">
      <div class="form-group pt-3">
        <label for="email">Email</label>
        <input
          type="email"
          id="email" 
          class="form-control"
          :class="{'is-invalid': $v.email.$error}"
          @blur="$v.email.$touch()"
          v-model="email"
          > 
          <div  class="invalid-feedback" v-if="!$v.email.required">Email field is required</div>
          <div  class="invalid-feedback" v-if="!$v.email.email">This field should be an email</div>
          <div  class="invalid-feedback" v-if="!$v.email.uniqEmail">This emai is already exists</div>
      </div>

      <div class="form-group pt-3">
        <label for="password">Password</label>
        <input
          type="password"
          id="password" 
          class="form-control"
          :class="{'is-invalid': $v.password.$error}"
          @blur="$v.password.$touch()"
          v-model="password"
          > 
          <div  class="invalid-feedback" v-if="!$v.email.minLength">
            Min length of password is {{ $v.password.$params.minLength.min }}. Now it is {{ password.length }}
          </div>
      </div>

      <div class="form-group pt-3">
        <label for="confirm">Confirm pasword</label>
        <input
          type="password"
          id="confirm" 
          class="form-control"
          :class="{'is-invalid': $v.confirmPassword.$error}"
          @blur="$v.confirmPassword.$touch()"
          v-model="confirmPassword"
          > 
          <div  class="invalid-feedback" v-if="!$v.confirmPassword.sameAs">
            Passwords should match
          </div>
      </div>
       <!-- {{ $v }} -->

       <button
        class="btn btn-success mt-3"
        :disabled="$v.$invalid"
        >Submit</button>
    </form>
    
  </div>
</template>

<script>
import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'

export default {
  data () {
    return {
      form: {
        password: '',
      },
        email: '',
        password: '',
        confirmPassword: '',
        
    }
  },
  methods: {
    onSubmit () {
      console.log('Email', this.email);
      console.log('Password', this.password);
    }
  },
  validations: {
    email: {
      required,
      email,
      uniqEmail: function(newEmail) {
        if (newEmail === '') return true

        return new Promise((resolve) => {
          setTimeout(() => {
            const value = newEmail !== 'test@mail.ru'
            resolve(value)
          }, 1000)
        } )
      }
    },
    password: {
      minLength: minLength(6)
    },
    confirmPassword: {
      sameAs: sameAs('password')
    }
  },

  components: {

  }
  
}
</script>

<style scoped>

</style>
