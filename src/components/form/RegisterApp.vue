<template>
  
    <div id="signup">   
      <h1>Sign Up for Free</h1>
      
      <Form @submit="register" method="post" :validation-schema="registerFormSchema" :initial-values="formData">
      
      <div class="top-row">
        <div class="field-wrap">
          <label>
            First and Last Name<span class="req">*</span>
          </label>
          <Field type="text" name="name" :validateOnInput="true"/>
        <span class="error-alert"><ErrorMessage name="name" /></span>
        </div>
        <div class="field-wrap">
          <label>
            Email Address<span class="req">*</span>
          </label>
          <Field type="email" name="email" :validateOnInput="true"/>
        <span class="error-alert"><ErrorMessage name="email" /></span>
        </div>
      </div>
      <div class="field-wrap">
        <label>
          Set A Password<span class="req">*</span>
        </label>
        <Field type="password" name="password" :validateOnInput="true"/>
        <span class="error-alert"><ErrorMessage name="password" /></span>
      </div>
      <div class="field-wrap">
        <label>
          Confirm A Password<span class="req">*</span>
        </label>
        <Field type="password" name="confirmPassword" :validateOnInput="true"/>
        <span class="error-alert"><ErrorMessage name="confirmPassword" /></span>
      </div>
      <div class="field-wrap">
        <label>
          Phone Number<span class="req">*</span>
        </label>
        <Field type="text" name="phoneNumber"/>
        <span class="error-alert"><ErrorMessage name="phoneNumber" :validateOnInput="true"/></span>
      </div>

      <div class="field-wrap">
        <label>
          Role<span class="req">*</span>
        </label>
        <Field as="select" class="form-style" name="role">
          <option value="">Select Role</option>
          <option value="user">User</option>
          <option value="student">Student</option>
          <option value="teacher">Teacher</option>
        </Field>
        <span class="error-alert"><ErrorMessage name="role" /></span>
      </div>

      <div class="field-wrap">
        <label>
          Ananymous
        </label>
        <Field type="radio" id="1" value="Ananymous" name="gender" />
      </div>
      <div class="field-wrap">
        <label>
          Female
        </label>
        <Field type="radio" id="2" value="Female" name="gender" />
      </div>
      <div class="field-wrap">
        <label>
          Male
        </label>
        <Field type="radio" id="3" value="Male" name="gender" />
      </div>
      
      <button class="button button-block">Register</button>
      
      </Form>

    </div>
    
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import { object, string, ref } from 'yup';

export default {
  name: "RegisterApp",
  components:{
    Form, 
    Field,
    ErrorMessage,
  },
  data() {
    const registerFormSchema = object({
      name: string().required(),
      email: string().required("Enter email!!!").email("This email is invalid!!!"),
      phoneNumber: string().required().min(12).max(11),
      password: string().required("Enter password!!!").min(5, "This password must be greater than 5 character!!!"),
      confirmPassword: string().required("Enter confirm password!!!").oneOf([ref('password'), null], 'the password is not equal!'),
      role: string().required()
    })
    return {
      registerFormSchema,
      formData: {
        gender: "Ananymous",
        role: "user"
      }
      // name: '',
      // email: '',
      // password: '',
      // confirmPassword: '',
      // phoneNumber: '',
      // role: 'user',
      // gender: 'Ananymous',
      // isOk: false,
    }
  },
  methods: {
    register(values){
      console.log(values)
      // this.validatieProperties();
      // if(this.isOk){
      //   console.log(this.name);
      //   console.log(this.email);
      //   console.log(this.password);
      //   console.log(this.confirmPassword);
      //   console.log(this.phoneNumber);
      //   console.log(this.role);
      //   console.log(this.gender);
      // }
    },
    // validatieProperties(){
    //   if(validateEmail(this.email)==false){
    //     this.$toast.error("Email is wrong");
    //   }else if(this.phoneNumber.length < 11 || this.phoneNumber.length > 11){
    //     this.$toast.error("Phone number must be 11 digit");
    //   }else if(!this.name){
    //     this.$toast.error("The name is empty!");
    //   }else if(!this.password){
    //     this.$toast.error("The password is empty!");
    //   }else if(!this.confirmPassword){
    //     this.$toast.error("The confirm password is empty!");
    //   }else{
    //     this.isOk = true;        
    //   }
    // },
  },
};

// function validateEmail(email)
// {
//   var re = /\S+@\S+\.\S+/;
//   return re.test(email);
// }

</script>

<style>

</style>