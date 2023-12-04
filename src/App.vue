<template>
  <div id="app">
    <header>
      <img src="./assets/logo.png" alt="Logo" class="logo" />
      <h1>我的硅基老师</h1>
      <h2>“知无不言，言无不尽”</h2>
    </header>

    <aside class="category-sidebar">
      <ul>
        <li v-for="group in groupedLinks" :key="group.category">
          <button @click="scrollToCategory(group.category)">{{ group.category }}</button>
        </li>
      </ul>
    </aside>

    <section v-for="group in groupedLinks" :key="group.category" class="link-group" :id="`group-${group.category}`">
      <h3>{{ group.category }}</h3>
      <div class="links">
        <div v-for="link in group.items" :key="link.url" class="link-item">
          <a :href="link.url" target="_blank" class="link-button">
            <!-- <img :src="`http://127.0.0.1:5000/${link.logo}`" alt="Logo" class="link-logo" /> -->
            <img :src="`https://flaskapp12.fly.dev/${link.logo}`" alt="Logo" class="link-logo" />

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
      links: [],
    };
  },
  computed: {
    groupedLinks() {
      const groups = {};
      for (const link of this.links) {
        if (!groups[link.category]) {
          groups[link.category] = { 
            category: link.category, 
            items: [], 
            category_weight: link.category_weight 
          };
        }
        groups[link.category].items.push(link);
      }

      // 对分组进行排序
      const sortedGroups = Object.values(groups).sort((a, b) => b.category_weight - a.category_weight);

      // 对每个分组内的链接进行排序
      sortedGroups.forEach(group => {
        group.items.sort((a, b) => b.weight - a.weight);
      });

      return sortedGroups;
    },
  },
  mounted() {
    this.fetchLinks();
    document.title = '我的硅基老师们';
    console.log('App.vue mounted!');
  },
  methods: {
    fetchLinks() {
      // fetch('http://127.0.0.1:5000/links')
      fetch('https://flaskapp12.fly.dev/links')

        .then(response => response.json())
        .then(data => {
          console.log('Fetched data:', data);
          this.links = data;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },
    scrollToCategory(category) {
      const element = document.getElementById(`group-${category}`);
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' });
      }
    },
  },
};
</script>

<style src="./App.css">
.category-sidebar {
  position: fixed;
  left: 10px; /* 距离屏幕左侧的距离 */
  top: 50%; /* 垂直居中于屏幕 */
  transform: translateY(-50%); /* 向上平移50%的高度，确保完全居中 */
  width: 200px; /* 宽度，可根据需要调整 */
  background-color: rgba(128, 128, 128, 0.7); /* 灰色半透明背景 */
  padding: 10px;
  border-radius: 10px; /* 圆角 */
  overflow-y: auto;
  z-index: 1000; /* 确保在其他元素之上 */
  max-height: 80%; /* 最大高度，以避免覆盖太多内容 */
}

.category-sidebar ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.category-sidebar button {
  width: 100%;
  padding: 5px;
  margin-bottom: 5px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  cursor: pointer;
  border-radius: 5px; /* 按钮圆角 */
}

.category-sidebar button:hover {
  background-color: #e0e0e0;
}

/* 可能需要调整页面主体内容的样式，以避免被目录遮挡 */
.link-group {
  margin-left: 220px; /* 根据实际情况调整 */
}


/* 其他样式 */
</style>
