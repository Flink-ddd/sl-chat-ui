<template>
    <div class="square-container">
      <h1>广场</h1>
      <div class="post-form">
        <textarea
          v-model="newPost"
          placeholder="分享新动态..."
          rows="3"
        ></textarea>
        <button @click="submitPost">发布</button>
      </div>
      <div class="posts">
        <div
          v-for="post in posts"
          :key="post.id"
          class="post-item"
        >
          <h3>{{ post.user }}</h3>
          <p>{{ post.content }}</p>
          <div class="post-footer">
            <span>{{ post.time }}</span>
            <button @click="likePost(post.id)">👍 {{ post.likes }}</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newPost: "", // 用户新发布的动态内容
        posts: [], // 存储广场的动态列表
      };
    },
    methods: {
      // 加载广场动态
      async fetchPosts() {
        try {
          // 调用后端接口获取动态数据
          const response = await fetch("/api/square/posts"); // 替换为真实接口
          const data = await response.json();
          this.posts = data.posts;
        } catch (error) {
          console.error("加载动态失败:", error);
        }
      },
      // 发布动态
      async submitPost() {
        if (!this.newPost.trim()) {
          alert("请输入内容再发布！");
          return;
        }
        try {
          // 调用后端接口发布动态
          const response = await fetch("/api/square/posts", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({ content: this.newPost }),
          });
          const data = await response.json();
          if (data.success) {
            alert("发布成功！");
            this.newPost = "";
            this.fetchPosts(); // 重新加载动态
          } else {
            alert("发布失败，请稍后重试！");
          }
        } catch (error) {
          console.error("发布动态失败:", error);
        }
      },
      // 点赞动态
      async likePost(postId) {
        try {
          // 调用后端接口给动态点赞
          const response = await fetch(`/api/square/posts/${postId}/like`, {
            method: "POST",
          });
          const data = await response.json();
          if (data.success) {
            this.fetchPosts(); // 更新动态列表
          } else {
            alert("点赞失败，请稍后重试！");
          }
        } catch (error) {
          console.error("点赞失败:", error);
        }
      },
    },
    mounted() {
      this.fetchPosts(); // 初始化加载动态
    },
  };
  </script>
  
  <style scoped>
  .square-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 16px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  .post-form {
    display: flex;
    flex-direction: column;
    margin-bottom: 16px;
  }
  textarea {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    resize: none;
  }
  button {
    margin-top: 8px;
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
  }
  button:hover {
    background-color: #0056b3;
  }
  .posts {
    margin-top: 16px;
  }
  .post-item {
    background-color: white;
    padding: 12px;
    margin-bottom: 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  .post-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 8px;
    font-size: 0.9em;
    color: #666;
  }
  </style>
  