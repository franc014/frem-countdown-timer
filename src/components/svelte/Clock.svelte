<script>
  import { onMount } from "svelte";
  import { slide } from "svelte/transition";

  import { sineInOut } from "svelte/easing";
  const dueDate = "2023-11-08 11:59:00";
  const dueTimestamp = new Date(dueDate);

  let remainingTime = {
    days: 0,
    hours: 0,
    minutes: 0,
    seconds: 0,
  };

  let timeOverMessage = "We've made it to the moon!! 🥳";
  let timeOver = false;

  function resetTimer() {
    remainingTime.days = 0;
    remainingTime.hours = 0;
    remainingTime.minutes = 0;
    remainingTime.seconds = 0;
  }

  function startAgain() {
    console.log("sa");
  }

  onMount(function () {
    const interval = setInterval(() => {
      setRemainingTime();
      if (remainingTime.seconds < 0) {
        clearInterval(interval);
        resetTimer();
        timeOver = true;
      }
    }, 1000);

    return () => {
      clearInterval(interval);
    };
  });

  function formatNumber(number) {
    return number < 10 ? `0${number}` : number;
  }

  function setRemainingTime() {
    const currentTimestamp = new Date();

    const remainderTimestamp = dueTimestamp - currentTimestamp;

    const seconds = Math.round(remainderTimestamp / 1000);
    const remainingSeconds = seconds % 60;
    const minutes = Math.floor(seconds / 60);
    const remainingMinutes = minutes % 60;
    const hours = Math.floor(seconds / 3600);

    const days = Math.floor(hours / 24);

    const remainingHours = hours % 24;

    remainingTime.seconds = remainingSeconds;
    remainingTime.minutes = remainingMinutes;
    remainingTime.hours = remainingHours;
    remainingTime.days = days;
  }
</script>

<div class="clock">
  <div>
    <p class="card">
      {#key remainingTime.days}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.days)}
        </span>
      {/key}
    </p>
    <p class="time-label">days</p>
  </div>
  <div>
    <p class="card">
      {#key remainingTime.hours}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.hours)}
        </span>
      {/key}
    </p>
    <p class="time-label">hours</p>
  </div>
  <div>
    <p class="card">
      {#key remainingTime.minutes}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.minutes)}
        </span>
      {/key}
    </p>
    <p class="time-label">minutes</p>
  </div>
  <div>
    <p class="card">
      {#key remainingTime.seconds}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.seconds)}
        </span>
      {/key}
    </p>
    <p class="time-label">seconds</p>
  </div>
</div>
<div class="time-over-message" class:timeOver>
  <p>{timeOverMessage}</p>
  <button on:click={startAgain}>START AGAIN</button>
</div>

<style>
  .clock {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
    place-content: center;
  }
  .time-over-message {
    opacity: 0;
    position: absolute;
    inset: 20px 0 0 0;
    width: 100%;
    height: 100%;
    background: hsl(var(--crl-dark-desat-blue));

    align-items: center;
    justify-content: center;
    padding: 4rem;
    z-index: 2;
    border: 2px solid hsl(var(--crl-soft-red));
    border-radius: 20px;
    container-type: inline-size;
  }
  .time-over-message p {
    font-size: 8cqi;
    max-width: fit-content;
  }

  .time-over-message button {
    cursor: pointer;
    margin-top: 2rem;
    padding: 1rem 2rem;
    letter-spacing: 0.2rem;
    border-radius: 0.5rem;
  }

  .timeOver {
    color: white;
    opacity: 0.9;
  }

  .clock div {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .card {
    width: 100%;
    height: 100%;
    aspect-ratio: 1 / 1;
    color: hsl(var(--crl-soft-red));
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    border-bottom: 10px solid hsl(var(--crl-darker-blue));
    border-radius: 20px;

    container-type: inline-size;
    position: relative;

    z-index: 1;
    margin-bottom: 1rem;
  }

  .card::before {
    content: "";
    position: absolute;
    display: block;
    inset: 0;
    width: 100%;
    height: 106%;

    background: url("/images/number-deco.svg");
    background-repeat: no-repeat;
    background-size: contain;
  }
  .card::after {
    content: "";
    position: absolute;
    display: block;
    bottom: 0;
    top: 50%;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    background: url("/images/number-deco-down.svg");
    background-repeat: no-repeat;
    background-size: contain;
    margin-top: -5px;
  }

  .card span {
    font-size: 40cqi;
    display: flex;
    align-items: center;
    justify-content: center;
    backface-visibility: hidden;
    font-feature-settings: "tnum";
    text-shadow: 2px 2px hsl(var(--crl-dark-blue));
  }

  .time-label {
    color: hsl(var(--crl-grayish-blue));
    text-transform: uppercase;
    letter-spacing: 0.4rem;
    font-size: 0.8rem;
  }
</style>
