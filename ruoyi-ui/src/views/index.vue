<template>
  <div class="home-container">
    <!-- 欢迎语 -->
    <div class="welcome-section">
      <h1 class="welcome-title">欢迎使用慧视管理系统</h1>
      <p class="welcome-subtitle">智能化医疗管理解决方案</p>
    </div>

    <!-- 图片轮播 -->
    <el-carousel :interval="5000" height="400px">
      <el-carousel-item>
        <img src="../assets/images/login-background.jpg" class="carousel-image" />
        <div class="carousel-caption">
          <h3>现代化医疗设施</h3>
          <p>配备国际先进医疗设备，提供精准诊疗服务</p>
        </div>
      </el-carousel-item>
    </el-carousel>

    <!-- 文字介绍板块 -->
    <div class="intro-section">
      <el-row :gutter="20">
        <el-col :xs="24" :sm="8" :md="8" :lg="8" v-for="(item, index) in introItems" :key="index">
          <div class="intro-card">
            <div class="icon-wrapper">
              <component :is="item.icon" style="width: 2em; height: 2em" />
            </div>
            <h3 class="intro-title">{{ item.title }}</h3>
            <p class="intro-content">{{ item.content }}</p>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script setup>


const carouselItems = [
  {
    image: '../assets/images/login-background.jpg',
    title: '现代化医疗设施',
    description: '配备国际先进医疗设备，提供精准诊疗服务'
  },
//   {
//     image: require('@/assets/images/hospital2.jpg'),
//     title: '专业医疗团队',
//     description: '汇聚各领域医疗专家，保障医疗质量'
//   },
//   {
//     image: require('@/assets/images/hospital3.jpg'),
//     title: '贴心护理服务',
//     description: '全程人性化服务，打造舒适就医体验'
//   }
]

// const introItems = [
//   {
//     icon: User,
//     title: '患者管理',
//     content: '完善的电子病历系统，实现患者信息全流程跟踪管理'
//   },
//   {
//     icon: Monitor,
//     title: '智能监控',
//     content: '实时监测医疗设备状态，保障诊疗过程安全可靠'
//   },
//   {
//     icon: Setting,
//     title: '系统配置',
//     content: '灵活可配置的权限管理体系，满足多样化管理需求'
//   }
// ]
</script>

<style scoped>
.home-container {
  padding: 20px;
}

.welcome-section {
  text-align: center;
  margin: 40px 0;
}

.welcome-title {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 15px;
}

.welcome-subtitle {
  font-size: 1.2rem;
  color: #7f8c8d;
}

.carousel-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.carousel-caption {
  position: absolute;
  bottom: 20px;
  left: 20px;
  color: white;
  text-shadow: 1px 1px 3px rgba(0,0,0,0.5);
}

.intro-section {
  margin: 40px 0;
}

.intro-card {
  background: #fff;
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.1);
  transition: transform 0.3s;
}

.intro-card:hover {
  transform: translateY(-5px);
}

.icon-wrapper {
  background: #409eff;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 15px;
}

.intro-title {
  font-size: 1.2rem;
  color: #2c3e50;
  margin-bottom: 10px;
}

.intro-content {
  color: #7f8c8d;
  line-height: 1.6;
}
</style>