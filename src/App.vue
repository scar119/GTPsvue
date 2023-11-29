<template>
  <div id="app">
    <header>
      <img src="./assets/logo.png" alt="Logo" class="logo" />
      <h1>我的硅基老师</h1>
      <h2>“知无不言，言无不尽”</h2>
    </header>
    <section v-for="group in groupedLinks" :key="group.category" class="link-group">
    <h3>{{ group.category }}</h3>
    <div class="links">
      <div v-for="link in group.items" :key="link.url" class="link-item">
        <!-- 这里我们将a标签做成一个按钮 -->
        <a :href="link.url" target="_blank" class="link-button">
          <img :src="`https://flaskapp.fly.dev/${link.logo}`" alt="Logo" class="link-logo" />

          <!-- <img src="https://flaskapp.fly.dev/static/logo.png" alt="Test Logo" /> -->

          <div class="link-content">
            <h4>{{ link.name }}</h4>
            <p>{{ link.description }}</p>
          </div>
        </a>
      </div>
     </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      links: [], // 原来的数据格式可能需要根据分组结构进行调整
    };
  },
  computed: {
    // 根据分类将链接分组
    groupedLinks() {
      const groups = {};
      for (const link of this.links) {
        if (!groups[link.category]) {
          groups[link.category] = { category: link.category, items: [] };
        }
        groups[link.category].items.push(link);
      }
      return Object.values(groups);
    },
  },
  mounted() {
    this.fetchLinks();
  },
  methods: {
  fetchLinks() {
    fetch('https://flaskapp.fly.dev/links')
      .then(response => response.json())
      .then(data => {
        console.log('Fetched data:', data); // 添加这行来查看获取的数据
        this.links = data;
      })
      .catch(error => {
        console.error('Error fetching data:', error); // 添加错误处理
      });
  },

},

};
</script>

<style src="./App.css">
</style>
