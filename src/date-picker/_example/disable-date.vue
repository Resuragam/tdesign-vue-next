<template>
  <t-space direction="vertical">
    <t-date-picker
      placeholder="禁用昨天、前天"
      :disable-date="[dayjs().subtract(1, 'day').format(), dayjs().subtract(2, 'day').format()]"
    />
    <t-date-picker
      placeholder="明后三天禁用"
      :disable-date="{
        from: dayjs().add(1, 'day').format(),
        to: dayjs().add(3, 'day').format(),
      }"
    />
    <t-date-picker placeholder="禁用所有周六" :disable-date="(date) => dayjs(date).day() === 6" />
    <t-date-picker
      placeholder="禁用最近 3 天外的日期"
      :disable-date="{
        before: dayjs().subtract(3, 'day').format(),
        after: dayjs().add(3, 'day').format(),
      }"
    />
    <t-date-picker
      placeholder="禁用日期精确到时间"
      enable-time-picker
      :disable-date="{ before: dayjs().subtract(1, 'day').format() }"
      :time-picker-props="timePickerProps"
      @pick="onPick"
    />
    <t-date-range-picker
      placeholder="禁用最近 5 天外的日期"
      :disable-date="{
        before: dayjs().subtract(5, 'day').format(),
        after: dayjs().add(5, 'day').format(),
      }"
    />
  </t-space>
</template>

<script setup>
import { ref, computed } from 'vue';
import dayjs from 'dayjs';

const pickDate = ref();

const timePickerProps = computed(() => {
  return {
    disableTime: () => {
      if (pickDate.value === dayjs().format('YYYY-MM-DD')) {
        return {
          hour: [0, 1, 2, 3, 4, 5, 6],
        };
      }
      return {};
    },
  };
});

const onPick = (date) => {
  pickDate.value = dayjs(date).format('YYYY-MM-DD');
};
</script>
