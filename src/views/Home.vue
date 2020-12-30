<template>
  <div class="home">
    <div class="block items-center">
      <span class="demonstration">一级分类</span>
      <div class="items-btns">
        <el-button
          v-for="(item, index) in meta"
          :key="index"
          type="primary"
          size="small"
          @click="levelOneChecked(item)"
          >{{ item.label }}</el-button
        >
      </div>
      <div></div>
    </div>
    <div class="block">
      <span class="demonstration">二级分类</span>
      <el-dropdown :hide-on-click="false">
        <span class="el-dropdown-link"
          >{{ level2CheckedLabel }} <i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="drop-item-wrap" v-for="(item, index) in level2" :key="index">
            <el-dropdown-item @click.native="levelTwoChecked(item)">
              <span>{{ item.label }}</span>
            </el-dropdown-item>
            <el-button
              @click="delLevelTwoItem(item)"
              type="danger"
              size="small"
              icon="el-icon-delete"
              circle
            ></el-button>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
      <el-button type="primary" size="small" @click="levelTwoAdd">新增</el-button>
    </div>
    <div class="block">
      <span class="demonstration">三级分类</span>
      <el-dropdown :hide-on-click="false">
        <span class="el-dropdown-link"
          >{{ level3CheckedLabel }} <i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="drop-item-wrap" v-for="(item, index) in level3" :key="index">
            <el-dropdown-item @click.native="levelThreeChecked(item)">
              <span>{{ item.label }}</span>
            </el-dropdown-item>
            <el-button
              @click="delLevelThreeItem(item)"
              type="danger"
              size="small"
              icon="el-icon-delete"
              circle
            ></el-button>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
      <el-button type="primary" size="small" @click="levelThreeAdd">新增</el-button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
export default {
  data() {
    return {
      level1Checked: '', // 一级当前选中的value
      level2Checked: '', // 二级当前选中的value
      level2CheckedLabel: '', // 二级当前选中的label
      level3Checked: '', // 三级当前选中的value
      level3CheckedLabel: '', // 三级当前选中的label
      level2: [], // 二级当前展示
      level3: [], // 三级当前展示
      // 原始数据
      meta: [
        {
          value: 'zhinan',
          label: '指南',
          children: [
            {
              value: 'shejiyuanze',
              label: '设计原则',
              children: [
                {
                  value: 'yizhi',
                  label: '一致',
                },
                {
                  value: 'kekong',
                  label: '可控',
                },
              ],
            },
            {
              value: 'daohang',
              label: '导航',
              children: [
                {
                  value: 'cexiangdaohang',
                  label: '侧向导航',
                },
                {
                  value: 'dingbudaohang',
                  label: '顶部导航',
                },
              ],
            },
          ],
        },
        {
          value: 'nihao',
          label: '你好',
          children: [
            {
              value: 'heihei',
              label: '嘿嘿',
              children: [
                {
                  value: 'haha',
                  label: '哈哈',
                },
                {
                  value: 'hehe',
                  label: '呵呵',
                },
              ],
            },
            {
              value: 'baibai',
              label: '拜拜',
              children: [
                {
                  value: 'zhenbang',
                  label: '真棒',
                },
                {
                  value: 'henhao',
                  label: '很好',
                },
              ],
            },
          ],
        },
        {
          value: 'zujian',
          label: '组件',
          children: [
            {
              value: 'basic',
              label: 'Basic',
              children: [
                {
                  value: 'layout',
                  label: '布局',
                },
                {
                  value: 'color',
                  label: '色彩',
                },
              ],
            },
            {
              value: 'notice',
              label: 'Notice',
              children: [
                {
                  value: 'alert',
                  label: '警告',
                },
                {
                  value: 'loading',
                  label: '加载',
                },
                {
                  value: 'message',
                  label: '消息提示',
                },
                {
                  value: 'message-box',
                  label: '弹框',
                },
                {
                  value: 'notification',
                  label: '通知',
                },
              ],
            },
            {
              value: 'navigation',
              label: 'Navigation',
              children: [
                {
                  value: 'menu',
                  label: '导航菜单',
                },
                {
                  value: 'tabs',
                  label: '标签页',
                },
                {
                  value: 'breadcrumb',
                  label: '面包屑',
                },
                {
                  value: 'dropdown',
                  label: '下拉菜单',
                },
                {
                  value: 'steps',
                  label: '步骤条',
                },
              ],
            },
            {
              value: 'others',
              label: 'Others',
              children: [
                {
                  value: 'dialog',
                  label: '对话框',
                },
                {
                  value: 'tooltip',
                  label: '文字提示',
                },
                {
                  value: 'popover',
                  label: '弹出框',
                },
                {
                  value: 'card',
                  label: '卡片',
                },
                {
                  value: 'carousel',
                  label: '走马灯',
                },
                {
                  value: 'collapse',
                  label: '折叠面板',
                },
              ],
            },
          ],
        },
      ],
    }
  },
  watch: {
    level1Checked() {
      this.changeLevelTwo()
    },
    level2Checked() {
      this.changeLevelThree()
    },
    level3() {
      this.changeLevelThree()
    },
  },
  created() {
    // 初始化一级分类数据
    this.levelOneChecked(this.meta[0])
    this.changeLevelTwo()
    this.changeLevelThree()
  },
  computed: {},
  methods: {
    // 当前选择一级
    levelOneChecked(item) {
      this.level1Checked = item.value
    },
    // 改变二级分类数据
    changeLevelTwo() {
      this.level2 = this.meta?.find(item => item.value === this.level1Checked).children
      this.level2Checked = this.level2[0]?.value
      this.level2CheckedLabel = this.level2[0]?.label
    },
    // 当前选择二级
    levelTwoChecked(item) {
      this.level2Checked = item.value
      this.level2CheckedLabel = item.label
    },
    // 删除指定的二级的数据
    delLevelTwoItem(v) {
      for (let key in this.level2) {
        if (this.level2[key].value === v.value) {
          this.level2.splice(key, 1)
        }
      }
    },
    // 改变三级分类数据
    changeLevelThree() {
      this.level3 = this.level2?.find(item => item.value === this.level2Checked).children
      this.level3Checked = this.level3[0]?.value
      this.level3CheckedLabel = this.level3[0]?.label
    },
    // 当前选择三级
    levelThreeChecked(item) {
      this.level3Checked = item.value
      this.level3CheckedLabel = item.label
    },
    // 删除指定的三级的数据
    delLevelThreeItem(v) {
      for (let key in this.level3) {
        if (this.level3[key].value === v.value) {
          this.level3.splice(key, 1)
        }
      }
    },
    // 新增二级
    levelTwoAdd() {
      this.$prompt('请输入要新增的二级字段', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
      })
        .then(({ value }) => {
          this.level2.push({
            label: value,
            value: this.randomValue(),
            children: [],
          })
        })
        .catch(res => {
          console.log(res)
        })
    },
    // 新增三级
    levelThreeAdd() {
      this.$prompt('请输入要新增的三级字段', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
      })
        .then(({ value }) => {
          this.level3.push({
            label: value,
            value: this.randomValue(),
          })
        })
        .catch(res => {
          console.log(res)
        })
    },
    // 生成随机key的方法
    randomValue() {
      return parseInt(Math.random() * 100000) + 1000000
    },
  },
}
</script>

<style scoped>
.block {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 400px;
  height: 40px;
}
.drop-item-wrap {
  display: flex;
  justify-content: space-between;
}
.el-icon-delete {
  flex-shrink: 0;
}
.el-dropdown-menu__item {
  width: 100px;
}
.el-cascader {
  margin: 10px;
}
</style>
