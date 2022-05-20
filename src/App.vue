<!-- <script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

</script> -->

<template>
  <header>

  </header>

  <main>
    <div id="app">

      <table class="table-auto bg-zinc-200 text-gray-500 rounded-xl m-auto h-40 mt-32">
        <tr class="">
          <th class="text-left p-8 pb-3 font-bold">
            Name
          </th>
          <th class="text-left p-8 pb-3 font-bold">
            Title
          </th>
          <th class="text-left p-8 pb-3 font-bold">
            User ID
          </th>
          <th class="text-left p-8 pb-3 font-bold">
            ID
          </th>
        </tr>
        <tr v-for="post in posts" :key="post.id" class="bg-zinc-100 border-2 border-black">
          <td class="text-left p-8">
            {{ post.username }}
          </td>
          <td class="text-left p-8 text-sky-500 underline">
            <a v-bind:href="'/content/' + post.postid">{{ post.title }}</a>
          </td>
          <td class="text-left p-8">
            {{ post.userId }}
          </td>
          <td class="text-left p-8">
            {{ post.postid }}
          </td>
        </tr>
        <tr>
          <td class="h-8"></td>
        </tr>
      </table>

      <form id="form" @submit.prevent="checkform" v-on:submit="handleSubmit">
        <table class="table-auto bg-zinc-200 text-gray-500 rounded-xl m-auto w-same mt-32 mb-32">
          <tr>
            <th class="text-left p-8 font-bold">Form</th>
          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2"><label for="title">Title</label></td>

          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2"><input name="title" id="title" v-model="modifiedData.data.title" type="text"
                class="border-2 border-black"></td>
          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2">
              <ul>
                <li v-for="error in errorstitle" :key="error" class="text-red-500">{{ error }}</li>
              </ul>
            </td>
          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2"><label for="content">Content</label></td>
          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2"><textarea name="content" id="content" cols="30" rows="10"
                v-model="modifiedData.data.content" class="border-2 border-black w-full"></textarea>
            </td>
          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2">
              <ul>
                <li v-for="error in errorscontent" :key="error" class="text-red-500">{{ error }}</li>
              </ul>
            </td>
          </tr>
          <tr class="bg-zinc-100 border-2 border-black">
            <td class="text-left p-2">
              <input type="submit" value="Submit" class="bg-blue-500 hover:bg-blue-400 text-white font-bold py-2 px-4 border-b-4 border-blue-700 hover:border-blue-500 rounded">
            </td>
          </tr>
        </table>
      </form>
    </div>


  </main>
</template>

<script>

blogusers: [];
posts: [];

export default {
  name: 'App',
  data() {
    return {
      blogusers: [],
      posts: [],
      errorstitle: [],
      errorscontent: [],
      modifiedData: {
        "data": {
          "title": "",
          "content": ""
        }
      },
      headers: {
        'Authorization': 'Bearer 36770a7931e4c3f1055d757c87084f00c40d9a593d8d5135b8f6daf6fc34f8b9fec4ff87a43f406034a9a4680b203c2c4434496b73825fef2a18d8a3e0fb0eeef21783b4b206da4e17b3c6f1e635a64dccefb4748fc4296d079a7666470556b43f5064f56200c3e5c7361a97c988d6f65ac5d80ba44dee25deddcbfbf13617d5'
      }
    }
  },
  mounted() {
    fetch('http://localhost:1337/api/blogusers', {
      method: 'GET',
    })
      .then(response => response.json())
      .then((data) => {
        var data1 = [];
        var finaldata = [];
        data1 = data.data;
        data1.forEach(element => {
          finaldata.push(element.attributes);
        });

        this.blogusers = finaldata;
        console.table(this.blogusers);
        console.log(this.blogusers);

      });
    fetch('http://localhost:1337/api/posts', {
      method: 'GET',
    })
      .then(response => response.json())
      .then((data) => {
        var data2 = [];
        var finaldata1 = [];
        data2 = data.data;
        data2.forEach(element => {
          finaldata1.push(element.attributes);
        });

        this.posts = finaldata1;
        console.table(this.posts);
        ///////////////
        this.posts.forEach(arr1 => {
          this.blogusers.forEach(arr2 => {
            if (arr1.userId == arr2.studentid) {
              arr1.username = arr2.username;
            }
          });
        });
        console.log("look here V");
        console.log("data.data");
        console.table(data.data);
        console.log("data");
        console.table(data);
        console.table(this.blogusers);
        console.log(this.posts);
        console.log("latest to look here");
        this.modifiedData = {
          "data": {
            "id": this.posts.length + 1,
            "postid": "xyz-" + (this.posts.length + 1).toString(),
            "title": "",
            "content": "",
            "userId": "20FTT1464"
          }
        }

      });
  },
  methods: {
    checkform() {
      if (this.modifiedData.data.title && this.modifiedData.data.content) {
        return true
      }
      this.errorstitle = [];
      this.errorscontent = [];
      if (!this.modifiedData.data.title) {
        this.errorstitle.push("Title is required!");
      }
      if (!this.modifiedData.data.content) {
        this.errorscontent.push("Content is required!");
      }
    },
    parseJSON: function (resp) {
      return (resp.json ? resp.json() : resp);
    },
    checkStatus: function (resp) {
      if (resp.status >= 200 && resp.status < 300) {
        return resp;
      }
      return this.parseJSON(resp).then((resp) => {
        throw resp;
      });
    },
    handleSubmit: async function (e) {
      e.preventDefault();
      if (this.modifiedData.data.title && this.modifiedData.data.content) {
        try {
          const response = await fetch('http://localhost:1337/api/posts', {
            method: 'POST',
            headers: {
              'Authorization': 'Bearer 562b65794406f21cb3f25109230f1b660535703d5dc5db21d8fbb87876fedb4c26922871b4416b409d65dfb1350361665a48f6796ac2c8392c70729d67d794250144d9d2a466de5c3078007ca5361baeb395bf8de712684bf6fc325f932cea49dcad9fdf44c2b4cb0140440bde94b20959272fd218b38940d6e9f92dc8208a15',
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(this.modifiedData)
          }).then(this.checkStatus)
            .then(this.parseJSON);
          console.log(response);
        } catch (error) {
          this.error = error;
          console.log(error);
        }
        document.location.reload();
      }
    }
  }
}
</script>

<style>
@import './assets/base.css';
/* 
#app {
  max-width: 1280px;
  margin: 0 auto;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }


  .logo {
    margin: 0 2rem 0 0;
  }
} */
</style>
