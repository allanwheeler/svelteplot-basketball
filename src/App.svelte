<script>
  import { fade } from 'svelte/transition';
  import { Plot, Dot, HTMLTooltip, BarX, groupY, Text } from 'svelteplot';
  import Court from './components/Court.svelte';
  import data from './data/shot-data.csv.js';

  let selectedPlayer = $state(null);

  // Filter: remove backcourt, then filter by player if selected
  let filteredData = $derived(
    data
      .filter(d => d.zoneBasic !== 'Backcourt')
      .filter(d => !selectedPlayer || d.namePlayer === selectedPlayer)
  );

  const players = [
    { name: 'Stephen Curry', id: '3975' },
    { name: 'LeBron James', id: '1966' },
  ];

  const toggle = n => (selectedPlayer = selectedPlayer === n ? null : n);
</script>

<div class="top">
  <Plot
    subtitle="Click image to filter by player "
    axes={false}
    aspectRatio={50 / 47}
    x={{ domain: [-250, 250] }}
    y={{ domain: [-50, 420] }}
    color={{ legend: false }}
  >
    {#snippet header()}
      <div class="player-image">
        {#each players as { name, id }}
          <button
            onclick={() => toggle(name)}
            class:active={selectedPlayer === name}
          >
            <img
              src="https://a.espncdn.com/combiner/i?img=/i/headshots/nba/players/full/{id}.png"
              alt={name}
            />
            <span class="player-label">{name}</span>
          </button>
        {/each}

        <div class="chart-container">
          <Plot
            class="small-plot"
            x={{ label: '', axis: false }}
            y={{ label: '' }}
          >
            <BarX
              {...groupY(
                { data: filteredData, y: 'typeShot', fill: 'typeShot' },
                { x: 'count' }
              )}
            />
            <Text
              dx={-20}
              {...groupY(
                { data: filteredData, y: 'typeShot', fill: 'typeShot' },
                { x: 'count' }
              )}
              text={d => d.__x.toLocaleString()}
              fill="#ffffff"
            />
          </Plot>
        </div>
      </div>
    {/snippet}

    <Court />

    {#key selectedPlayer}
      <g out:fade={{ duration: 200 }}>
        <Dot
          canvas={true}
          data={filteredData}
          x="locationX"
          y="locationY"
          fill="typeShot"
          opacity={0.5}
          r={4}
        />
      </g>
    {/key}

    {#snippet overlay()}
      <HTMLTooltip data={filteredData} x="locationX" y="locationY">
        {#snippet children({ datum })}
          <div class="tooltip">
            <div><strong>{datum.namePlayer}</strong></div>
            <div>Zone: {datum.zoneBasic}</div>
            <!-- <div>x: {datum.locationX} y: {datum.locationY}</div> -->
          </div>
        {/snippet}
      </HTMLTooltip>
    {/snippet}
  </Plot>
</div>

<style>
  .top {
    min-width: 500px;
    max-width: 800px;
  }
  :global(h3) {
    font-weight: 400;
  }
  .player-image {
    display: flex;
    gap: 15px;
    margin-bottom: 10px;
    align-items: flex-start;
  }

  .player-image button {
    background: none;
    border: 3px solid transparent;
    padding: 4px;
    cursor: pointer;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    transition: all 0.2s ease;
  }

  .player-image img {
    display: block;
    object-fit: cover;
    height: 70px;
    width: 70px;
    clip-path: circle(50%);
    background: #eee;
  }

  .player-image:has(.active) button:not(.active) {
    filter: grayscale(100%);
    opacity: 0.4;
  }

  .player-label {
    font-size: 11px;
    color: #444;
    white-space: nowrap;
  }

  .chart-container {
    flex: 1;
    min-width: 0;
  }

  .tooltip {
    background: white;
    border: 1px solid #ccc;
    font-size: 12px;
    padding: 1ex 1em;
    border-radius: 3px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
</style>
