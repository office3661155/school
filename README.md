<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>姓名验证系统</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: '#165DFF',
            secondary: '#36D399',
            neutral: '#F3F4F6',
            danger: '#F87272',
          },
          fontFamily: {
            inter: ['Inter', 'sans-serif'],
          },
        },
      }
    }
  </script>
  <style type="text/tailwindcss">
    @layer utilities {
      .content-auto {
        content-visibility: auto;
      }
      .shadow-soft {
        box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05);
      }
      .transition-custom {
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
      }
    }
  </style>
</head>
<body class="font-inter bg-gray-50 min-h-screen flex flex-col">
  <!-- 顶部导航栏 -->
  <header class="bg-white shadow-md sticky top-0 z-50 transition-all duration-300">
    <div class="container mx-auto px-4 py-4 flex justify-between items-center">
      <div class="flex items-center space-x-2">
        <i class="fa-solid fa-id-card text-primary text-2xl"></i>
        <h1 class="text-xl font-bold text-gray-800">姓名验证系统</h1>
      </div>
      <div class="flex items-center space-x-4">
        <span class="hidden md:inline-block text-sm text-gray-600">
          <i class="fa-regular fa-calendar-check mr-1"></i>
          <span id="current-date">2025年5月21日</span>
        </span>
        <button id="theme-toggle" class="p-2 rounded-full hover:bg-gray-100 transition-colors">
          <i class="fa-regular fa-moon text-gray-600"></i>
        </button>
      </div>
    </div>
  </header>

  <!-- 主内容区 -->
  <main class="flex-grow container mx-auto px-4 py-8">
    <!-- 介绍卡片 -->
    <section class="max-w-3xl mx-auto mb-10 bg-white rounded-2xl shadow-soft p-6 transform hover:translate-y-[-5px] transition-custom">
      <h2 class="text-[clamp(1.5rem,3vw,2rem)] font-bold text-gray-800 mb-4">
        <i class="fa-solid fa-info-circle text-primary mr-2"></i>系统说明
      </h2>
      <p class="text-gray-600 mb-4 leading-relaxed">
        欢迎使用姓名验证系统。请在下方输入您的姓名进行验证。系统将比对您的姓名是否在授权列表中。
        如果验证成功，将显示相关表单供您填写。填写完成并提交后，表单将自动隐藏。
      </p>
      <div class="bg-primary/10 border-l-4 border-primary p-4 rounded-r-lg mb-4">
        <h3 class="font-semibold text-primary mb-1">注意事项</h3>
        <ul class="text-sm text-gray-700 space-y-1 list-disc list-inside">
          <li>请输入与授权列表中完全一致的姓名（包括大小写和特殊字符）</li>
          <li>验证成功后请尽快填写并提交表单</li>
          <li>如有疑问，请联系系统管理员</li>
        </ul>
      </div>
    </section>

    <!-- 验证区域 -->
    <section class="max-w-3xl mx-auto mb-10 bg-white rounded-2xl shadow-soft p-6 transform hover:shadow-lg transition-custom">
      <h2 class="text-xl font-bold text-gray-800 mb-6 flex items-center">
        <i class="fa-solid fa-user-check text-primary mr-2"></i>姓名验证
      </h2>
      
      <form id="verification-form" class="space-y-4">
        <div class="relative">
          <label for="name-input" class="block text-sm font-medium text-gray-700 mb-1">请输入您的姓名</label>
          <div class="relative">
            <span class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none text-gray-500">
              <i class="fa-solid fa-user"></i>
            </span>
            <input 
              type="text" 
              id="name-input" 
              class="w-full pl-10 pr-10 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary/50 focus:border-primary transition-all outline-none"
              placeholder="请输入您的真实姓名"
              required
            >
            <button 
              type="button" 
              id="clear-button" 
              class="absolute inset-y-0 right-0 flex items-center pr-3 text-gray-500 hover:text-gray-700 transition-colors opacity-0 pointer-events-none"
            >
              <i class="fa-solid fa-times-circle"></i>
            </button>
          </div>
        </div>
        
        <button 
          type="submit" 
          id="verify-button" 
          class="w-full bg-primary hover:bg-primary/90 text-white font-medium py-3 px-4 rounded-lg transition-all flex items-center justify-center space-x-2 transform hover:scale-[1.02] active:scale-[0.98]"
        >
          <i class="fa-solid fa-search"></i>
          <span>验证姓名</span>
        </button>
      </form>
      
      <!-- 验证结果区域 -->
      <div id="result-container" class="mt-6 hidden">
        <div id="loading-indicator" class="hidden text-center py-4">
          <div class="inline-block animate-spin rounded-full h-8 w-8 border-b-2 border-primary"></div>
          <p class="mt-2 text-gray-600">正在验证，请稍候...</p>
        </div>
        
        <div id="success-message" class="hidden bg-secondary/10 border-l-4 border-secondary p-4 rounded-r-lg">
          <div class="flex items-start">
            <div class="flex-shrink-0">
              <i class="fa-solid fa-check-circle text-secondary text-xl"></i>
            </div>
            <div class="ml-3">
              <h3 class="text-sm font-medium text-secondary">验证成功</h3>
              <div class="mt-2 text-sm text-gray-700">
                <p>您好，<span id="verified-name" class="font-semibold"></span>，您已通过验证。</p>
                <p class="mt-1">请填写下方表单完成相关信息提交。</p>
              </div>
            </div>
          </div>
        </div>
        
        <div id="error-message" class="hidden bg-danger/10 border-l-4 border-danger p-4 rounded-r-lg">
          <div class="flex items-start">
            <div class="flex-shrink-0">
              <i class="fa-solid fa-exclamation-circle text-danger text-xl"></i>
            </div>
            <div class="ml-3">
              <h3 class="text-sm font-medium text-danger">验证失败</h3>
              <div class="mt-2 text-sm text-gray-700">
                <p>未在授权列表中找到您的姓名 "<span id="unverified-name" class="font-semibold"></span>"。</p>
                <p class="mt-1">请检查输入是否正确，或联系管理员获取授权。</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Google表单嵌入区域 -->
    <section id="form-section" class="max-w-3xl mx-auto mb-10 bg-white rounded-2xl shadow-soft overflow-hidden transform hover:shadow-lg transition-custom scale-95 opacity-0 pointer-events-none h-0">
      <div class="p-6 border-b border-gray-200 bg-gray-50">
        <h2 class="text-xl font-bold text-gray-800 flex items-center">
          <i class="fa-solid fa-file-signature text-primary mr-2"></i>授权表单
        </h2>
        <p class="text-gray-600 mt-1">请填写以下表单完成相关信息提交</p>
      </div>
      <div class="relative aspect-video">
        <!-- 这里嵌入Google表单，使用示例URL -->
        <iframe 
          id="google-form" 
          src="https://docs.google.com/forms/d/e/1FAIpQLSdR8sD0JQ7w9oQdZ0nQ5p7n6Q5x0q0c7a5o7a8/edit" 
          width="100%" 
          height="100%" 
          frameborder="0" 
          marginheight="0" 
          marginwidth="0"
          class="absolute inset-0"
        >
          加载中...
        </iframe>
      </div>
      <div class="p-4 bg-gray-50 border-t border-gray-200 flex justify-end">
        <button 
          id="close-form" 
          class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-medium py-2 px-4 rounded-lg transition-all flex items-center space-x-2"
        >
          <i class="fa-solid fa-times"></i>
          <span>关闭表单</span>
        </button>
      </div>
    </section>
  </main>

  <!-- 页脚 -->
  <footer class="bg-gray-800 text-white py-8">
    <div class="container mx-auto px-4">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div>
          <h3 class="text-lg font-semibold mb-4 flex items-center">
            <i class="fa-solid fa-id-card text-primary mr-2"></i>
            姓名验证系统
          </h3>
          <p class="text-gray-400 text-sm leading-relaxed">
            本系统用于验证用户身份并提供相关服务授权。所有数据将严格保密，请放心使用。
          </p>
        </div>
        <div>
          <h3 class="text-lg font-semibold mb-4">快速链接</h3>
          <ul class="space-y-2 text-gray-400">
            <li><a href="#" class="hover:text-primary transition-colors"><i class="fa-solid fa-question-circle mr-2"></i>帮助中心</a></li>
            <li><a href="#" class="hover:text-primary transition-colors"><i class="fa-solid fa-file-text mr-2"></i>使用条款</a></li>
            <li><a href="#" class="hover:text-primary transition-colors"><i class="fa-solid fa-shield-alt mr-2"></i>隐私政策</a></li>
          </ul>
        </div>
        <div>
          <h3 class="text-lg font-semibold mb-4">联系我们</h3>
          <ul class="space-y-2 text-gray-400">
            <li class="flex items-center"><i class="fa-solid fa-envelope mr-2"></i> support@example.com</li>
            <li class="flex items-center"><i class="fa-solid fa-phone mr-2"></i> +886 123 4567</li>
            <li class="flex items-center"><i class="fa-solid fa-map-marker-alt mr-2"></i> 台北市信義區信義路5段7號</li>
          </ul>
        </div>
      </div>
      <div class="mt-8 pt-6 border-t border-gray-700 text-center text-gray-500 text-sm">
        <p>© 2025 姓名验证系统 版权所有</p>
      </div>
    </div>
  </footer>

  <!-- JavaScript -->
  <script>
    // 设置当前日期
    const setCurrentDate = () => {
      const now = new Date();
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      document.getElementById('current-date').textContent = now.toLocaleDateString('zh-TW', options);
    };
    
    // 显示/隐藏清除按钮
    const nameInput = document.getElementById('name-input');
    const clearButton = document.getElementById('clear-button');
    
    nameInput.addEventListener('input', () => {
      if (nameInput.value.trim() !== '') {
        clearButton.classList.remove('opacity-0', 'pointer-events-none');
      } else {
        clearButton.classList.add('opacity-0', 'pointer-events-none');
      }
    });
    
    clearButton.addEventListener('click', () => {
      nameInput.value = '';
      clearButton.classList.add('opacity-0', 'pointer-events-none');
      nameInput.focus();
    });
    
    // 表单验证处理
    const verificationForm = document.getElementById('verification-form');
    const resultContainer = document.getElementById('result-container');
    const loadingIndicator = document.getElementById('loading-indicator');
    const successMessage = document.getElementById('success-message');
    const errorMessage = document.getElementById('error-message');
    const verifiedName = document.getElementById('verified-name');
    const unverifiedName = document.getElementById('unverified-name');
    const formSection = document.getElementById('form-section');
    const googleForm = document.getElementById('google-form');
    const closeFormButton = document.getElementById('close-form');
    
    verificationForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const name = nameInput.value.trim();
      
      // 显示结果容器和加载指示器
      resultContainer.classList.remove('hidden');
      loadingIndicator.classList.remove('hidden');
      successMessage.classList.add('hidden');
      errorMessage.classList.add('hidden');
      
      // 模拟API调用延迟
      await new Promise(resolve => setTimeout(resolve, 1500));
      
      // 隐藏加载指示器
      loadingIndicator.classList.add('hidden');
      
      try {
        // 实际应用中，这里应该调用Google Sheets API进行验证
        // 这里使用模拟数据进行演示
        const isValid = await verifyNameWithGoogleSheets(name);
        
        if (isValid) {
          // 验证成功
          verifiedName.textContent = name;
          successMessage.classList.remove('hidden');
          
          // 显示表单区域
          setTimeout(() => {
            formSection.classList.remove('scale-95', 'opacity-0', 'pointer-events-none', 'h-0');
            formSection.classList.add('scale-100', 'opacity-100', 'pointer-events-auto');
          }, 500);
        } else {
          // 验证失败
          unverifiedName.textContent = name;
          errorMessage.classList.remove('hidden');
          
          // 隐藏表单区域
          formSection.classList.add('scale-95', 'opacity-0', 'pointer-events-none', 'h-0');
          formSection.classList.remove('scale-100', 'opacity-100', 'pointer-events-auto');
        }
      } catch (error) {
        console.error('验证过程中出错:', error);
        // 显示错误信息
        unverifiedName.textContent = name;
        errorMessage.classList.remove('hidden');
        
        // 隐藏表单区域
        formSection.classList.add('scale-95', 'opacity-0', 'pointer-events-none', 'h-0');
        formSection.classList.remove('scale-100', 'opacity-100', 'pointer-events-auto');
      }
    });
    
    // 关闭表单按钮
    closeFormButton.addEventListener('click', () => {
      // 隐藏表单区域
      formSection.classList.add('scale-95', 'opacity-0', 'pointer-events-none', 'h-0');
      formSection.classList.remove('scale-100', 'opacity-100', 'pointer-events-auto');
      
      // 重置输入
      nameInput.value = '';
      clearButton.classList.add('opacity-0', 'pointer-events-none');
      
      // 滚动到验证区域
      document.getElementById('verification-form').scrollIntoView({ behavior: 'smooth' });
    });
    
    // 模拟与Google Sheets API交互的函数
    async function verifyNameWithGoogleSheets(name) {
      // 在实际应用中，这里应该使用Google Sheets API
      // 例如：https://developers.google.com/sheets/api/guides/values
      
      // 以下是模拟实现，实际使用时需要替换为真实的API调用
      
      try {
        // 实际应用中，需要使用Google API密钥和Sheet ID
        const sheetId = '1zu6fJDLQ3VzJuHgoUtxsxq59tfXcQ20wyg5GruaGLPs';
        const apiKey = 'YOUR_API_KEY'; // 替换为您的API密钥
        
        // 构建API请求URL
        const url = `https://sheets.googleapis.com/v4/spreadsheets/${sheetId}/values/A2:A?key=${apiKey}`;
        
        // 发送请求
        const response = await fetch(url);
        
        if (!response.ok) {
          throw new Error(`API请求失败: ${response.status}`);
        }
        
        const data = await response.json();
        
        // 检查姓名是否存在于返回的数据中
        if (data.values) {
          // 从第二行开始比对（跳过标题行）
          return data.values.some(row => row[0].trim() === name);
        }
        
        return false;
      } catch (error) {
        console.error('与Google Sheets通信时出错:', error);
        // 为了演示，在错误发生时返回false
        return false;
      }
    }
    
    // 主题切换
    const themeToggle = document.getElementById('theme-toggle');
    const htmlElement = document.documentElement;
    
    // 检查用户偏好
    const isDarkMode = localStorage.getItem('darkMode') === 'true' || 
                      (localStorage.getItem('darkMode') === null && 
                       window.matchMedia('(prefers-color-scheme: dark)').matches);
    
    // 应用主题
    if (isDarkMode) {
      htmlElement.classList.add('dark');
      themeToggle.innerHTML = '<i class="fa-regular fa-sun text-yellow-400"></i>';
    }
    
    themeToggle.addEventListener('click', () => {
      const isDark = htmlElement.classList.toggle('dark');
      localStorage.setItem('darkMode', isDark);
      
      if (isDark) {
        themeToggle.innerHTML = '<i class="fa-regular fa-sun text-yellow-400"></i>';
      } else {
        themeToggle.innerHTML = '<i class="fa-regular fa-moon text-gray-600"></i>';
      }
    });
    
    // 初始化
    setCurrentDate();
  </script>
</body>
</html>    
