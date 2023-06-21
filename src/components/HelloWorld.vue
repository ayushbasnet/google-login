<template>
  <div class="hello">
    <h1>Login With Google</h1>

    <div
      id="g_id_onload"
      data-client_id="917452060688-54frsm3m4i41ukd0atbdsoe8kovdei2g.apps.googleusercontent.com"
      data-context="signin"
      data-ux_mode="popup"
      data-callback="handleCredentialResponse"
      data-auto_prompt="false"
    ></div>

    <div
      class="g_id_signin"
      data-type="standard"
      data-shape="pill"
      data-theme="filled_blue"
      data-text="signin_with"
      data-size="large"
      data-logo_alignment="left"
    ></div>
    <h2 v-if="fullName">{{ fullName }}</h2>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      fullName: "Nothing",
    };
  },
  mounted() {
    // Load the Google Sign-In API library
    this.loadGoogleSignInLibrary();
  },
  methods: {
    loadGoogleSignInLibrary() {
      const script = document.createElement("script");
      script.src = "https://accounts.google.com/gsi/client";
      script.async = true;
      script.defer = true;
      document.head.appendChild(script);
    },
    setName(name) {
      this.fullName = name;
    },
  },
};
function decodeJwtResponse(token) {
  let base64Url = token.split(".")[1];
  let base64 = base64Url.replace(/-/g, "+").replace(/_/g, "/");
  let jsonPayload = decodeURIComponent(
    atob(base64)
      .split("")
      .map(function (c) {
        return "%" + ("00" + c.charCodeAt(0).toString(16)).slice(-2);
      })
      .join("")
  );
  return JSON.parse(jsonPayload);
}

window.handleCredentialResponse = (response) => {
  // decodeJwtResponse() is a custom function defined by you
  // to decode the credential response.
  const responsePayload = decodeJwtResponse(response.credential);

  console.log("ID: " + responsePayload.sub);
  console.log("Full Name: " + responsePayload.name);
  console.log("Given Name: " + responsePayload.given_name);
  console.log("Family Name: " + responsePayload.family_name);
  console.log("Image URL: " + responsePayload.picture);
  console.log("Email: " + responsePayload.email);
};
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
