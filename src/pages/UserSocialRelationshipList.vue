<template>
  <div class="user-social-relationship-list">
    <loading :show="loading" :text="textLoading"></loading>
    <div class="social-relationship-wrapper">
      <social-relationship-edit v-for="socialRelationship in socialRelationshipList" :social_relationship="socialRelationship"></social-relationship-edit>
    </div>
    <div class="btn-wrapper">
      <a v-link="'/home/edit/addUserSocialRelationship'" class="btn-add-work-exp">添加更多社会关系</a>
    </div>
    <toast :show.sync="showToast" type="text" width="12em">{{textToast}}</toast>
  </div>
</template>

<script lang="babel">
import * as api from 'src/api.js'
import { Loading, Toast, Sticky } from 'vux-components'
import SocialRelationshipEdit from 'components/SocialRelationshipEdit'

export default {
  name: 'UserSocialRelationshipList',
  components: {
    Loading,
    Toast,
    Sticky,
    SocialRelationshipEdit
  },
  data () {
    return {
      loading: true,
      textLoading: 'Loading...',
      textToast: '',
      showToast: false,
      memberLoginId: '',
      cpUserId: '',
      socialRelationshipList: []
    }
  },
  ready () {
    let u = JSON.parse(window.localStorage.getItem('userInfo'))
    this.$set('memberLoginId', u.memberLoginId)
    this.$set('cpUserId', u.cpUserId)
    this.fetchSocialRelationshipList()
  },
  methods: {
    fetchSocialRelationshipList () {
      const that = this
      that.$http({
        url: api.showRelaInfo,
        params: {
          memberLoginId: that.memberLoginId,
          xgCpUserBaseId: that.cpUserId
        },
        method: 'GET'
      }).then(res => {
        // console.log('socialRelationshipList === ' + JSON.stringify(res.data))
        that.$set('socialRelationshipList', res.data.personalList)
        that.$set('loading', false)
      }).catch(err => {
        console.error(err.data)
      })
    }
  }
}
</script>

<style lang="less">
.user-social-relationship-list {
  box-sizing: border-box;
  width: 100%;
  padding: 16px;

  .weui_btn {
    margin-top: 0;
  }
  .weui_btn:after {
    border: none;
    border-radius: 2px;
  }
  .weui_btn_default {
    color: #fff;
    border-radius: 2px;
    background-color: #38acfd;
  }
  .social-relationship-wrapper {
    box-sizing: border-box;
    width: 100%;
    background-color: #fff;
    padding: 0 16px;
  }
  .btn-wrapper {
    padding-left: 0;
    padding-right: 0;
  }
  .btn-wrapper-fixed {
    box-sizing: border-box;
    width: 100%;
    height: 67px;
    border-top: 1px solid #f7f7f7;
    background-color: #fff;
    padding: 12px 18px;
    position: fixed;
    left: 0;
    bottom: 0;
  }
}
</style>