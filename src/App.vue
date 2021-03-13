<template>
  <div id="app">
    <div class="container w-50">
      <div class="card">
        <h3 class="card-header text-center">
          Input Validation
        </h3>
        <div class ="card-body">
          <form>
            <!-- <div class="form-row"> -->
              
              <div class="form-group">
                <label>Text</label>
                <input type="text" class="form-control"
                v-model="$v.text.$model" :class="{'is-invalid':$v.text.$error, 'is-valid':!$v.text.$invalid}">
                <div class="invalid-feedback">
                  <span v-if="!$v.text.required">text is required.</span>
                  <span v-if="!$v.text.maxLength">text cannot be longer than {{$v.text.$params.maxLength.max}} characters.</span>  
                </div>  
              </div>

              <div class="form-group">
                <label>Textarea</label>
                <textarea type="textarea" class="form-control"
                v-model="$v.textarea.$model" :class="{'is-invalid':$v.textarea.$error, 'is-valid':!$v.textarea.$invalid}"/>
                <div class="invalid-feedback">
                  <span v-if="!$v.textarea.required">textarea is required.</span>
                  <span v-if="!$v.textarea.maxLength">textarea cannot be longer than {{$v.textarea.$params.maxLength.max}} characters.</span>  
                  <span v-if="!$v.textarea.numbersAndSpaces">textarea can only contain numbers and spaces.</span>  
                </div>  
              </div>

              <div class="form-group">
                <label>Date</label>
                <input type="text" class="form-control"
                v-model="$v.date.$model" :class="{'is-invalid':$v.date.$error, 'is-valid':!$v.date.$invalid}">
                <div class="invalid-feedback">
                  <span v-if="!$v.date.required">date is required.</span>
                  <span v-if="!$v.date.validDate">date must match format dd/mm/yyyy.</span>  
                </div>  
              </div>

              <button @click.prevent="submit"  
                :class="$v.$invalid?'btn btn-secondary':'btn btn-primary'"
              >
                Submit
              </button>
            <!-- </div> -->
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {required, maxLength} from 'vuelidate/lib/validators'
import moment  from "moment"

const validDate = (value) => 
  moment(value, 'DD/MM/YYYY', true).isValid()

const numbersAndSpaces = (value) =>
  /^[0-9 ]+$/.test(value)

export default {
  name: 'App',
  data: function(){
    return {
      text:"",
      textarea: "",
      date:""
    }
  },
  methods:{
    submit(){
      this.$v.$touch() 
      if (!this.$v.$invalid) { 
        console.log("submit")

        // to make testing easier
        window.alert("submit")
      }
    }
  },
  validations: {
    text: {
      required,
      maxLength: maxLength(20),
    },
    textarea: {
      required,
      maxLength: maxLength(50),
      numbersAndSpaces
    },
    date:{
      required,
      validDate
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
