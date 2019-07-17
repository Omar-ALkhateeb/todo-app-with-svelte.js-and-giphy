<script>
  let todo = "";
  let url =
    "https://api.giphy.com/v1/gifs/search?api_key=L8OaLBCzG3O6FdrexueXereAa4olD2Ws&q=";
  let todos = [];
  async function load(word) {
    const fullUrl = `${url}${word}`;
    console.log(fullUrl);
    const response = await fetch(fullUrl);
    const json = await response.json();
    let gifs = json.data.map(gif => gif.images.fixed_height.url);
    console.log(gifs);
    return gifs[0];
    //loading = false;
  }
  async function handleClick() {
    if (todo != " " && todo) {
      //console.log(todo);
      let words = todo.split(" ").join("+");
      //   words.reverse();
      //   let word = words[0];
      let gif = await load(words);
      if (gif) {
        todos.push({
          done: false,
          desc: todo,
          gif: gif
        });
        todos = todos;
      }
      console.log(todos);

      todo = "";
    } else console.log("empty");
  }
  const del = i => () => {
    todos.splice(i, 1);
    todos = todos;
  };
  $: todoNums = todos.length;
</script>

<style>
  :global(body) {
    background-color: black;
    color: #fff;
  }
  .container {
    position: relative;
    max-width: 800px;
    /* margin: 0 auto; */
  }

  .image {
    display: block;
    width: 100%;
    max-width: 800px;
    height: auto;
    margin: 0 auto;
  }

  .overlay {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100%;
    width: 100%;
    transition: 0.5s ease;
    background-color: rgba(186, 0, 56, 0.5);
  }
  .container:hover .overlay {
    background-color: rgb(186, 0, 40);
  }

  .del {
    display: none;
    color: white;
    font-size: 20px;
    position: absolute;
    top: 50%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    text-align: center;
    color: #fff;
  }

  .container:hover .text {
    /* text-decoration: line-through; */
    display: none;
  }
  .container:hover .del {
    text-decoration: line-through;
    display: block;
  }

  .text {
    color: white;
    font-size: 20px;
    position: absolute;
    top: 50%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    text-align: center;
    color: #fff;
  }
  button {
    background-color: rgb(44, 43, 43);
    color: #fff;
  }
  input {
    background-color: rgb(44, 43, 43);
    color: #fff;
  }
</style>

<h1>todos</h1>
<input bind:value={todo} placeholder="enter your todo" />
<button on:click={handleClick}>add todos</button>
{#if todoNums == 0}
  <!-- <p>yay nothing todo</p> -->
{:else}
  {#each todos as { done, desc, gif }, i}
    <div on:click={del(i)} class="container">
      <img class="image" alt="gif" src={gif} />
      <div class="overlay">
        <div class="text">{desc}</div>
        <div class="del">Cross it</div>
      </div>
    </div>
  {/each}
{/if}
