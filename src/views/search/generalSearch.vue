<template>
    <div class="container">      
          <el-row class="search_box">
            <el-col :span="24">             
                <el-autocomplete
                  class="inline-input "
                  v-model="searchs"
                  :fetch-suggestions="querySearch"
                  placeholder="请输入您想要搜索的内容"
                  :trigger-on-focus="false"
                  popper-class="search_input"
                >
                <i slot="prefix" class="iconfont icon-sousuo1"></i>
                <el-button slot="suffix" @click="search">搜&nbsp;&nbsp;索</el-button>
              </el-autocomplete>
            </el-col>
            <el-col :span="24" class="flex_ceter" style="height:24px;line-height:24px">
              <span class="font_size_14 fontC_333">搜索条件：</span>
              <el-radio-group v-model="radio">
                <el-radio label="all">不限</el-radio>
                <el-radio label="fileName">文件名</el-radio>
                <el-radio label="fileCon">文件内容</el-radio>
              </el-radio-group>
              <el-select v-model="lang" placeholder="所有语言" placement="top-end" @change="getLang" popper-class="lang_select">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-col>
          </el-row>        
        <div class="main_box">
            <div class="main_left_box">
              <div class="hot_search">
                <span class="item_title">
                  热词搜索
                </span>
                <span class="refresh_words" @click="hotWordRefresh()"><i class="iconfont icon-refresh"/>换一换</span>
                <ul class="item_content">
                  <li v-for="item in item_hot_contents" :key="item.index" @click="getHotValue(item)">{{item}}</li>
                  
                </ul>
              </div>
              <div class="key_search">
                 <span class="item_title">
                  关键词搜索
                </span>
                <span class="refresh_words" @click="keyWordRefresh()"><i class="iconfont icon-refresh"/>换一换</span>
                 <ul class="item_content">
                  <li v-for="(item,index) in item_key_contents" :key="index" @click="getKeyValue(item)">{{index+1}}、{{item}}</li>              
                </ul>
              </div>
            </div>
            <div class="theme_search">
               <span class="item_title">
                  主题搜索
                </span>
                <span class="refresh_words" @click="themeRefresh()"><i class="iconfont icon-refresh"/>换一换</span>
                <div class="theme_search_item" >
                  <template v-for="item in theme_options_values">
                  <el-cascader :placeholder="item"  :key="item.index" @change="getThemeValue" :options="theme_options" class="theme_search_input" :show-all-levels="false" popper-class="theme_search"></el-cascader>
                  </template>
                </div>
              
            </div>
        </div>
    </div>
</template>
<script>

export default {
  data() {
    return {
      restaurants: [{ value: '1' }, { value: '21' }, { value: '311贝斯1' }, { value: '43123' }, { value: '5' }, { value: '贝斯无锡1' }, { value: '贝斯无锡信息系统' }, { value: '73123' }, { value: '8' }],
      searchs: '',
      radio: 'fileName',
      lang: '',
      options: [{
        value: 'cn',
        label: '中文'
      }, {
        value: 'en',
        label: '英文'
      }, {
        value: 'ja',
        label: '日文'
      }],
      item_hot_contents: ["锦鲤", "杠精", "佛系", "确认过眼神", "官宣", "C位", "土味情话", "皮一下", "卡路里", "创造101", "超越妹妹", "五位一体", "四个全面", "共享经济", "大数据", "互联网+", "全十四五规划全面小康"],
      item_key_contents: ["联想 笔记本 四核 超薄", "晨光 笔记本 厚", "动物园 湖里 鱼", "中国 上海 浦东 陆家嘴软件园 ivison", "china Beijing", "导弹 东风 使命必达", "上班 工作 累"],
      theme_options_values: ["合同", "组件", "资源", "组件", "资源1", "组件2", "资源4"],
      theme_options: [
        {
          value: 'zhinan',
          label: '指南',
          children: [{
            value: 'shejiyuanze',
            label: '设计原则'
          }, {
            value: 'daohang',
            label: '导航',
          }]
        },
        {
          value: 'zujian',
          label: '组件',
          children: [{
            value: 'basic',
            label: 'Basic',
          }, {
            value: 'form',
            label: 'Form',
          }, {
            value: 'data',
            label: 'Data',
          }, {
            value: 'notice',
            label: 'Notice',
          }, {
            value: 'navigation',
            label: 'Navigation',
          }, {
            value: 'others',
            label: 'Others',
          }]
        },
        {
          value: 'ziyuan',
          label: '资源',
          children: [{
            value: 'axure',
            label: 'Axure Components'
          }, {
            value: 'sketch',
            label: 'Sketch Templates'
          }, {
            value: 'jiaohu',
            label: '组件交互文档'
          }]
        }]

    }
  },

  methods: {
    querySearch(queryString, cb) {
      var restaurants = this.restaurants;
      var results = queryString ? restaurants.filter(this.createFilter(queryString)) : restaurants;
      // 调用 callback 返回建议列表的数据
      cb(results);
    },
    createFilter(queryString) {
      return (restaurant) => {
        return (restaurant.value.toLowerCase().includes(queryString.toLowerCase()) === true);
      };
    },
    getLang(value) {
      this.lang = value
    },
    hotWordRefresh() {
      this.item_hot_contents.sort(() => Math.random() - 0.5);
    },
    keyWordRefresh() {
      this.item_key_contents.sort(() => Math.random() - 0.5);
    },
    themeRefresh() {
      this.theme_options_values.sort(() => Math.random() - 0.5);
    },
    getHotValue(value) {
      this.$router.push({
        path: '/search_result',
        query: {
          search: value
        }
      })
    },
    getKeyValue(value) {
      this.$router.push({
        path: '/search_result',
        query: {
          search: value
        }
      })
    },
    getThemeValue(value) {
      this.$router.push({
        path: '/search_result',
        query: {
          search: value[1]
        }
      })
    },
    search() {
      this.$router.push({
        path: '/search_result',
        query: {
          search: this.searchs,
          radio: this.radio,
          lang: this.lang
        }
      })
    }
  }


}
</script>
<style scoped>
.search_box {
  height: 164px;
  margin-bottom: 20px;
  background: #FFFFFF;
  box-shadow: 0px 2px 10px 0px rgba(68, 100, 163, 0.1);
  border-radius: 16px;
}
.main_box {
  height: calc(100vh - 248px);
  box-shadow: 0px 2px 10px 0px rgba(68, 100, 163, 0.1);
  background: #FFFFFF;
  border-radius: 16px;
  overflow: hidden;
  flex: 1;
  display: flex;
  justify-content: space-between;
  padding: 30px;
  box-sizing: border-box;
}
.inline-input {
  width: 100%;
  padding: 40px 30px 20px;
}
.icon-sousuo1 {
  line-height: 50px;
}
.search_box .el-button {
  width: 160px;
  height: 50px;
  background: linear-gradient(180deg, #475BA0 0%, #2FA5BB 100%);
  border-radius: 25px;
  margin-right: -5px;
  color: #FFFFFF;
  font-size: 16px;
  font-family: "微软雅黑";
  border: none;
}
.main_left_box {
  width: calc(66% - 30px);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.hot_search {
  height: calc(50% - 15px);
  background: #FFFFFF;
  border-radius: 16px;
  padding: 30px;
  border: 1px solid #CCCCCC;
  position: relative;
  box-sizing: border-box;
  overflow: hidden;
}
.key_search {
  height: calc(50% - 15px);
  background: #FFFFFF;
  border-radius: 16px;
  padding: 30px;
  border: 1px solid #CCCCCC;
  position: relative;
  box-sizing: border-box;
  overflow: hidden;
}
.theme_search {
  width: 34%;
  background: #FFFFFF;
  border-radius: 16px;
  padding: 30px;
  border: 1px solid #CCCCCC;
  position: relative;
  box-sizing: border-box;
  overflow: hidden;
}
.item_title {
  font-size: 18px;
  font-family: "微软雅黑";
  color: #333333;
  line-height: 24px;
}
.refresh_words {
  position: absolute;
  right: 30px;
  top: 20px;
  font-size: 16px;
  font-family: "微软雅黑";
  font-weight: 400;
  color: #999999;
  line-height: 28px;
  cursor: pointer;
}
.refresh_words:hover {
  color: #2D7A9C;
}
.icon-refresh {
  margin-right: 10px;
}
.item_content {
  margin-top: 20px;
}
.item_content li {
  float: left;
  margin-right: 30px;
  margin-bottom: 20px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 400;
  color: #333333;
}
.item_content li:hover {
  color: #2263EC;
}
.theme_search_input {
  width: 100%;
}
.theme_search_item {
  height: calc(100% - 40px);
  flex: 1;
  margin-top: 20px;
  padding-bottom: 20px;
  overflow-x: hidden;
  overflow-y: auto;
}
</style>