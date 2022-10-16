<template>

    <div id="login">   
      <h1>Welcome Back!</h1>
      
      <Form @submit="login" method="post" :validation-schema="loginFormSchema">
      
        <div class="field-wrap">
        <label>
          Email Address<span class="req">*</span>
        </label>
        <!-- <Field type="email" name="email" :rules="emailRule"/> -->
        <Field type="email" name="email" :validateOnInput="true"/>
        <span class="error-alert"><ErrorMessage name="email" /></span>
      </div>
      
      <div class="field-wrap">
        <label>
          Password<span class="req">*</span>
        </label>
        <!-- <Field type="password" name="password" :rules="passwordRule" /> -->
        <Field type="password" name="password" :validate-on-input="true" />
        <span class="error-alert"><ErrorMessage name="password" /></span>
      </div>
      
      <p class="forgot"><a href="#">Forgot Password?</a></p>
      
      <button class="button button-block">Log In</button>
      
      </Form>

    </div>
        
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import { object, string } from 'yup';

export default {
  name: "LoginApp",
  components:{
    Form, 
    Field,
    ErrorMessage,
  },
  data() {
    const loginFormSchema = object({
      email: string().required("Enter email!!!").email("This email is invalid!!!"),
      password: string().required("Enter password!!!").min(5, "This password must be greater than 5 character!!!"),
    })
    return{
      // email: '',
      // password: '',
      // emailRule: yup.string().required("Enter email!").email("This email is invalid!"),
      // passwordRule: yup.string().required("Enter password!").min(5, "This password must be greater than 5 character!"),
      loginFormSchema,
    };
  },
  methods: {
    login(values){
      console.log(values)
    },
    // validateEmail(email)
    // {
    //   if(!email)
    //     return "Enter email."
    //   const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
    //   if (!regex.test(email)) {
    //     return 'This email is invalid.';
    //   }
    //   return true;
    // },
    // validatePassword(password)
    // {
    //   if(!password) return "Enter password.";
    //   if(password.lenght < 5) return 'This password must be greater than 5 character';
    //   return true;
    // }
  },
}
</script>

<style>

</style>