<script>
  import config from './config'
  import store from '@/store'
  import { getToken,setToken } from '@/utils/auth'

  

  export default {
    onLaunch: function() {
      this.initApp()
    },
    methods: {
      // 初始化应用
      initApp() {
        // 初始化应用配置2345
        this.initConfig()
		//获取微信相关信息
        this.getWeiXin()
        // 检查用户登录状态22
        this.checkLogin()
        
      },
      initConfig() {
        this.globalData.config = config
      },
	  getWeiXin(){

		  //获取code
		  wx.login({ 
		  				  success:(res)=>{
		  					  console.log(res) 
							  this.globalData.config.code=res.code
							  // console.log("this.globalData.config.code   :"+this.globalData.config.code)
		  					  
							  //获取openid
							  uni.request({
								    data:{"code":res.code},  
									method:'POST', 
							        url: this.globalData.config.baseUrl+'/weixin/login',
							        success:(res)=>{
							          console.log(res)
							          this.globalData.config.openid=res.data.openid   
									  if(res.data.code==500){ 
										  console.log("openId 不存在！"+res)
										  this.$tab.reLaunch('/pages/register')
									  }
									  setToken(res.data.token)
									  this.$tab.reLaunch('/pages/index')
							        }
									
							      })
		  				  }
		  })
		  

	  },
	  
      checkLogin() {
		 
        if (!getToken() && config.appInfo.program == 'weixin') {
          this.$tab.reLaunch('/pages/register') 
        }else{
			//this.$tab.reLaunch('/pages/login')
		}
      }
    }
  }
</script>

<style lang="scss">
  @import '@/static/scss/index.scss';
  @import '@/node_modules/uview-ui/theme.scss'
</style>
