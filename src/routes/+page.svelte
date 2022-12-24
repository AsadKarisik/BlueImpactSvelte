<script>
  let pressB1 = "background-color: white;";
  let pressB2 = "background-color: transparent";
  let pressB3 = "background-color: transparent";
  let firstL = "first-disp";
  let firstD = "first-disp1";

  function pressButt1() {
    pressB1 = "background-color: white;";
    pressB2 = "background-color: transparent";
    pressB3 = "background-color: transparent";
    firstL = "first-disp";
    firstD = "first-disp1";
  }

  function pressButt2() {
    pressB2 = "background-color: #EF8C3B";
    pressB1 = "background-color: transparent";
    pressB3 = "background-color: transparent";
    firstL = "sec-disp";
    firstD = "sec-disp1";
  }

  function pressButt3() {
    pressB3 = "background-color: #138EF5";
    pressB1 = "background-color: transparent";
    pressB2 = "background-color: transparent";
    firstL = "third-disp";
    firstD = "third-disp1";
  }

  import Home from "./home.svelte";
  import Problem from "./problem.svelte";
  import Solution from "./solution.svelte";

  import Home1 from "./home1.svelte";
  import Problem1 from "./problem1.svelte";
  import Solution1 from "./solution1.svelte";
</script>

<div class="basic">
  <div class="navbar">
    <div class="navbar-1">
      <div class="home">
        <div>
          <button class="home-button" on:click={pressButt1}
            ><img src="Home.png" style="width: 60px;" alt="Logotip1" /></button
          >
        </div>
        <div class="text-home">Home</div>
        <div class="line1" style={pressB1} />
      </div>
      <div class="problem">
        <div>
          <button class="problem-button" on:click={pressButt2}
            ><img
              src="Problem1.png"
              style="width: 60px;"
              alt="Logotip1"
            /></button
          >
        </div>
        <div class="text-problem">Problem</div>
        <div class="line2" style={pressB2} />
      </div>
      <div class="solution">
        <div>
          <button class="solution-button" on:click={pressButt3}
            ><img
              src="Solution.png"
              style="width: 60px;"
              alt="Logotip1"
            /></button
          >
        </div>
        <div class="text-solution">Solution</div>
        <div class="line3" style={pressB3} />
      </div>
    </div>
    <div class="navbar-2" />
  </div>
  <div class="middle">
    <div class="middle-l">
      {#if firstL === "first-disp"}
        <Home />
      {:else if firstL === "sec-disp"}
        <Problem />
      {:else if firstL === "third-disp"}
        <Solution />
      {/if}
    </div>
    <div class="midle-r">
      {#if firstD === "first-disp1"}
        <Home1 />
      {:else if firstD === "sec-disp1"}
        <Problem1 />
      {:else if firstD === "third-disp1"}
        <Solution1 />
      {/if}
    </div>
  </div>
  <div class="footer">
    <p>Copyrights by Asad Karisik</p>
    <p>
      Contact:
      <a href="mailto:asad.karisik@gmail.com">asad.karisik@gmail.com</a>
    </p>
  </div>
</div>

<style>
  * {
    padding: 0;
    margin: 0;
    background-color: black;
    font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
    box-sizing: border-box;
  }

  .basic {
    height: 100vh;
  }
  .navbar {
    background-color: rgb(0, 0, 0);
    display: flex;
    flex-wrap: wrap;
    height: 25%;
    color: white;
  }
  .navbar-1 {
    background-color: rgb(0, 0, 0);
    width: 60%;
    display: flex;
    align-items: center;
    text-align: center;
    justify-content: space-around;
  }
  .home {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .problem {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .solution {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .line1 {
    width: 60px;
    height: 5px;
    background-color: transparent;
    margin-top: 2px;
  }
  .line2 {
    width: 60px;
    height: 5px;
    background-color: transparent;
    margin-top: 2px;
  }
  .line3 {
    width: 60px;
    height: 5px;
    background-color: transparent;
    margin-top: 2px;
  }
  .text-problem {
    color: #ef8c3b;
  }
  .text-solution {
    color: #138ef5;
  }

  .navbar-2 {
    background-color: rgb(0, 0, 0);
    width: 40%;
  }

  .middle {
    background-color: rgb(0, 0, 0);
    display: flex;
    height: 68%;
    color: white;
  }
  .middle-l {
    background-color: rgb(0, 0, 0);
    width: 60%;
    display: flex;
    flex-direction: column;
  }
  .midle-r {
    background-color: rgb(0, 0, 0);
    width: 40%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    color: rgb(255, 255, 255);
  }
  .footer {
    background-color: rgb(0, 0, 0);
    height: 7%;
    color: white;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-top: #138ef5 1px solid;
  }

  a:link {
    color: #138ef5;
    background-color: transparent;
    text-decoration: none;
    cursor: pointer;
  }
  a:visited {
    color: #ffffff;
    background-color: transparent;
    text-decoration: none;
  }
  a:hover {
    color: #138ef5;
    background-color: transparent;
    text-decoration: underline;
    cursor: pointer;
  }
  .home-button:hover {
    cursor: pointer;
  }
  .problem-button:hover {
    cursor: pointer;
  }
  .solution-button:hover {
    cursor: pointer;
  }

  @media screen and (max-width: 800px) {
    .middle,
    .navbar {
      flex-direction: column;
    }
    .navbar-1,
    .middle-l,
    .midle-r {
      width: 100%;
    }
    .basic {
      height: auto;
      justify-content: center;
      align-items: center;
      margin: 0;
      padding: 0;
    }
  }
</style>
