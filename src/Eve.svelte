<script>
  export let millisecondsUntilChristmas;

  let seasonsGreetings;
  let eve;

  const dayMilliseconds = 1000 * 60 * 60 * 24;
  $: daysUntilChristmas = Math.floor(
    millisecondsUntilChristmas / dayMilliseconds
  );
  // Set the max 'eves' to 150 so peoples devices don't struggle
  $: days = Math.min(150, daysUntilChristmas);

  let mouseDistance = { x: 0, y: 0 };
  const handleMousemove = (e) => {
    if (daysUntilChristmas > 0) {
      mouseDistance.x =
        e.clientX -
        seasonsGreetings.offsetLeft -
        seasonsGreetings.offsetWidth +
        eve.offsetWidth / 2;
      mouseDistance.y =
        seasonsGreetings.offsetTop - e.clientY - eve.offsetHeight / 2;
    }
  };
</script>

<svelte:body on:mousemove={handleMousemove} />

<h1 class="text-box seasons-greetings" bind:this={seasonsGreetings}>
  merry christmas
  <span style="position: relative; width: 0; height: 0">
    {#each { length: days } as _, i}
      <span
        class="eve"
        style="bottom:{(mouseDistance.y / days) *
          (i + 1.0)}px;left:{(mouseDistance.x / days) * (i + 1.0)}px"
        bind:this={eve}
      >
        {#if i === days}
          eve!!!
        {:else}
          eve
        {/if}
      </span>
    {/each}
  </span>
  <span>
    {#if millisecondsUntilChristmas > 0}
      eve
    {/if}
  </span>
</h1>

<style lang="scss">
  .seasons-greetings {
    background-image: url(../assets/border3.svg);
    bottom: 30px;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
    position: absolute;
    padding-top: 16px;
    padding-bottom: 12px;

    @media (max-width: 450px) {
      bottom: 120px;
    }

    &:before {
      background-image: url(../assets/border_transparent3.svg);
    }

    .eve:nth-child(odd) {
      color: var(--main-bg-color);
      -webkit-text-stroke: 1.5px #000;
    }

    .eve:nth-child(even) {
      -webkit-text-stroke: 1.5px var(--secondary-color);
    }

    .eve {
      color: #fff;
      font-weight: 700;
      position: absolute;
      user-select: none;
    }
  }
</style>
