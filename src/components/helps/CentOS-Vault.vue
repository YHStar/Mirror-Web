<template>
  <div>
    <h1>CentOS-Valut镜像使用指南</h1>
    <div>
      <div class="select-version">
        <strong>选择你的CentOS版本:</strong>
      </div>
      <div class="select-version">
        <el-select v-model="selected" placeholder="请选择" v-on:change="handleSelect()">
          <el-option value="CentOS7Before">CentOS7及之前</el-option>
          <el-option value="CentOS8"></el-option>
        </el-select>
      </div>
    </div>
    <p>
      <customized-markdown :content="content" v-on:render-complete="$emit('render-complete')">
      </customized-markdown>
    </p>
  </div>
</template>

<script>
export default {
  name: 'CentOS-Valut',
  data () {
    return {
      selected: 'CentOS7Before',
      content: 'CentOS7及之前'
    }
  },
  mounted () {
    this.$axios.get('/static/help/CentOS-Vault/' + this.selected + '.md').then((response) => {
      this.content = response.data
    })
  },
  methods: {
    handleSelect: function () {
      // Avoid unnecessary scrolling when the rendering is complete.
      if (this.$route.hash) {
        this.$router.replace({ hash: '' })
      }
    }
  },
  watch: {
    selected: function () {
      this.$axios.get('/static/help/CentOS-Vault/' + this.selected + '.md').then((response) => {
        this.content = response.data
      })
    }
  }
}
</script>

<style scoped>
p {
  text-align: left;
}

.select-version {
  display: inline-block;
  vertical-align: middle;
}
</style>
