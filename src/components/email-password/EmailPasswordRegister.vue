<template>
  <el-form :model="model" label-position="top">
    <el-form-item label="Email">
      <el-input v-model="model.email" type="email"></el-input>
    </el-form-item>
    <el-form-item label="Password">
      <el-input v-model="model.password" type="password" show-password>
      </el-input>
    </el-form-item>
    <el-form-item>
      <el-button @click="doRegister" type="primary">Register</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import { ref } from "vue";
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
export default {
  name: "EmailPasswordRegister",
  setup(props, context) {
    const model = ref({ email: null, password: null });
    function doRegister() {
      if (!model.value.email || model.value.email.length < 4) {
        alert("Please enter an email address.");
        return;
      }
      if (!model.value.password || model.value.password.length < 4) {
        alert("Please enter a password.");
        return;
      }
      // Create user with email and pass.
      const auth = getAuth();
      createUserWithEmailAndPassword(
        auth,
        model.value.email,
        model.value.password
      )
        .then((userCredential) => {
          // Signed in
          const user = userCredential.user;
          context.emit("submit", user);
        })
        .catch((error) => {
          const errorCode = error.code;
          const errorMessage = error.message;
          console.log(errorCode);
          console.log(errorMessage);
        });
    }
    return { model, doRegister };
  },
};
</script>

<style scoped></style>
