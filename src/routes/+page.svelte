<script>
  import "../app.css";
  import { onMount } from "svelte"
  import moment from 'moment'

  /**
     * @type {any[]}
     */
  let matches = []

  /**
     * @param {{ teams: string; size: any; }} match
     */
  function getTeamsize(match) {
    if (match.teams == 'chosen') {
      return `Chosen To${match.size}`
    } else if (match.teams == 'random') {
      return `Random To${match.size}`
    } else if (match.teams == 'rvb') {
      return `Red v. Blue`
    } else if (match.teams == 'market') {
      return `Auctions`
    } else if (match.teams == 'ffa') {
      return 'FFA'
    }
  }

  /**
     * @param {{ scenarios: any[]; }} match
     */
  function getScenarios(match) {
    return match.scenarios.join(', ')
  }

  /**
     * @param {{ opening: moment.MomentInput; }} match
     */
  function getOpening(match) {
    return moment.utc(match.opens).fromNow()
  }

  onMount(() => {
    async function fetchData() {
      matches = []
      console.log('Updating matches...')
      const res = await fetch("https://hosts.uhc.gg/api/matches/upcoming") 
      const data = await res.json()
      console.log(data)
      for (const match of data) {
        if (match.address.endsWith('applejuice.games') || match.address.endsWith('applejuice.bar')) {
          matches.push(match)
          matches = matches
          console.log(`Found match ${match.id} as a valid match.`)
        }
      }
    }

    const interval = setInterval(fetchData, 15000)
    fetchData()

    return () => clearInterval(interval)
  })

  let rulesShown = false
  /**
     * @param {any} e
     */
  function handleRulesClick(e) {
    rulesShown = !rulesShown
  }
</script>

<main class="container pt-6 pl-6">
  <script src="https://kit.fontawesome.com/2fa8dd0704.js" crossorigin="anonymous"></script>
  <h1 class="font-bold text-3xl">
    <img alt="Logo" class="mb-2 inline" src="https://em-content.zobj.net/source/skype/289/beverage-box_1f9c3.png" height="48" width="48"/>&nbsp;applejuice.games
  </h1>
  <h2 class="text-xl mt-2 text-gray-700">
    <i class="fa-brands fa-discord"></i> <a class="hover:underline" href="https://discord.com/invite/X7ujJThANP" target="_blank" rel="noopener noreferrer">Join us on Discord</a> <br/> 
    <i class="fa-brands fa-twitter"></i> <a class="hover:underline" href="https://twitter.com/applejuiceuhc" target="_blank" rel="noopener noreferrer">Follow us on Twitter</a> <br/> 
    <i class="fa-solid fa-store"></i> <a class="hover:underline" href="https://applejuice.tebex.io" target="_blank" rel="noopener noreferrer">Purchase a rank on our store</a> <br/>
    <i class="fa-solid fa-clipboard-user"></i> <a class="hover:underline" href="https://forms.gle/Z2fYxzqLLNDd3Fjv7" target="_blank" rel="noopener noreferrer">Apply for Staff</a> <br/>
    <i class="fa-solid fa-gavel"></i> <a class="hover:underline" href="https://forms.gle/S3a7WLFCBBWudpLbA" target="_blank" rel="noopener noreferrer">Banned? Appeal here!</a> <br/>
    <i class="fa-solid fa-camera-web"></i> <a class="hover:underline" href="https://forms.gle/wfMBS2JD2SYPuojt5" target="_blank" rel="noopener noreferrer">Apply for media rank here!</a> <br/>
    
    
  </h2>
  <h1 class="mt-6 font-bold text-2xl">
    <i class="fa-solid fa-book"></i> Rules
  </h1>
  <button on:click={handleRulesClick} class="mb-2 mt-2 mr-6 text-lg pt-2 pb-2 pl-4 pr-4 rounded bg-red-50 hover:bg-red-100">Show rules</button>
  {#if rulesShown == true}
    <div id="rules">
      <h2>The following can get you banned:</h2>
      <ul>
          <li><strong>Usage of a Hacked Client</strong> - Using a hacked clinet.</li>
          <li><strong>Unfair Advantage</strong> - Knowingly obtaining items from teammates or F5 Abusing.</li>
          <li><strong>X-ray / Cavefinder</strong> - Using a mod or resource pack to help you find ores/caves more easier.</li>
          <li><strong>Extreme Disrespect</strong> - Extreme disrespect to players or staff.</li>
          <li><strong>Death Wishes/Threats</strong> - Wishing or threatening death upon someone.</li>
          <li><strong>Breaking Random Team Rules</strong> - Suiciding &amp; leaving the game during a random teams game qualifies as this.</li>
          <li><strong>DDoS-ing</strong> - Attempts to, or threats to get the server booted offline qualify as breaking this rule.</li>
          <li><strong>Dox-ing</strong> - Attempts to, or threats to reveal personal information about players qualify as breaking this rule.</li>
          <li><strong>iPvP</strong> - Attempting to damage players before PvP qualify as breaking this rule.</li>
          <li><strong>Not listening to staff</strong> - Not listening to staff despite receiving multiple warnings.</li>
          <li><strong>Stalking</strong> - Continuing to stalk/killing a player despite being warned not to.</li>
          <li><strong>Bug/Glitch Abuse</strong> - Abusing a bug/glitch in the server.</li>
          <li><strong>Extreme Spoiling</strong> - Spoiling crucial information, e.g. moles in a game.</li>
          <li><strong>Crossteaming</strong> - If not allowed, teaming with someone in a teams game or an FFA game qualifies as crossteaming.</li>
      </ul>
      <br/>
      <h2>The following can get you muted:</h2>
      <ul>
          <li><strong>Disrespect</strong> - Mild disrespect to players or staff.</li>
          <li><strong>Spoiling</strong> - Mild spoiling, e.g. revealing what other players have in terms of gear.</li>
          <li><strong>Spamming</strong> - Mild spamming, e.g. sending 3 of the same message within a short period of time.</li>
          <li><strong>Extreme Spamming</strong> - Extreme spamming, e.g. sending 10 of the same message within a short period of time.</li>
          <li><strong>Annoying</strong> - Being a bother to the players/host.</li>
          <li><strong>Spamming Help-Op</strong> - Abusing the /helpop system with useless requests.</li>
          <li><strong>Timeout</strong> - Blanket term for a player being a general nuisance in chat; spamming players; raging, etc.</li>
          <li><strong>Discrimination</strong> - Blanket term for a player discriminating against other players for arbitrary things, i.e. their race, sex, etc.</li>
      </ul>
      <br/>
      <h2>The following can get you warned:</h2>
      <ul>
          <li><strong>Prebuilding</strong> - Utilized in SkyHigh games, when a player's base is too close (about 20 blocks) to another's.</li>
          <li><strong>Groundcamping</strong> - Utilized in SkyHigh games, when its disallowed, a player is not disallowed to go down to camp (stay down to wait for other players).</li>
          <li><strong>Skybasing/Towering</strong> - Towering even after it's Meetup, typically only warnable when it's disallowed.</li>
          <li><strong>Go to 0,0, Mining at Meetup or Go to Surface</strong> - Not going to 0,0, taking a long time to get to 0,0 because of enchanting, shearing, mining, etc.</li>
          <li><strong>Running at Meetup</strong> - Blanket term for people that run away from multiple fights at meetup, this rule is typically loose depending on the host.</li>
          <li><strong>Teaming in Arena</strong> - Crossteaming with other players in the FFA Arena.</li>
      </ul>
    </div>
  {/if}
  <h1 class="mt-4 font-bold text-2xl">
    <i class="fa-solid fa-signs-post"></i> Matches
  </h1>
  {#if matches.length == 0} 
    <h2 class="mt-2 text-xl text-gray-700">Searching for matches...</h2>
  {:else}
    <div class="sm:container sm:mx-auto">
      {#each matches as match}
        {#if match.removed == false || match.removed == null}
          <div class="pt-4 pb-4 mt-4 shadow-md box-border bg-white">
            <h2 class="ml-4 font-bold text-2xl"><a href="https://namemc.com/{match.hostingName}" target="_blank" rel="noopener noreferrer"><img alt={match.hostingName} class="inline rounded mb-2" src="https://minotar.net/helm/{match.hostingName}/32.png"/></a> {match.hostingName}'s #{match.count}</h2>
            <h3 class="ml-4 text-lg">Teamsize: <strong>{getTeamsize(match)}</strong></h3>
            <h3 class="ml-4 text-lg">Scenarios: <strong>{getScenarios(match)}</strong></h3>
            <h3 class="ml-4 text-lg">Opening: <strong>{getOpening(match)}</strong></h3>
            <h3 class="ml-4 text-md"><strong>u/{match.author}</strong> • <a class="text-sky-500 hover:underline" href="https://hosts.uhc.gg/m/{match.id}" target="_blank" rel="noopener noreferrer">Matchpost</a></h3>
          </div>
        {/if}
      {/each}
    </div>
  {/if}
  <h3 class="mt-6 text-md">
    applejuice.games is owned & created by <a class="text-sky-500 hover:underline" href="https://twitter.com/minotadev" target="_blank" rel="noopener noreferrer">minota</a> • (c) 2023
  </h3>
</main>