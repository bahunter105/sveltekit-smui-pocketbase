<script>
  import PocketBase from 'pocketbase';
  import CircularProgress from '@smui/circular-progress';

  const client = new PocketBase('http://127.0.0.1:8090');

  let state ={
    email:"",
    name:"",
    password:"",
    passwordConfirm:"",
  }

  let status = false
  async function sendLogin(){
    const user = await client.users.create({
      email: state.email,
      password: state.password,
      passwordConfirm: state.passwordConfirm,
    });
    console.log(user)
    if(user){
      console.log("user is true")
      // something wrong with sendemail & update profile
      // sendeMail()
      // updateProfile(user.id)
    }
  }

  async function sendeMail(){
    await client.users.requestVerification(state.email);
  }

  async function updateProfile(id){
    await client.records.update('profiles', id, {
        name: state.name,
    });
  }

  //Logging in
  async function btnLogin(){
    const authData = await client.users.authViaEmail(state.email, state.password);
    if(authData){
      console.log("success Login")
      status = true
    }
  }

  // Logging out.
  async function btnLogout(){
    await client.authStore.clear();
    status = false
  }

</script>
<div>
  <h1>Register</h1>
  name: <input type="text" bind:value={state.name} name="">
  <br/>
  email: <input type="text" bind:value={state.email} name="">
  <br/>
  password: <input type="password" bind:value={state.password} name="">
  <br/>
  confirm password: <input type="password" bind:value={state.passwordConfirm} name="">
  <br/>
  <button
  on:click={sendLogin}
  >Register now</button>
  <hr/>


  {#if status == false}
    <h1>Login</h1>
    email: <input type="text" bind:value={state.email} name="">
    <br/>
    password: <input type="password" bind:value={state.password} name="">
    <br/>
    <button on:click={btnLogin}>login</button>
    <h1>You're Not Logged In</h1>
  {/if}

  {#if status == true}
    <button on:click={btnLogout}>logout</button>

    <h1>Login Successful</h1>
  {/if}
  <hr/>


</div>
