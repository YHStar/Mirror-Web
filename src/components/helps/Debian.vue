<template>
  <div>
    <h1>Debian镜像使用指南</h1>
    <div>
      <div class="select-version">
        <strong>选择你的Debian版本:</strong>
      </div>
      <div class="select-version">
        <el-select v-model="version" value-key="version" placeholder="请选择" v-on:change="handleSelect()">
          <el-option v-for="v in versions" :key="v.version" :value="v" :label="v.version">
          </el-option>
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
  name: 'Debian',
  data () {
    return {
      content_raw: '',
      versions: [
        {
          version: 'testing',
          codename: 'testing'
        },
        {
          version: 'bullseye',
          codename: 'bullseye'
        },
        {
          version: 'buster',
          codename: 'buster'
        },
        {
          version: 'stretch',
          codename: 'stretch'
        }
      ],
      version: null
    }
  },
  created () {
    this.version = this.versions[1]
  },
  mounted () {
    this.$axios.get('/static/help/Debian.md').then((response) => {
      this.content_raw = response.data
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
  computed: {
    content () {
      if (!this.version) {
        return ''
      }
      return this.content_raw.replace(
        /{debian_version}/g, this.version.version
      ).replace(
        /{debian_codename}/g, this.version.codename
      )
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
