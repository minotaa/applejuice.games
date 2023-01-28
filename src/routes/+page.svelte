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
</script>

<main class="container pt-6 pl-6">
  <script src="https://kit.fontawesome.com/2fa8dd0704.js" crossorigin="anonymous"></script>
  <h1 class="font-bold text-3xl">
    <img alt="Logo" class="mb-2 inline" src="https://em-content.zobj.net/source/skype/289/beverage-box_1f9c3.png" height="48" width="48"/>&nbsp;applejuice.games
  </h1>
  <h2 class="text-xl mt-2 text-gray-700">
    <i class="fa-brands fa-discord"></i> <a href="https://discord.com/invite/X7ujJThANP" target="_blank" rel="noopener noreferrer">Join us on Discord</a> <br/> <i class="fa-brands fa-twitter"></i> <a href="https://twitter.com/applejuiceuhc" target="_blank" rel="noopener noreferrer">Follow us on Twitter</a> <br/> <i class="fa-solid fa-store"></i> <a href="https://applejuice.tebex.io" target="_blank" rel="noopener noreferrer">Purchase a rank on our store</a>  
  </h2>
  <h1 class="mt-6 font-bold text-2xl">
    <i class="fa-solid fa-signs-post"></i> Matches
  </h1>
  {#if matches.length == 0} 
    <h2 class="mt-2 text-xl text-gray-700">Searching for matches...</h2>
  {:else}
    <div class="sm:container">
      {#each matches as match}
        {#if match.removed == false || match.removed == null}
          <div class="pt-4 pb-4 mt-4 shadow-md box-border">
            <h2 class="ml-4 font-bold text-2xl"><a href="https://namemc.com/{match.hostingName}" target="_blank" rel="noopener noreferrer"><img alt={match.hostingName} class="inline rounded mb-2" src="https://minotar.net/helm/{match.hostingName}/32.png"/></a> {match.hostingName}'s #{match.count}</h2>
            <h3 class="ml-4 text-lg">Teamsize: <strong>{getTeamsize(match)}</strong></h3>
            <h3 class="ml-4 text-lg">Scenarios: <strong>{getScenarios(match)}</strong></h3>
            <h3 class="ml-4 text-lg">Opening: <strong>{getOpening(match)}</strong></h3>
            <h3 class="ml-4 text-md"><strong>u/{match.author}</strong> • <a class="text-sky-500" href="https://hosts.uhc.gg/{match.id}" target="_blank" rel="noopener noreferrer">Matchpost</a></h3>
          </div>
        {/if}
      {/each}
    </div>
  {/if}
  <h3 class="mt-6 text-md">
    applejuice.games is owned & created by <a class="text-sky-500" href="https://twitter.com/minotadev" target="_blank" rel="noopener noreferrer">minota</a> • (c) 2023
  </h3>
</main>