<script>
  import { onMount } from "svelte";
  import { fly, fade, slide, scale, blur } from "svelte/transition";
  import { flip } from "svelte/animate";
  import {
    backIn,
    backOut,
    bounceIn,
    bounceInOut,
    bounceOut,
    circOut,
    quadOut,
    quartOut,
    sineIn,
    sineInOut,
    sineOut,
  } from "svelte/easing";
  const dueDate = "2023-09-14 07:39:00";
  const dueTimestamp = new Date(dueDate);

  let remainingTime = {
    days: 0,
    hours: 0,
    minutes: 0,
    seconds: 0,
  };

  let timeOverMessage =
    "The time to purchase my beautiful product is over! :-(";
  let timeOver = false;

  function resetTimer() {
    remainingTime.days = 0;
    remainingTime.hours = 0;
    remainingTime.minutes = 0;
    remainingTime.seconds = 0;
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
    <div class="card">
      <div class="card-before" />
      {#key remainingTime.days}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.days)}
        </span>
      {/key}
    </div>
  </div>
  <div>
    <p class="card">
      {#key remainingTime.hours}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.hours)}
        </span>
      {/key}
    </p>
  </div>
  <div>
    <p class="card">
      {#key remainingTime.minutes}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.minutes)}
        </span>
      {/key}
    </p>
  </div>
  <div>
    <p class="card">
      {#key remainingTime.seconds}
        <span transition:slide={{ ease: sineInOut }}>
          {formatNumber(remainingTime.seconds)}
        </span>
      {/key}
    </p>
  </div>
</div>
<div class="time-over-message" class:timeOver>{timeOverMessage}</div>

<style>
  .time-over-message {
    opacity: 0;
  }
  .timeOver {
    color: white;
    opacity: 1;
  }

  .clock {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
    place-content: center;
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
    background-color: hsl(var(--crl-darker-blue));
    container-type: inline-size;
    position: relative;

    z-index: 1;
  }

  .card-before {
    position: absolute;
    display: block;
    inset: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
  }

  .card-before::before {
    content: "";
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    opacity: 0.3;
    background-color: blueviolet;
    clip-path: circle(1rem at left);
  }
  .card-before::after {
    content: "";
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    opacity: 0.3;
    background-color: hsl(var(--crl-dark-blue));
    clip-path: circle(1rem at right);
    border: 1px solid blue;
  }

  .card::before {
    content: "";

    background-repeat: no-repeat;
    position: absolute;
    inset: 0;

    opacity: 0.6;
    display: block;
    width: 100%;
    height: 100%;
    z-index: -1;
    border-bottom: 1px solid grey;
  }

  .card::after {
    content: "";

    position: absolute;
    bottom: 0;
    left: 0;
    /* background: radial-gradient(
      circle at 0 50%,
      #232430 1rem,
      var(--_corner-color, transparent) calc(0.5rem + 1px)
    ); */
    background-color: hsl(120, 40%, 40%);
    opacity: 0.6;
    display: block;
    width: 100%;
    height: 50%;
    z-index: -2;
    clip-path: path("M16 0 H204 v1 h0 -188");
  }

  /* .card::after {
    content: "";

    background-repeat: no-repeat;
    position: absolute;
    bottom: 0;
    left: 0;
    background: radial-gradient(
      circle at 0% 0%,
      green 1rem,
      var(--_corner-color, blue) calc(1rem + 2px)
    );
    opacity: 0.6;
    display: block;
    width: 100%;
    height: 50%;
    z-index: -1;
  } */

  /* .deco-up {
    width: 100%;
    height: 50%;

    position: absolute;
    inset: 0;
    z-index: -1;
    border-bottom: 1px solid blueviolet;
  }

  .deco-up::before {
    content: url("images/deco-left.svg");
    position: absolute;
    left: 0;
    bottom: -7px;
  }

  .deco-up::after {
    content: url("images/deco-right.svg");
    position: absolute;
    right: 0;
    bottom: -7px;
  } */
  /* div p:before {
    content: url("/images/number-deco.svg");
    background-color: lightcoral;
    width: 100%;
    height: 50%;
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    z-index: -1;
  } */

  .card span {
    font-size: 40cqi;
    display: flex;
    align-items: center;
    justify-content: center;
    backface-visibility: hidden;
    font-feature-settings: "tnum";
    text-shadow: 2px 2px hsl(var(--crl-dark-blue));
  }
</style>
