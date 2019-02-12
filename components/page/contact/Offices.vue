<template>
  <div :class="{'theme-dark': themeDark}" class="of-hidden pl-20-md pr-20-md">
    <section class="container">
      <div class="row">
        <div class="col-6 big-padding py-5-md-down">
          <h2 class="h2">Our Offices</h2>
          <ul class="tab-head list-reset row">
            <li v-for="({}, name) in tabs" :key="name" class="d-i-block">
              <button
                class="btn-reset tab-btn upper p-3"
                :class="{'highlight-text': name === activeTab}"
                @click="name === activeTab ? '' : tabClick(name)"
              >{{ name }}</button>
            </li>
          </ul>
          <div class="tab-body">
            <div v-for="({title, address}, name) in tabs" v-show="name === activeTab" :key="name">
              <h3 class="h3">{{ title }}</h3>
              <p v-html="address"/>
            </div>
          </div>
        </div>
        <div class="col-6 p-0 map-min-height mw-300" :class="{'pos-rel': fullWidth}">
          <div class="gmap-container map-min-height" :class="{gmapFullWidth: fullWidth}">
            <GmapMap
              :center="coords"
              :zoom="11"
              style="width: 100%; height: 100%"
              :options="gmapOptions"
            >
              <GmapMarker
                :position="coords"
                :clickable="true"
                :draggable="true"
                icon="/image/gmapCIrcle.png"
              />
            </GmapMap>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
const gmapStyles = require('@/assets/js/googleMapStyle.json')
export default {
  name: 'ContactOffices',
  props: {
    themeDark: {
      default: true,
      type: Boolean
    },
    fullWidth: {
      default: true,
      type: Boolean
    }
  },
  data() {
    return {
      gmapOptions: {
        styles: gmapStyles,
        disableDefaultUI: true
      },
      tabs: {
        Kyiv: {
          coords: {
            lat: 50.4909116,
            lng: 30.4749767
          },
          title: 'Global Message Services Ukraine LLC',
          address: 'Kuiv, Stepan Bandera, 33 <br> 02066 <br> Ukraine'
        },
        'New York': {
          coords: {
            lat: 40.6976633,
            lng: -74.1201069
          },
          title: 'Global sometext Ukraine LLC',
          address: 'NY, Stepan Bandera, 33 <br> 02066 <br> Ukraine'
        },
        Guangzhou: {
          coords: {
            lat: 23.1259806,
            lng: 112.9469733
          },
          title: 'Guangzhou Message Services Ukraine LLC',
          address: 'Guangzhou, Stepan Bandera, 33 <br> 02066 <br> Ukraine'
        },
        Barcelona: {
          coords: {
            lat: 41.3948975,
            lng: 2.0785566
          },
          title: 'Barcelona Message Services Ukraine LLC',
          address: 'Barcelona, Stepan Bandera, 33 <br> 02066 <br> Ukraine'
        }
      },
      activeTab: 'Kyiv'
    }
  },
  computed: {
    coords() {
      return this.tabs[this.activeTab].coords
    }
  },
  methods: {
    tabClick(key) {
      this.activeTab = key
    }
  }
}
</script>
<style lang="scss" scoped>
.map-min-height {
  min-height: 300px;
}
.theme-dark {
  background: #262626;
  color: #fff;
}
.big-padding {
  padding-top: 90px;
  padding-bottom: 140px;
  @media (max-width: 600px) {
    padding-top: 30px;
    padding-bottom: 30px;
  }
}
.tab-btn {
  font-size: 11px;
  letter-spacing: 1.5px;
  // cursor: pointer;

  &:hover,
  &:focus {
    text-decoration: underline;
    outline: none;
  }
}
.gmap-container {
  display: flex;
  width: 100%;
  height: 100%;
}
.gmapFullWidth {
  min-width: 50vw;
}
@media (min-width: 768px) {
  .gmapFullWidth {
    max-width: 50vw;
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
  }
}
</style>
