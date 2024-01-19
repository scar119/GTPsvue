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
            <img :src="`https://flask-app119.fly.dev/${link.logo}`" alt="Logo" class="link-logo" />

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
      fetch('https://flask-app119.fly.dev/links')

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
/* 使用更现代的颜色和字体 */
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
}

/* 优化标题和标语样式 */
header h1 {
  color: #9d602a; /* 深绿色调 */
}

header h2 {
  color: #51e7a1; /* 橙色调 */
}

/* 侧边栏样式调整 */
.category-sidebar {
  /* 保留原有样式 */
}

.category-sidebar button {
  background-color: #264653; /* 深蓝色调 */
  color: white;
  border: none;
  transition: background-color 0.3s;
}

.category-sidebar button:hover {
  background-color: #2a9d8f; /* 深绿色调 */
}

/* 链接组样式 */
.link-group {
  margin-left: 250px; /* 调整边距以适应侧边栏 */
}

.link-item {
  transition: transform 0.3s;
}

.link-item:hover {
  transform: translateY(-5px); /* 鼠标悬停时轻微上移 */
}

/* 响应式设计：媒体查询 */
@media (max-width: 768px) {
  .link-group {
    margin-left: 20px;
  }

  .category-sidebar {
    left: 0;
    width: 100%;
    top: initial;
    transform: initial;
  }
}

/* 其他样式调整 */
</style>