<template>
  <div
    style="background-color: black; height: 100vh"
    class="cal-flex cal-px-4"
  >
    <div style="padding-top: 20px; padding-left: 20px">
      <ZaptimeCalendar
        calendarId="calendar-1"
        :config="config"
      ></ZaptimeCalendar>
    </div>

    <!-- <ZaptimeCalendar
      :config="config2"
      calendarId="calendar-2"
    ></ZaptimeCalendar>
    <ZaptimeCalendar
      :config="config3"
      calendarId="calendar-3"
    ></ZaptimeCalendar> -->
  </div>
</template>

<script setup lang="ts">
import { default as ZaptimeCalendar } from '../src/App.vue';
import { IZapTimeConfig } from '@zaptime/core';

const calypsoTheme: IZapTimeConfig['theme'] = {
  mode: 'dark',
  colors: {
    'accent--1': '#9e7700',
    'accent-0': '#ffc000',
    'accent-1': '#ffcc31',
    // 100: '#CBCBCD',
    // 200: '#828488',
    // 300: '#575B62',
    // 400: '#494D55',
    // 500: '#3C4048',
    // 600: '#32363E',
    // 700: '#272A30',
    // 800: '#272A30',
    // 900: '#13171E',
  },
};

const config: IZapTimeConfig = {
  token: 'kPRrAtlXbgnCqi309Zjvr5Lq6RRPfYgc',

  compact: false,

  externalBooking: false,

  // min: 0,
  // max: 1,
  // externalBooking: false,
  // compact: true,

  profileImage: 'https://media.licdn.com/dms/image/D4E03AQE3gawFsTv2rA/profile-displayphoto-shrink_800_800/0/1674467277976?e=1687996800&v=beta&t=cuvWY8CocEGn1fFSpncV6PE5aZBRWoPaXXL_atHYf34',

  locale: {
    preset: 'en',

    hideTimePreferences: false,
    startDayOfWeek: 'sun',

    texts: {
      introduction: 'Book a Topol walktrough',
    },
    confirmationForm: {
      confirmBooking: 'Confirm booking',
      name: {
        label: 'Your name',
        placeholder: 'Enter your name',
      },

      email: {
        label: 'Your email',
        placeholder: 'Enter your email',
      },
      seats: {
        label: 'Number of seats',
        placeholder: 'Enter number of seats',
      },
      buttons: {
        confirmBooking: 'Confirm booking',
        goBack: 'Go back',
      },
    },
  },
  theme: calypsoTheme,
};
</script>

<style>
body {
  margin: 0;
}
</style>
