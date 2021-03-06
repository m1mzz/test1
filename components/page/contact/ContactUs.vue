<template>
  <section id="contact_form" class="container py-10 text-white">
    <h2 class="h2">Contact Us</h2>
    <div class="row">
      <form class="col-6 pr-10-md" :action="sendUrl" method="post" @submit.prevent="onSubmit">
        <div v-for="(input, name) in form" :key="name">
          <ContactInput
            :name="name"
            :value="input.value"
            :type="input.type"
            :error="input.error"
            :errorMsg="input.errorMsg"
            @onInput="onInput"
          />
        </div>
        <input
          v-model="agree"
          id="agree"
          type="checkbox"
          class="custom-checkbox visually-hidden"
          name="agree"
        >
        <label for="agree" class="fs-18">I agree the processing of personal data</label>
        <button
          type="submit"
          class="send-btn btn-reset bold upper mt-20"
          :disabled="disabledSubmit"
        >
          Get in touch
          <span v-if="sending" class="spin"></span>
        </button>
        <span v-if="errorSend || successSend" class="h4 bold ml-10 d-i-block">
          Send
          {{ successSend ? 'success' : '' }}
          {{ errorSend ? 'error' : '' }}
        </span>
      </form>
      <div class="col-6 pl-10-md">
        <p
          class="text-black mt-2 fs-18"
        >Please tell us more about your request and give us info about your company and country</p>
      </div>
    </div>
  </section>
</template>
<script>
import pattern from '~/assets/js/RegExpPattern'
import ContactInput from '@/components/wrapper/ContactInput'
import Vue from 'vue'
export default {
  name: 'ContactUs',
  components: {
    ContactInput
  },
  data() {
    return {
      sendUrl: 'https://httpbin.org/post',
      form: {
        Name: {
          value: '',
          errorMsg: 'Enter valid name',
          pattern: pattern.name,
          type: 'text'
        },
        Phone: {
          value: '',
          errorMsg: 'Enter valid phone',
          pattern: pattern.phone,
          type: 'tel'
        },
        'E-mail': {
          value: '',
          errorMsg: 'Enter valid e-mail',
          pattern: pattern.email,
          type: 'email'
        }
      },
      errorInputs: [],
      agree: false,
      sending: false,
      errorSend: false,
      successSend: false,
      withoutJs: true
    }
  },
  computed: {
    disabledSubmit() {
      if (this.withoutJs) return false
      if (!this.agree) return true
      for (const key in this.form) {
        const item = this.form[key]
        if (!item.activated || item.error) {
          return true
        }
      }
      return false
    }
  },
  created() {
    for (const key in this.form) {
      const item = this.form[key]
      Vue.set(item, 'error', false)
      Vue.set(item, 'activated', false)
    }
  },
  mounted() {
    this.withoutJs = false
  },
  methods: {
    onInput(name, value) {
      const pattern = this.form[name].pattern
      const error = !pattern.test(value)
      const index = this.errorInputs.indexOf(name)

      this.form[name].value = value
      if (!this.form[name].activated) {
        this.form[name].activated = true
      }
      if (index === -1 && error) {
        this.errorInputs.push(name)
        this.form[name].error = true
      } else if (!error) {
        this.errorInputs.splice(index, 1)
        this.form[name].error = false
      }
    },
    onSubmit() {
      this.successSend = false
      this.errorSend = false
      this.sending = true
      const sendForm = {}
      for (const key in this.form) {
        const item = this.form[key]
        sendForm[key] = item.value
      }
      this.$axios
        .$post(this.sendUrl, sendForm)
        .then(({ data }) => {
          this.clearForm()
          this.successSend = true
          this.sending = false
        })
        .catch(e => {
          this.sending = false
          this.errorSend = true
        })
    },
    clearForm() {
      for (const key in this.form) {
        const item = this.form[key]
        item.value = ''
        item.activated = false
      }
      this.agree = false
    }
  }
}
</script>
<style lang="scss" scoped>
.send-btn {
  padding: 30px 90px;
  background: #262626;
  font-size: 11px;
  transition: background-color 0.3s;

  &:disabled {
    cursor: not-allowed;
    background: gray;
  }

  &:focus {
    outline: none;
    border-bottom: 3px solid #fff;
  }
}
</style>
