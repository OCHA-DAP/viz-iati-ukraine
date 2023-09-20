<template>
  <div class="iati-viz mb-4">
    <OchaHeader />
    <b-alert show class="alert-message">
      <p>We are no longer maintaining the IATI Ukraine Funding Dashboard as of <b>2023-08-14</b>. Contact us at <a href="mailto:hdx@un.org">hdx@un.org</a> with questions or feedback.</p>
    </b-alert>
    <b-container>
      <b-navbar-brand :to="'/'">
        <span v-html="pageTitle" />
      </b-navbar-brand>
      <b-navbar toggleable="lg" type="light" variant="bg-white" class="navbar-iati">
        <b-navbar-toggle target="nav-collapse" />

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item :to="{name: 'index'}" exact-active-class="active" class="nav-index" no-prefetch @click="onClick('Commitments and Spending')">
              Commitments and Spending
            </b-nav-item>
            <b-nav-item :to="{name: 'spending_flows'}" active-class="active" class="nav-flows" no-prefetch @click="onClick('Spending Flows')">
              Spending Flows
            </b-nav-item>
          </b-navbar-nav>
<!--           <b-navbar-nav class="ml-auto">
            <b-nav-item :to="{name: 'about'}" active-class="active" class="ml-lg-auto nav-about" no-prefetch @click="onClick('About this Dashboard')">
              About this Dashboard
            </b-nav-item>
          </b-navbar-nav> -->
        </b-collapse>
      </b-navbar>
      <nuxt class="main-content" />

      <b-container class="footer">
        <b-row>
          <b-col cols="12">
            <div class="logo-container">
              <a href="https://www.unocha.org" target="_blank"><img src="~@/assets/logos//ocha.png" alt="UN OCHA" height="50"></a>
              <a href="https://centre.humdata.org" target="_blank"><img src="~@/assets/logos/centrehumdata.png" alt="Centre for Humanitarian Data" height="45"></a>
              <a href="https://iatistandard.org" target="_blank"><img src="~@/assets/logos/iati.png" alt="Powered by IATI" height="50"></a>
            </div>
          </b-col>
        </b-row>
      </b-container>
    </b-container>
  </div>
</template>

<style lang='scss'>
.navbar-brand {
  color: #000;
  font-family: 'Gotham Bold', sans-serif;
  font-size: 24px;
  line-height: 28px;
  white-space: normal;
}
.navbar-iati {
  box-shadow: inset 0px -3px 0px 0px #D8D8D8;
  margin: 26px 0 32px;
  padding: 0 27px;
  .nav-item {
    color: #888;
    font-family: 'Gotham Bold', sans-serif;
    font-size: 14px;
    .nav-link {
      padding: 8px 30px;
      &.active {
        border-bottom: 3px solid #F2645A;
        color: #F2645A;
      }
    }
  }
}
.main-content {
  min-height: 100vh;
}
.footer {
  padding: 80px 15px 40px;
  text-align: right;
  .logo-container {
    align-items: center;
    display: flex;
    justify-content: space-between;
    img {
      width: auto;
    }
  }
}
.alert-message {
  background-color: #FDE9E8;
  border: 1px solid #F2645A;
  border-radius: 0;
  color: #000;
  font-size: 16px;
  padding: 0;
  text-align: center;
  a {
    color: #F2645A;
  }
  p {
    margin: 0;
    padding: 20px;
  }
}

@media only screen and (max-width: 992px) {
  .navbar-nav {
    text-align: center;
  }
  .navbar-collapse {
    margin-bottom: 20px;
  }
  .navbar-iati {
    box-shadow: none;
    margin: 0;
    padding: 10px;
  }
  .navbar-toggler {
    border: none;
    padding-right: 0;
    position: absolute;
    right: 0;
    top: -66px;
  }
  .footer {
    padding-top: 0;
    .logo-container {
      flex-wrap: wrap;
      > a {
        margin-top: 30px;
        width: 50%;
      }
    }
  }
}
</style>

<script>
import mixpanel from 'mixpanel-browser'
import config from '../nuxt.config'
import OchaHeader from '~/components/OchaHeader'

export default {
  components: {
    OchaHeader
  },
  data () {
    return {
      title: config.head.title
    }
  },
  computed: {
    pageTitle () {
      let isProd = true
      if (process.client) {
        isProd = !!(window.location.host.includes('ocha-dap')) || !!(window.location.host.includes('humdata'))
        this.$store.commit('setProd', isProd)
      }
      return (isProd) ? 'IATI Ukraine Funding Dashboard' : '*STAGE* IATI Ukraine Funding Dashboard'
    }
  },
  mounted () {
    const MIXPANEL_TOKEN = this.$store.state.isProd ? process.env.NUXT_ENV_MIXPANEL_TOKEN_PROD : process.env.NUXT_ENV_MIXPANEL_TOKEN_DEV
    mixpanel.init(MIXPANEL_TOKEN)
    this.$mixpanelTrackView()
  },
  methods: {
    onClick (page) {
      this.$mixpanelTrackAction('switch viz', config.head.title, page)
    }
  }
}
</script>
