<template>
  <h1 id="center-text">You can contact me via my email: men@umass.edu or matthew.en.c@gmail.com</h1>

  <BContainer class="d-flex justify-content-center align-items-center">
    <BRow align-v="center" class="g-10">
      <BCol>
        <BCard
          title="Linkedin link"
          :img-src="linkedinLogo"
          img-alt="Image"
          img-top
          tag="Linkedin"
          style="max-width: 20rem"
        >
          <BButton href="https://www.linkedin.com/in/matthewen25/" target="_blank" variant="primary"
            >To Linkedin</BButton
          >
        </BCard>
      </BCol>
      <BCol>
        <BCard
          title="Github link"
          :img-src="githubLogo"
          img-alt="Image"
          img-top
          tag="Github"
          style="max-width: 20rem"
        >
          <BCardText> </BCardText>

          <BButton href="https://github.com/Mcen25" target="_blank" variant="primary"
            >To Github</BButton
          >
        </BCard>
      </BCol>
    </BRow>
  </BContainer>
  <h1 id="center-text">Or you can reach me through this contact form:</h1>

  <BContainer class="p-3">
    <BForm @submit="onSubmit" @reset="onReset" v-if="show">
      <BFormGroup id="input-group-3" label="Category:" label-for="input-3">
        <BFormSelect id="input-3" v-model="form.About" :options="category" required />
      </BFormGroup>
      <BFormGroup
        id="input-group-1"
        label="Feedback Form:"
        label-for="input-1"
        description="Tell me what you think!"
      >
        <BFormInput
          id="input-1"
          v-model="form.Feedback"
          type="text"
          placeholder="Write your feedback here"
          required
        />
      </BFormGroup>
      <BButton type="submit" variant="primary">Submit</BButton>
      <BButton type="reset" variant="danger">Reset</BButton>
    </BForm>

    <BCard class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </BCard>
  </BContainer>
</template>

<script lang="ts">
import { defineComponent, nextTick } from 'vue'
import {
  BContainer,
  BRow,
  BCol,
  BCard,
  BButton,
  BForm,
  BFormGroup,
  BFormInput,
  BFormSelect,
  BFormCheckboxGroup,
  BFormCheckbox,
  BCardText
} from 'bootstrap-vue-next'

import linkedinLogo from '../assets/LinkedIn_icon.svg.png'
import githubLogo from '../assets/GitHub_Invertocat_Logo.svg.png'
import axios from 'axios'

export default defineComponent({
  name: 'ContactView',
  components: {
    BContainer,
    BRow,
    BCol,
    BCard,
    BButton,
    BForm,
    BFormGroup,
    BFormInput,
    BFormSelect,
    BFormCheckboxGroup,
    BFormCheckbox,
    BCardText
  },
  data() {
    return {
      category: [
        { text: 'Select One', value: null },
        'Home',
        'About',
        'Projects',
        'Constellation Website',
        'Game Jam #14'
      ],
      linkedinLogo,
      githubLogo,
      form: {
        Date: '',
        Feedback: '',
        About: ''
      },
      show: true
    }
  },
  mounted() {
    const currentDate = new Date().toISOString().split('T')[0]
    this.form.Date = currentDate
  },
  methods: {
    async onSubmit(event: Event) {
      event.preventDefault()

      console.log('Form Data:', this.form)

      try {
        const response = await axios.post('http://localhost:3000/api/form', this.form)
        alert('Form submitted successfully!')
        this.form.Date = new Date().toISOString().split('T')[0]
      } catch (error: any) {
        alert('Error submitting form: ' + error.message)
      }
    },
    onReset(event: Event) {
      event.preventDefault()
      this.form.Date = ''
      this.form.Feedback = ''
      this.form.About = ''
      this.show = false
      nextTick(() => {
        this.show = true
      })
    }
  }
})
</script>

<style scoped>
#center-text {
  text-align: center;
  font-size: 3rem;
  font-weight: 700;
  line-height: 1.5;
  margin-bottom: 1rem;
  background: linear-gradient(to top, #2217bd, #2e1857);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  color: transparent;
}
</style>
