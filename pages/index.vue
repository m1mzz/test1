<template>
  <div class="fake-body">
    <div class="p20">
      <div class="container">
        <h1 class="h1">Contacts</h1>
      </div>
      <div>
        <div class="container">
          <div class="row pb">
            <div class="col-6">
              <Contacts/>
            </div>
            <div class="col-6">
              <Support/>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="background-for-map">
      <Offices/>
    </div>
    <div class="fake-footer background-pattern"></div>
  </div>
</template>

<script>
import Contacts from '~/components/page/contact/Contacts.vue'
import Support from '~/components/page/contact/Support.vue'
import Offices from '~/components/page/contact/Offices.vue'
export default {
  name: 'ContactPage',
  components: {
    Contacts,
    Support,
    Offices
  },
  head() {
    return {
      title: 'Contact',
      meta: [
        { hid: 'og:title', name: 'og:title', content: 'About Our Company' },
        { hid: 'og:type', name: 'og:type', content: 'article' },
        {
          hid: 'og:url',
          name: 'og:url',
          content: 'http://www.mysite.com/article/'
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content: 'http://www.mysite.com/articleimage.jpg'
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          content: 'My Company Name'
        },
        { hid: 'fb:app_id', name: 'fb:app_id', content: '1234567890987654321' },
        {
          hid: 'og:description',
          name: 'og:description',
          content: 'A description of our services and company profile.'
        }
      ]
    }
  }
}
</script>

<style>
.fake-body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
.fake-footer {
  flex: auto;
}
.background-for-map {
  background: #262626;
}
</style>
