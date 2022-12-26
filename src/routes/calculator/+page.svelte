<script>
  const TL1 = 20;
  const TT1 = 10;
  const TO1 = 10;
  const TL2 = 0;
  const TT2 = 0;
  const TO2 = 0;
  let nameCity = "";
  let population = 0;
  let TL = 20;
  let TT = 10;
  let TO = 10;
  $: shipments = population * 0.1;
  $: tukupT1 = TL1 + TT1 + TO1;
  $: tukupT2 = TL2 + TT2 + TO2;
  $: tukupT = TL + TT + TO;

  $: frekCity1 = Math.round(shipments / ((30 / (30 + tukupT1)) * 24));
  $: frekCity2 = Math.round(shipments / ((30 / (30 + tukupT2)) * 24));
  $: frekCity3 = Math.round(shipments / ((30 / (30 + tukupT)) * 24));

  const shipmentsGlob = 5000000 * 0.1;
  const frekGlob1 = Math.round(
    shipmentsGlob / ((30 / (30 + TL1 + TT1 + TO1)) * 24)
  );

  const frekGlob2 = Math.round(
    shipmentsGlob / ((30 / (30 + TL2 + TT2 + TO2)) * 24)
  );

  $: frekGlob3 = Math.round(shipmentsGlob / ((30 / (30 + tukupT)) * 24));

  $: VPDC = Math.round(frekCity1 - frekCity3);
  $: VPYC = Math.round((frekCity1 - frekCity3) * 365);
  $: VPDG = Math.round(frekGlob1 - frekGlob3);
  $: VPYG = Math.round((frekGlob1 - frekGlob3) * 365);

  let style;
  $: if (VPDC > 0 || VPDG > 0) {
    style = "background-color: #138ef5;";
  } else if (VPDC < 0 || VPYG < 0 || VPDG < 0 || VPYG < 0) {
    style = "background-color: #ef8c3b;";
  } else if (VPDC === 0 && VPDG === 0) {
    style = "background-color: #f3f3f3;";
  }

  //odavdje brisanje
  import { scaleLinear } from "d3-scale";

  const points = [
    { year: "Scenario 1", frekvencija: 2000.8 },
    { year: "Scenario 2", frekvencija: 1400.6 },
    { year: "Scenario 3", frekvencija: 2320.4 },
  ];
  $: points[0].frekvencija = frekCity1;
  $: points[1].frekvencija = frekCity2;
  $: points[2].frekvencija = frekCity3;

  // ubaceno - drugi chart
  const pointss = [
    { year: "Scenario 1", frekvenc: 2000.8 },
    { year: "Scenario 2", frekvenc: 1400.6 },
    { year: "Scenario 3", frekvenc: 2320.4 },
  ];

  pointss[0].frekvenc = frekGlob1;
  pointss[1].frekvenc = frekGlob2;
  $: pointss[2].frekvenc = frekGlob3;

  const xTicks = [1000, 1000, 1000];
  const yTicks = [0, 200, 400, 600, 800, 1000];
  const padding = { top: 20, right: 15, bottom: 20, left: 50 };

  $: if (population < 50000) {
    yTicks[0] = 0;
    yTicks[1] = 200;
    yTicks[2] = 400;
    yTicks[3] = 600;
    yTicks[4] = 800;
    yTicks[5] = 1000;
  } else if (population > 50000 && population < 500000) {
    yTicks[0] = 0;
    yTicks[1] = 2000;
    yTicks[2] = 4000;
    yTicks[3] = 6000;
    yTicks[4] = 8000;
    yTicks[5] = 10000;
  } else if (population > 500000 && population < 5000000) {
    yTicks[0] = 0;
    yTicks[1] = 20000;
    yTicks[2] = 40000;
    yTicks[3] = 60000;
    yTicks[4] = 80000;
    yTicks[5] = 100000;
  } else if (population > 5000000) {
    yTicks[0] = 0;
    yTicks[1] = 100000;
    yTicks[2] = 200000;
    yTicks[3] = 300000;
    yTicks[4] = 400000;
    yTicks[5] = 500000;
  }

  let width = 500;
  let height = 200;

  function formatMobile(tick) {
    return "'" + tick.toString().slice(-2);
  }

  $: xScale = scaleLinear()
    .domain([0, xTicks.length])
    .range([padding.left, width - padding.right]);

  $: yScale = scaleLinear()
    .domain([0, Math.max.apply(null, yTicks)])
    .range([height - padding.bottom, padding.top]);

  $: innerWidth = width - (padding.left + padding.right);
  $: barWidth = innerWidth / xTicks.length;

  //tikovi - drugi chart

  const xTickss = [1000, 1000, 1000];
  const yTickss = [0, 20000, 40000, 60000, 80000, 100000];
  const paddings = { top: 20, right: 15, bottom: 20, left: 50 };

  $: xScales = scaleLinear()
    .domain([0, xTickss.length])
    .range([paddings.left, width - paddings.right]);

  $: yScales = scaleLinear()
    .domain([0, Math.max.apply(null, yTickss)])
    .range([height - paddings.bottom, paddings.top]);

  $: innerWidths = width - (paddings.left + paddings.right);
  $: barWidths = innerWidths / xTickss.length;

  //novi chartovi
</script>

<div class="flex-container">
  <div class="flex-container-1">
    <div class="box-1" />
    <div class="box-2" />
    <div class="box-3">
      <div class="naslov">
        <div>
          <h2 class="naslov-1"><a href="/">Blue Impact Estimator</a></h2>
        </div>
      </div>
      <div>
        <p class="naslov-1tekst">
          The logistics inefficiencies are the cause of different loss of time
          that creates excessive transport emissions. The following loss in time
          is given as default for comparison purposes, their values vary, and
          they have higher values in most of the cities globally:
        </p>
        <ul>
          <li>T<sub>L</sub> Average dwell time (20 min per shipment)</li>
          <li>
            T<sub>T</sub> Average congestion loss in time (10 min per shipment)
          </li>
          <li>T<sub>O</sub> Other loss in time (10 min per shipment)</li>
        </ul>
        <p class="naslov-1tekst">
          This calculator estimates how number of vehicles/trucks that are
          necessary for daily operations in the cities depends on loss in time
          values. The figures on the right represent the relationship for the
          selected city and globally. Each of the figures has three scenarios:
        </p>
        <ul>
          <li>Scenario 1 - default values for loss of time</li>
          <li>Scenario 2 - time losses at a 0 value</li>
          <li>
            Scenario 3 - time losses to be manipulated within this application
          </li>
        </ul>
        <p class="naslov-1tekst">
          There are almost 5 billion people living in urban areas in more than
          10 thousands cities in the world. Both figures, represent how
          introduction of solutions that are applicable globally, could
          contribute to the better environment with a significant impact.
        </p>
      </div>
      <div class="gia-area">
        <h2 class="gia-naslov">Inefficiency Impact Indicators</h2>
        <p>
          The more time is lost for logistics inefficiencies, the more working
          hours for vehicles engaged or more new vehicles are necessary. More
          working hours and more vehicles leads to CO2, Particulate matter and
          Nitrogen Oxide emissions!
        </p>
        <p>
          By reducing loss in time (T<sub>L</sub>, T
          <sub>T</sub>
          or T<sub>O</sub>) one could see how many equivalent vehicles-pollution
          days VPD or vehicles-pollution days per year VPD/Y could be achieved.
        </p>
      </div>
    </div>
    <div class="box-4" />
  </div>

  <div class="flex-container-2">
    <div class="prva-cjelina">
      <div class="grad-populacija">
        <div>
          <input
            bind:value={nameCity}
            placeholder="enter the city name"
            style="height: 30px; border-radius:5px; font-size:15px"
            maxlength="10"
          />
          <div style="font-size: 10px;">City</div>
        </div>
        <div>
          <div style="margin-left:10px">
            <input
              style="height: 30px; border-radius:5px; font-size:15px"
              type="number"
              bind:value={population}
              min="10000"
              max="30000000"
              step="10000"
            />
          </div>
          <div style="font-size: 10px; margin-left:10px">Population</div>
        </div>
      </div>
      <div class="vrem-gubici">
        <div>
          <input
            type="number"
            style="height: 30px; border-radius:5px; font-size:15px"
            bind:value={TL}
            min="0"
            max="40"
            step="1"
          />
          <div class="unos-pr" style="font-size: 10px;">
            T<sub>L</sub>(minutes)
          </div>
        </div>
        <div style="margin-left:15px">
          <input
            type="number"
            style="height: 30px; border-radius:5px; font-size:15px"
            bind:value={TT}
            min="0"
            max="20"
            step="1"
          />
          <div class="unos-pr" style="font-size: 10px;">
            T<sub>T</sub>(minutes)
          </div>
        </div>
        <div style="margin-left:15px">
          <input
            type="number"
            style="height: 30px; border-radius:5px; font-size:15px"
            bind:value={TO}
            min="0"
            max="20"
            step="1"
          />
          <div class="unos-pr" style="font-size: 10px;">
            T<sub>O</sub>(minutes)
          </div>
        </div>
      </div>
    </div>
    <div class="druga-cjelina">
      <div class="cities">
        <div>
          <div class="number-poll" {style}>
            {VPDC}
          </div>
          <div class="number-tekst" {style}>
            Vehicle Pollution Days<br />
            {nameCity}
          </div>
        </div>
        <div>
          <div class="number-poll" {style}>{VPYC}</div>
          <div class="number-tekst" {style}>
            Vehicle Pollution Days per Year <br />
            {nameCity}
          </div>
        </div>
      </div>
      <div class="global">
        <div>
          <div class="number-poll" {style}>{VPDG}</div>
          <div class="number-tekst" {style}>
            Vehicle Pollution Days Globally
          </div>
        </div>
        <div>
          <div class="number-poll" {style}>{VPYG}</div>
          <div class="number-tekst" {style}>
            Vehicle Pollution Days per Year Globally
          </div>
        </div>
      </div>
    </div>
    <div class="treca-cjelina">
      <div>
        <h3 class="head-chart">Number of trucks for city per day</h3>
        <div class="chart" bind:clientWidth={width} bind:clientHeight={height}>
          <svg>
            <!-- y axis -->
            <g class="axis y-axis">
              {#each yTicks as tick}
                <g
                  class="tick tick-{tick}"
                  transform="translate(0, {yScale(tick)})"
                >
                  <line x2="100%" />
                  <text y="-4"
                    >{tick}
                    {tick === yTicks[5] ? " vehicles per day " : ""}</text
                  >
                </g>
              {/each}
            </g>

            <!-- x axis -->
            <g class="axis x-axis">
              {#each points as point, i}
                <g class="tick" transform="translate({xScale(i)},{height})">
                  <text x={barWidth / 2} y="-4"
                    >{width > 380 ? point.year : formatMobile(point.year)}</text
                  >
                </g>
              {/each}
            </g>

            <g class="bars">
              {#each points as point, i}
                <rect
                  x={xScale(i) + 2}
                  y={yScale(point.frekvencija)}
                  width={barWidth - 4}
                  height={yScale(0) - yScale(point.frekvencija)}
                />
              {/each}
            </g>
          </svg>
        </div>
      </div>
      <div>
        <h3 class="head-chart">Applied on world urban population</h3>

        <div class="chart" bind:clientWidth={width} bind:clientHeight={height}>
          <svg>
            <!-- y axis -->
            <g class="axis y-axis">
              {#each yTickss as tick}
                <g
                  class="tick tick-{tick}"
                  transform="translate(0, {yScales(tick)})"
                >
                  <line x2="100%" />
                  <text y="-4"
                    >{tick}
                    {tick === 100000
                      ? " vehicles per day in thousands"
                      : ""}</text
                  >
                </g>
              {/each}
            </g>

            <!-- x axis -->
            <g class="axis x-axis">
              {#each pointss as point, i}
                <g class="tick" transform="translate({xScales(i)},{height})">
                  <text x={barWidths / 2} y="-4"
                    >{width > 380 ? point.year : formatMobile(point.year)}</text
                  >
                </g>
              {/each}
            </g>

            <g class="barss">
              {#each pointss as point, i}
                <rect
                  x={xScales(i) + 2}
                  y={yScales(point.frekvenc)}
                  width={barWidths - 4}
                  height={yScales(0) - yScales(point.frekvenc)}
                />
              {/each}
            </g>
          </svg>
        </div>
      </div>
    </div>
    <div class="cetvrta-cjelina">
      <div class="cetvrta-prva">
        <div style="color: white;">
          <div>Scenario 1</div>
          <div>
            <b>{frekCity1} </b>
          </div>
          <div>Vehicles per Day</div>
        </div>
        <div style="color: white; margin-left:30px">
          <div>Scenario 2</div>
          <div>
            <b>{frekCity2}</b>
          </div>
          <div>Vehicles per Day</div>
        </div>
        <div style="color: white; margin-left:30px">
          <div>Scenario 3</div>
          <div>
            <b>{frekCity3}</b>
          </div>
          <div>Vehicles per Day</div>
        </div>
      </div>
      <div class="cetvrta-druga">
        <div style="color: #ef8c3b;">
          <div>Scenario 1</div>
          <div>
            <b>{frekGlob1 * 1000}</b>
          </div>
          <div>Vehicles per Day</div>
        </div>
        <div style="color: #ef8c3b; margin-left:30px">
          <div>Scenario 2</div>
          <div>
            <b>{frekGlob2 * 1000}</b>
          </div>
          <div>Vehicles per Day</div>
        </div>
        <div style="color: #ef8c3b; margin-left:30px">
          <div>Scenario 3</div>
          <div>
            <b>{frekGlob3 * 1000}</b>
          </div>
          <div>Vehicles per Day</div>
        </div>
      </div>
    </div>
    <div class="line-6">
      <div class="footer-1"><a href="/">Home</a></div>
      <div class="footer-1">Copyrights by Asad Karisik</div>
      <div class="footer-1">
        Contact: <a href="mailto:asad.karisik@gmail.com"
          >asad.karisik@gmail.com</a
        >
      </div>
    </div>
  </div>
</div>

<style>
  * {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
  }
  .flex-container {
    display: flex;
    flex-wrap: wrap;
    background-color: rgb(0, 0, 0);
    min-height: 900px;
  }
  .flex-container-1 {
    display: flex;
    background-color: rgb(255, 255, 255);
    margin: 0px;
    flex: 40%;
    height: 100vh;
  }
  .flex-container-2 {
    background-color: rgb(255, 255, 255);
    flex: 60%;
    height: 100vh;
    display: flex;
    flex-direction: column;
  }
  .box-1 {
    width: 8%;
    background-color: rgb(255, 255, 255);
    min-height: 100vh;
  }
  .box-2 {
    width: 0.5%;
    background-color: #f3f3f3;
    min-height: 100vh;
  }
  .box-3 {
    width: 91%;
    min-height: 100vh;
    color: rgb(0, 0, 0);
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .naslov-1 {
    color: #138ef5;
    width: 100%;
    text-align: center;
    font-size: 1.5em;
    margin: 2%;
    padding: 0.2em 0 1em 0;
    margin: 3 3 1em 3;
    border-bottom: 0.5px solid #138ef5;
  }
  .naslov-1tekst {
    font-size: 1em;
    text-align: center;
    margin: 2%;
    color: rgb(0, 0, 0);
  }

  .gia-naslov {
    color: rgb(61, 29, 29);
    padding: 0 0 0.2em 0;
    margin: 0 0 1em 0;
    border-bottom: 1px solid rgb(61, 29, 29);
  }
  .gia-area {
    width: 90%;
    height: 25%;
    border: 1px solid #aaa;
    border-radius: 2px;
    box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
    padding: 1em;
    align-items: center;
    justify-content: center;
    margin: 2%;
    margin-top: 40px;
    color: rgb(61, 29, 29);
    font-size: 1em;
  }
  ul {
    width: 90%;
  }
  .box-4 {
    width: 0.5%;
    background-color: #f3f3f3;
    min-height: 100vh;
  }

  .prva-cjelina {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    height: 8%;
    width: 100%;
    align-items: center;
    justify-content: space-around;
    color: rgb(255, 255, 255);
    background-color: rgb(61, 29, 29);
  }
  .grad-populacija {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }
  .vrem-gubici {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
  }
  .druga-cjelina {
    display: flex;
    flex-wrap: wrap;
    height: 20%;
    flex-direction: row;
    background-color: #f3f3f3;
    align-items: center;
    justify-content: space-around;
  }
  .cities,
  .global {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    width: 50%;
    align-items: center;
    justify-content: space-around;
  }

  .number-poll {
    display: flex;
    width: 200px;
    height: 80px;
    border: #000000;
    color: rgb(0, 0, 0);
    margin: 10px;
    font-size: larger;
    text-align: center;
    align-items: center;
    justify-content: center;
    border-radius: 5px;
    border: grey 1px solid;
  }
  .number-tekst {
    display: flex;
    color: rgb(0, 0, 0);
    width: 200px;
    height: 60px;
    margin: 10px;
    text-align: center;
    align-items: center;
    justify-content: center;
    border-radius: 5px;
    border: grey 1px solid;
  }

  .treca-cjelina {
    height: 54%;
    flex-wrap: wrap;
    background-color: rgb(255, 255, 255);
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
  }

  .cetvrta-cjelina {
    height: 12%;
    background-color: rgb(61, 29, 29);
    width: 100%;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    text-align: center;
    justify-content: space-around;
    font-size: small;
  }
  .cetvrta-prva {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    text-align: center;
    justify-content: center;
  }
  .cetvrta-druga {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    text-align: center;
    justify-content: space-around;
  }

  .unos-pr {
    padding: 5 0 0 1.5em;
    background: 0 0 no-repeat;
    background-size: 20px 20px;
    margin: 0 0 0.5em 0;
    line-height: 1.5;
  }

  @media (max-width: 1500px) {
    .gia-area {
      height: auto;
    }
  }
  @media (max-width: 1000px) {
    .box-1 {
      width: 1%;
    }
    .box-3 {
      width: 98%;
    }
    .gia-area {
      margin-top: 20px;
    }
    .flex-container {
      min-height: auto;
    }
    .cetvrta-cjelina {
      height: auto;
    }
  }
  @media (max-width: 1450px) {
    .flex-container-1 {
      height: auto;
    }
    .flex-container-2 {
      height: auto;
    }
    .druga-cjelina {
      height: auto;
    }
    .treca-cjelina {
      height: auto;
    }
  }
  @media (max-width: 1000px) {
    .line-6 {
      flex-direction: column;
    }
    .treca-cjelina {
      height: auto;
    }
  }
  @media (max-width: 800px) {
    .flex-container-1,
    .flex-container-2 {
      flex: 100%;
    }
    .prva-cjelina {
      height: auto;
    }
    .druga-cjelina {
      height: auto;
    }
    .treca-cjelina {
      height: auto;
    }
  }
  @media (max-width: 600px) {
    .cities,
    .global {
      flex-direction: column;
    }
    .number-poll {
      width: 150px;
      height: 60px;
    }
    .number-tekst {
      width: 150px;
      height: 60px;
    }
    .cetvrta-cjelina {
      font-size: x-small;
    }
  }
  .head-chart {
    text-align: center;
    color: #00000070;
  }

  .chart {
    width: 110%;
    max-width: 500px;
    margin: 0 auto;
  }

  svg {
    position: relative;
    width: 100%;
    height: 270px;
  }

  .tick {
    font-family: Helvetica, Arial;
    font-size: 0.725em;
    font-weight: 200;
  }

  .tick line {
    stroke: #72727270;
    stroke-dasharray: 2;
  }

  .tick text {
    fill: #72727270;
    text-anchor: start;
  }

  .tick.tick-0 line {
    stroke-dasharray: 0;
  }

  .x-axis .tick text {
    text-anchor: middle;
  }

  .bars rect {
    fill: rgb(61, 29, 29);
    stroke: none;
    opacity: 0.65;
  }

  .barss rect {
    fill: #ef8c3b;
    stroke: none;
    opacity: 0.65;
  }
  .line-6 {
    height: 6%;
    background-color: #f3f3f3;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    color: rgb(61, 29, 29);
    justify-content: space-around;
  }
  .footer-1 {
    font-size: 15px;
    padding: 0;
    margin: 0;
  }
  a:link {
    color: rgb(61, 29, 29);
    background-color: transparent;
    text-decoration: none;
  }
  a:visited {
    color: rgb(61, 29, 29);
    background-color: transparent;
    text-decoration: none;
  }
  a:hover {
    color: rgb(0, 0, 0);
    background-color: transparent;
    text-decoration: underline;
  }
</style>
