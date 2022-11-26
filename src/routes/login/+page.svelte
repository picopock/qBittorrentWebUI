<script lang="ts">
  import Logo from '$lib/images/logo.png';
  import LoginPic from '$lib/images/login.png';
  import PersonSvg from '$lib/svgs/icons/person.svg';
  import PasswordSvg from '$lib/svgs/icons/password.svg';
  import VisibilitySvg from '$lib/svgs/icons/visibility.svg';
  import VisibilityOffSvg from '$lib/svgs/icons/visibility_off.svg';
  import axios from 'axios';

  let username: string = '';
  let password: string = '';
  let passwordVisibility: boolean = false;

  $: passwordType = passwordVisibility ? 'text' : 'password';
  $: visibilitySvg = passwordVisibility ? VisibilitySvg : VisibilityOffSvg;

  const handleUsernameChange = (evt: Event) => {
    const value = (evt?.target as HTMLInputElement)?.value || '';
    username = value;
  };

  const handlePasswordChange = (evt: Event) => {
    const value = (evt?.target as HTMLInputElement)?.value || '';
    password = value;
  };

  const handlePWVisibility = () => {
    passwordVisibility = !passwordVisibility;
  };

  const handleSubmit = () => {
    const queryString = `username=${encodeURIComponent(username)}&password=${encodeURIComponent(
      password
    )}`;
    axios
      .post('/api/v2/auth/login', queryString, {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8'
        }
      })
      .then(() => {
        location.href = '/';
      })
      .catch((err) => {
        console.log(err);
      });
  };
</script>

<svelte:head>
  <title>登录</title>
  <meta name="description" content="登录" />
</svelte:head>

<div class="login">
  <div class="login-wrap">
    <div class="logo-wrap">
      <img class="logo" src={Logo} alt="qBittorrent" />
      <div class="brand">qBittorrent</div>
    </div>
    <div class="content">
      <img class="img" src={LoginPic} alt="cultule" />
      <div class="login-info">
        <div class="welcome">
          <div class="title">Welcome Back :)</div>
          <div class="sub">
            To keep connect with us, please login with your personal information by email address
            and password.
          </div>
        </div>
        <div class="login-form">
          <div class="input-wrap">
            <div class="username">
              <img src={PersonSvg} class="icon" alt="people" />
              <div class="field">
                <label class="label" for="username">Username</label>
                <!-- svelte-ignore a11y-autofocus -->
                <input
                  type="text"
                  id="username"
                  class="value"
                  autofocus
                  value={username}
                  on:change={handleUsernameChange}
                />
              </div>
            </div>
            <div class="divide" />
            <div class="password">
              <img src={PasswordSvg} class="icon" alt="password" />
              <div class="field">
                <label class="label" for="passwd">Password</label>
                <input
                  type={passwordType}
                  id="passwd"
                  class="value"
                  autocomplete="current-password"
                  value={password}
                  on:change={handlePasswordChange}
                />
              </div>
              <!-- svelte-ignore a11y-click-events-have-key-events -->
              <img
                src={visibilitySvg}
                alt="visibility"
                class="visibility-icon"
                on:click={handlePWVisibility}
              />
            </div>
          </div>
          <div class="remember"><input type="checkbox" />Remember Me</div>
          <div class="submit">
            <button class="submit-btn" on:click={handleSubmit}>Login Now</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style lang="scss">
  @import './style.scss';
</style>
