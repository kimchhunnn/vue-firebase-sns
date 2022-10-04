<template>
  <div class="hello">
    <h3>Login With</h3>
    <ul>
      <li><button>Instagram</button></li>
      <li><button @click="line()">Line</button></li>
      <li><button @click="facebook()">Facebook</button></li>
      <li><button @click="twitter()">Twitter</button></li>
      <li><button @click="google()">Google</button></li>
    </ul>
    <h3>Provider: {{ provider }}</h3>
    <h3>UID: {{ uid }}</h3>
    <h3>Provider UID: {{ providerUid }}</h3>
  </div>
</template>

<script>
import { TwitterAuthProvider, GoogleAuthProvider, getAuth, signInWithPopup, FacebookAuthProvider  } from "firebase/auth";
import axios from 'axios';
import { OAuthProvider } from "firebase/auth";
import { ref } from "vue";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  setup() {
    const twitterProvider = new TwitterAuthProvider();
    const facebookProvider = new FacebookAuthProvider();
    const googleProvider = new GoogleAuthProvider();
    const lineProvider = new OAuthProvider('oidc.line');

    const registerUserUrl = 'http://127.0.0.1:23380/api/users/login';
    const provider = ref("Unauthorized");
    const uid = ref("XXXXXXXXX");
    const providerUid = ref("XXXXXXXXX");
    
    const facebook = () => {
      console.log("Facebook")
      const auth = getAuth();
        signInWithPopup(auth, facebookProvider)
          .then((result) => {
            console.log(result.user.accessToken) // token

            axios
              .post(registerUserUrl, {
                access_token:result.user.accessToken
              })
              .then(response => {
                console.log(response)
                provider.value = response.data.provider_id;
                uid.value = response.data.uid;
                providerUid.value = response.data.provider_uid;
              });

          })
          .catch((error) => {
            // Handle Errors here.
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode, errorMessage);
          });
    }

    const twitter = () => {
      console.log("Twitter")
      const auth = getAuth();
      signInWithPopup(auth, twitterProvider)
        .then((result) => {
          console.log(result.user.accessToken) // token

          axios
            .post(registerUserUrl, {
              access_token:result.user.accessToken
            })
            .then(response => {
              console.log(response)
              provider.value = response.data.provider_id;
              uid.value = response.data.uid;
              providerUid.value = response.data.provider_uid;
            });

        }).catch((error) => {
          const errorCode = error.code;
          const errorMessage = error.message;
          console.log(errorCode, errorMessage);
        });
    }

    const google = () => {
      console.log("Google")
      const auth = getAuth();
      signInWithPopup(auth, googleProvider)
        .then((result) => {
          console.log(result.user.accessToken); // token

          axios
            .post(registerUserUrl, {
              access_token:result.user.accessToken
            })
            .then(response => {
              console.log(response)
              provider.value = response.data.provider_id;
              uid.value = response.data.uid;
              providerUid.value = response.data.provider_uid;
            });

        }).catch((error) => {
          const errorCode = error.code;
          const errorMessage = error.message;
          console.log(errorCode, errorMessage);
        });
    }

    const line = () => {
      console.log("Line")
      const auth = getAuth();
      signInWithPopup(auth, lineProvider)
        .then((result) => {
          console.log(result.user.accessToken); // token

          axios
            .post(registerUserUrl, {
              access_token:result.user.accessToken
            })
            .then(response => {
              console.log(response)
              provider.value = response.data.provider_id;
              uid.value = response.data.uid;
              providerUid.value = response.data.provider_uid;
            });

        }).catch((error) => {
          const errorCode = error.code;
          const errorMessage = error.message;
          console.log(errorCode, errorMessage);
        });
    }

    return {
      facebook,
      twitter,
      google,
      line,
      provider,
      uid,
      providerUid
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
