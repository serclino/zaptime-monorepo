<template>
  <div
    class="cal-flex cal-h-full cal-min-h-[524px] cal-items-end cal-justify-center cal-rounded-xl cal-bg-white cal-px-[20px] cal-pb-10 dark:cal-bg-theme-900 sm:cal-px-6"
    :class="[config.compact ? 'cal-w-[330px] sm:cal-w-[400px]' : 'cal-w-[330px] sm:cal-w-[840px]']"
    :style="{ backgroundColor: color2 }"
  >
    <form
      v-if="selectedEvent !== undefined"
      class="cal-mt-6 sm:cal-w-[370px]"
      :class="[selectedEvent.seats > 1 ? 'cal-mt-8' : 'cal-mt-20']"
      @submit.prevent="onSubmit"
    >
      <h1 class="cal-text-2xl cal-font-medium cal-text-theme-500 dark:cal-text-theme-500">
        {{ locale?.confirmationForm?.confirmBooking }}
      </h1>
      <h2 class="cal-mt-[24px] cal-text-2xl cal-font-semibold cal-text-theme-700 dark:cal-text-theme-100">
        {{ getFormattedDay(selectedEvent.start) }}
        {{ getFormattedDayInMonth(selectedEvent.start) }}
      </h2>
      <h3 class="cal-text-[24px] cal-font-semibold cal-leading-[36px] cal-text-theme-500 dark:cal-text-theme-300">
        {{ getFormattedTime(selectedEvent.start) }} -
        {{ getFormattedTime(selectedEvent.end) }}
      </h3>

      <div class="cal-mt-6 cal-max-w-[370px]">
        <CalInput
          v-model="name"
          :label="locale?.confirmationForm?.name?.label"
          :placeholder="locale?.confirmationForm?.name?.placeholder"
          type="text"
          name="name"
          autocomplete="name"
        ></CalInput>
      </div>

      <div class="cal-mt-6 cal-max-w-[370px]">
        <CalInput
          v-model="email"
          :label="locale?.confirmationForm?.email?.label"
          :placeholder="locale?.confirmationForm?.email?.placeholder"
          type="email"
          name="email"
          autocomplete="email"
        ></CalInput>
      </div>

      <div
        v-if="selectedEvent.seats > 1"
        class="cal-mt-6 cal-max-w-[370px]"
      >
        <label
          for="email"
          class="cal-block cal-text-sm cal-font-medium cal-text-theme-700 dark:cal-text-theme-200"
        >
          {{ locale?.confirmationForm?.seats?.label }}</label
        >
        <div class="cal-mt-2">
          <input
            v-model="seats"
            type="number"
            :min="1"
            :max="selectedEvent.seats"
            name="seats"
            autocomplete="off"
            class="dark:cal-bg-theme-150 cal-block cal-w-full cal-rounded-md cal-border-2 cal-border-theme-800 cal-bg-theme-50 cal-px-5 cal-py-3.5 cal-text-base cal-font-medium cal-text-theme-100 cal-placeholder-theme-800 focus:cal-border-theme-500 focus:cal-outline-none focus:cal-ring-theme-500 sm:cal-text-sm"
            :placeholder="locale?.confirmationForm?.seats?.placeholder"
          />
        </div>
      </div>

      <div class="cal-mt-[32px] cal-flex cal-justify-between">
        <SecondaryButton @click="$emit('go-back')">
          {{ locale?.confirmationForm?.buttons?.goBack }}
        </SecondaryButton>
        <PrimaryButton
          :loading="disabled"
          type="submit"
        >
          {{ locale?.confirmationForm?.buttons?.confirmBooking }}
        </PrimaryButton>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { computed, inject, ref } from 'vue';
import { useSelectedEvent, book, useConfig, useDateFormatters } from '@zaptime/core';
import PrimaryButton from './atomic/PrimaryButton.vue';
import SecondaryButton from './atomic/SecondaryButton.vue';
import CalInput from './DefaultCalendar/CalInput.vue';

const emits = defineEmits(['booking-confirmed', 'go-back']);

const { selectedEvent } = useSelectedEvent(inject('calendarId'));
const { getFormattedTime, getFormattedDay, getFormattedDayInMonth } = useDateFormatters(inject('calendarId'));
const { config } = useConfig(inject('calendarId'));
const color2 = inject<string>('color2');
const calendarId = inject<string>('calendarId');

const email = ref('');
const name = ref('');
const seats = ref(1);

const disabled = ref(false);

const locale = computed(() => {
  if (config === undefined) {
    return;
  }
  return config.value.locale;
});

const splitName = (name: string) => {
  let firstName = '';
  let lastName = '';

  if (name !== '') {
    const splitName = name.split(' ');

    if (splitName.length > 1) {
      firstName = splitName[0];
      lastName = splitName[1];
    } else {
      firstName = name;
    }
  }

  return {
    firstName,
    lastName,
  };
};

const onSubmit = async () => {
  disabled.value = true;

  const { firstName, lastName } = splitName(name.value);
  await book(email.value, firstName, lastName, seats.value, calendarId);
  emits('booking-confirmed');

  disabled.value = false;
};
</script>
