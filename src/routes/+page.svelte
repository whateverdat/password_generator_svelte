  <script lang="ts">

  // password settings
  let length: number = 32;
  let uppercase: boolean = false;
  let lowercase: boolean = true;
  let digits: boolean = true;
  let symbols: boolean = false;
  let password: string = "";

  // states
  let type: "password" | "text" = "password";
  let btnText: "Reveal" | "Hide" = "Reveal";
  let message: string | number = "";

  // functions
  function revealPswd() {
      type = type === "password" ? (type = "text") : (type = "password");
      btnText = type === "password" ? (btnText = "Reveal") : (btnText = "Hide");
  }

  function copyPswd() {
    if (password) {
      navigator.clipboard.writeText(password);
      message = "Password has been copied.";
    }
  }

  function generatePswd() {
    if (!uppercase && !lowercase && !digits && !symbols) {
      message = "Check atleast one box below.";
    } else {
      let selected: string = "";
      password = "";

      if (lowercase) {
        selected += "abcdefghijklmnopqrstuvwxyz";
      }
      if (uppercase) {
        selected += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      }
      if (digits) {
        selected += "0123456789";
      }
      if (symbols) {
        selected += "~!@#$%^&*_-+=:;.?";
      }

      for (let i = 0; i < length; i++) {
        password += selected[Math.floor(Math.random() * selected.length)];
      }

      message = "Password has been generated.";
      return password;
    }
  }
    
  </script>
  
  <!-- Start page div -->
  <div class="h-screen flex flex-col items-center justify-evenly">
    <!-- Page title -->
    <h1>Create a secure password</h1>
    <!-- Message -->
    <h4>{message}</h4>
    <!-- Start options div -->
    <div id="options-section" class="grid grid-cols-2 items-center p-5">
      <label for="length">Length</label>
      <input id="length" type="range" min="8" max="64" bind:value={length} on:input={() => {message = length}}>
      <label for="lowercase">Lowercase</label>
      <input id="lowercase" type="checkbox" bind:checked={lowercase}>
      <label for="uppercase">Uppercase</label>
      <input id="uppercase" type="checkbox" bind:checked={uppercase}>
      <label for="digits">Digits</label>
      <input id="digits" type="checkbox" bind:checked={digits}>
      <label for="symbols">Symbols</label>
      <input id="symbols" type="checkbox" bind:checked={symbols}>
    </div>
    <!-- End options div -->
    <!-- Start password div -->
    <div id="password-section" class="grid">
      <input placeholder="The password will appear here" class="placeholder:text-sm text-center" {type} value={password}  />
      <!-- Start button container -->
      <div class="flex justify-center space-x-2 mt-2">
        <button id="show-pswd" on:click={revealPswd}>{btnText}</button>
        <button id="copy-pswd" on:click={copyPswd}>Copy</button>
      </div>
      <!-- End button container -->
    </div>
    <!-- End password div -->
    <button id="generate" on:click={generatePswd}>Generate</button>
  </div>
  <!-- End page div -->
  
  <style lang="postcss">
    :global(html) {
      background-color: theme(colors.gray.100);
    }

    button {
      background-color: lightgray;
      padding: 0.3em;
      width: 10rem;
    }
    button:hover {
      opacity: 75%;
    }
  </style>