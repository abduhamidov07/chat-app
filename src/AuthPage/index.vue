<template>
<main class="main">
<div class="container" id="container">
	<div class="form-container sign-up-container">
		 <form @submit.prevent="signup">
        <h1>Create account</h1>
        <input
          class="input"
          placeholder="Username"
          type="text"
          v-model="username"
        />
        <input placeholder="Password" type="password" v-model="password" />
        <input placeholder="Email" type="text" v-model="email" />
        <input placeholder="First name" type="text" v-model="first_name" />
        <input placeholder="Last name" type="text" v-model="last_name" />
        <button type="submit">Sign Up</button>
      </form>
	</div>
	<div class="form-container log-in-container">
    <form @submit.prevent="login">
			<h1>Login</h1>
			<span>or use your account</span>
			<div class="social-container">
				<a href="#" class="social"><i class="fab fa-facebook-f"></i></a>
				<a href="#" class="social"><i class="fab fa-google-plus-g"></i></a>
				<a href="#" class="social"><i class="fab fa-linkedin-in"></i></a>
			</div>
        <input placeholder="Username" type="text" v-model="username" />
        <input placeholder="Password" type="password" v-model="password" />
		  	<a class="forPaBtn" href="#">Forgot your password?</a>
        <button type="submit">Login</button>
      </form>
	</div>
	<div class="overlay-container">
		<div class="overlay">
			<div class="overlay-panel overlay-left">
				<h1>Welcome Back!</h1>
				<p>To keep connected with us please login with your personal info</p>
				<button @click="signIn" class="ghost" id="signIn">Login</button>
			</div>
			<div class="overlay-panel overlay-right">
				<h1>Hello, Friend!</h1>
				<p>Enter your personal details and start journey with us</p>
				<button @click="signUp" class="ghost" id="signUp">Sign Up</button>
			</div>
		</div>
	</div>
</div>
</main>
</template>

<script>
import { loginRest, signupRest } from "./api";

export default {
  data() {
    return {
      username: "",
      password: "",
      email: "",
      first_name: "",
      last_name: "",
    };
  },
  methods: {
    login() {
      loginRest(this.username, this.password)
        .then((response) =>
          this.$emit("onAuth", { ...response.data, secret: this.password })
        )
        .catch((error) => console.log("Login error", error));
    },
    signup() {
      signupRest(
        this.username,
        this.password,
        this.email,
        this.first_name,
        this.last_name
      )
        .then((response) =>
          this.$emit("onAuth", { ...response.data, secret: this.password })
        )
        .catch((error) => console.log("Sign up error", error));
    },
    signUp() {
      const container = document.querySelector('.container')
      container.classList.add("right-panel-active");
    },
    signIn() {
      const container = document.querySelector('.container')
      container.classList.remove("right-panel-active");
    }
  },
};
</script>

<style scoped>

@import url('https://fonts.googleapis.com/css?family=Montserrat:400,800');

* {
	box-sizing: border-box;
}

.main {
	background: #f6f5f7;
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	font-family: 'Montserrat', sans-serif;
	height: 100vh;
	/* margin: -20px 0 50px; */
}

h1 {
	font-weight: bold;
	margin: 0;
}

p {
	font-size: 14px;
	font-weight: 100;
	line-height: 20px;
	letter-spacing: 0.5px;
	margin: 20px 0 30px;
}

span {
	font-size: 12px;
}

a {
	color: #333;
	font-size: 14px;
	text-decoration: none;
}

button {
	border-radius: 20px;
  border: none;
  background-color: #2baaff;
	color: #FFFFFF;
	font-size: 12px;
	font-weight: bold;
	padding: 15px 48px;
	letter-spacing: 1px;
	text-transform: uppercase;
	transition: transform 80ms ease-in;
}

button:active {
	transform: scale(0.95);
}

button:focus {
	outline: none;
}

button.ghost {
	background-color: transparent;
	border: 1px solid #fff
}

form {
	background-color: #FFFFFF;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	padding: 0 50px;
	height: 100%;
	text-align: center;
}

input {
	background-color: #eee;
	border: none;
  outline: none;
  border-radius: 5px;
	padding: 12px 15px;
	margin: 8px 0;
	width: 100%;
}

.container {
	background-color: #fff;
	border-radius: 10px;
  	box-shadow: 0 14px 28px rgba(0,0,0,0.25), 
			0 10px 10px rgba(0,0,0,0.22);
	position: relative;
	overflow: hidden;
	width: 768px;
	max-width: 100%;
	min-height: 480px;
}

.form-container {
	position: absolute;
	top: 0;
	height: 100%;
	transition: all 0.6s ease-in-out;
}

.log-in-container {
	left: 0;
	width: 50%;
	z-index: 2;
}

.log-in-container h1 {
  margin-bottom: 20px;
}

.log-in-container a:last-child {
  margin: 8px 0;
}

.log-in-container .forPaBtn {
  margin: 12px 0 15px 0;
}

.container.right-panel-active .log-in-container {
	transform: translateX(100%);
}

.sign-up-container {
	left: 0;
	width: 50%;
	opacity: 0;
	z-index: 1;
}

.sign-up-container h1 {
  margin-bottom: 20px;
}

.sign-up-container button {
  margin-top: 15px;
}

.container.right-panel-active .sign-up-container {
	transform: translateX(100%);
	opacity: 1;
	z-index: 5;
	animation: show 0.6s;
}

@keyframes show {
	0%, 49.99% {
		opacity: 0;
		z-index: 1;
	}
	
	50%, 100% {
		opacity: 1;
		z-index: 5;
	}
}

.overlay-container {
	position: absolute;
	top: 0;
	left: 50%;
	width: 50%;
	height: 100%;
	overflow: hidden;
	transition: transform 0.6s ease-in-out;
	z-index: 100;
}

.container.right-panel-active .overlay-container{
	transform: translateX(-100%);
}

.overlay {
	background: #FF416C;
	background: linear-gradient(to right, #2baaff, #0190b4);
	background-repeat: no-repeat;
	background-size: cover;
	background-position: 0 0;
	color: #FFFFFF;
	position: relative;
	left: -100%;
	height: 100%;
	width: 200%;
  	transform: translateX(0);
	transition: transform 0.6s ease-in-out;
}

.container.right-panel-active .overlay {
  	transform: translateX(50%);
}

.overlay-panel {
	position: absolute;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	padding: 0 40px;
	text-align: center;
	top: 0;
	height: 100%;
	width: 50%;
	transform: translateX(0);
	transition: transform 0.6s ease-in-out;
}

.overlay-left {
	transform: translateX(-20%);
}

.container.right-panel-active .overlay-left {
	transform: translateX(0);
}

.overlay-right {
	right: 0;
	transform: translateX(0);
}

.container.right-panel-active .overlay-right {
	transform: translateX(20%);
}

.social-container {
	margin: 5px 0 8px 0;
}

.social-container a {
	border: 1px solid #DDDDDD;
	border-radius: 50%;
	display: inline-flex;
	justify-content: center;
	align-items: center;
	margin: 0 5px;
	height: 40px;
	width: 40px;
}

</style>
