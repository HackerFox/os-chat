<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
      <br>
      <li><a href="http://vuejs-templates.github.io/webpack/" target="_blank">Docs for This Template</a></li>
    </ul>
    <h2>Ecosystem</h2>
    <ul>
      <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
      <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
      <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
    </ul>

    <div style="background: yellow;">
      os-chat
      <button @click="signin">
        Sign In
      </button>
      <button @click="getChats">
        Get Chats
      </button>
      <button @click="writeSample">
        Write Sample
      </button>
      <ol>
        <li v-for="chat in chats">
          {{ chat }}
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'

export const snapshotToArray = snapshot => {
  let returnArr = [];
  snapshot.forEach(childSnapshot => {
    let item = childSnapshot.val(); 
    item.key = childSnapshot.key;
    returnArr.push(item);
  });
  return returnArr;
};

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      chats: []
    }
  },
  methods: {
    now: function () {
      return Date.now()
    },
    signin: function () {
      const email = 'tngmichael89@gmail.com'
      const password = '123456'
      firebase.auth().signInWithEmailAndPassword(email, password).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        console.error(errorCode, errorMessage);
      });
      this.getChats()
    },
    getChats: function () {
      const chatsRef = firebase.database().ref('chats'); // << perlu di refactor
      chatsRef.on('value', snapshot => {
        const data = snapshotToArray(snapshot);
        this.chats = data;
      });
    },
    writeSample: function () {
      const chatsRef = firebase.database().ref('chats'); // << perlu di refactor
      const newChatsRef = chatsRef.push({
        person: 'Anonymous',
        lastMessage: 'pesan terakhir Good Luck!!',
        status: 'UNRESOLVED',
        timestamp: Date.now()
      });
      const chatId = newChatsRef.key;
      const messagesRef = firebase.database().ref('messages'); // << perlu di refactor
      messagesRef.child(chatId).push({
        from: 'Anonymous',
        message: 'pesan terakhir Good Luck!!',
        timestamp: Date.now()
      });
      messagesRef.child(chatId).on('child_added', snapshot => {
        console.log(snapshot);
      });
    }
  },
  mounted: function () {
    // this.getChats() << error karena belum di init, initnya di mounted app
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
