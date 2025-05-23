<script setup>
import { computed } from "vue";
import {
  SelectItem,
  SelectItemIndicator,
  SelectItemText,
  useForwardProps,
} from "radix-vue";
import { Check } from "lucide-vue-next";
import { cn } from "@/lib/utils";

const props = defineProps({
  value: { type: [String, Object], required: true },
  disabled: { type: Boolean, required: false },
  textValue: { type: String, required: false },
  asChild: { type: Boolean, required: false },
  as: { type: null, required: false },
  class: { type: null, required: false },
  isCheckIcon: { typed: Boolean, required: false, default: true }
});

const delegatedProps = computed(() => {
  const { class: _, ...delegated } = props;

  return delegated;
});

const forwardedProps = useForwardProps(delegatedProps);
</script>

<template>
  <SelectItem v-bind="forwardedProps" :class="cn(
    'select-item relative flex w-full justify-between cursor-pointer select-none items-center px-[1.1875rem] py-3 text-base font-medium text-[var(--gray-700)] hover:text-[var(--primary-500)] hover:bg-activeBG outline-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 dark:focus:bg-slate-800 dark:focus:text-slate-50',
    props.class
  )">
    <SelectItemText>
      <slot />
    </SelectItemText>
    <span class="flex h-3.5 w-3.5 items-center justify-center ml-2 sm:ml-0" v-if="isCheckIcon">
      <SelectItemIndicator class="item-indicator flex">
        <Check class="h-4 w-4" />
      </SelectItemIndicator>
    </span>
  </SelectItem>
</template>

<style lang="scss">
.select-item {
  &:hover {
    & .select-icon {
      & svg {
        fill: var(--primary-500);
      }
    }

    & .item-indicator {
      &>svg {
        stroke: var(--primary-500);
      }
    }
  }
}

.footer-select{
  width: 120px;
  &:hover {
    & .select-icon {
      & svg {
        fill: var(--gray-700);
      }
    }

    & .item-indicator {
      &>svg {
        stroke: var(--gray-700);
      }
    }
  }
}
.active_select_item {
  background-color: #E9F1FF;
  color: var(--primary-500);

  & .select-icon {
    & svg {
      fill: var(--primary-500);
    }
  }
}
</style>
