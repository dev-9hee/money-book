<template>
  <div v-if="isOpen" class="cmodal" @click.self="closeModal">
    <div class="modal-content">
      <div class="category-selection">
        <button
          style="border-style: none"
          :class="{ active: isIncome }"
          @click.prevent="selectIncome"
        >
          수입
        </button>
        <div class="categorysize">카테고리</div>
        <button
          style="border-style: none"
          :class="{ active: !isIncome }"
          @click.prevent="selectExpense"
        >
          지출
        </button>
      </div>

      <div class="tag-options">
        <div v-if="isIncome" class="grid">
          <div
            v-for="tag in incomeTags"
            :key="tag.id"
            class="tag-option"
            @click="selectTag(tag.name, tag.image)"
          >
            <img :src="tag.image" alt="x" class="category-img" />
            <span>{{ tag.name }}</span>
          </div>
        </div>

        <div v-else class="grid">
          <div
            v-for="tag in expenseTags"
            :key="tag.id"
            class="tag-option"
            @click="selectTag(tag.name, tag.image)"
          >
            <img :src="tag.image" alt="x" class="category-img" />
            {{ tag.name }}
          </div>
        </div>
      </div>
      <div>
        <button class="modalClose" @click="close">닫기</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import EventBus from '../eventBus';
import '@/asset/categorymodal.css';
const props = defineProps({
  isOpen: Boolean,
});
const emit = defineEmits(['close']);
const incomeTags = ref([
  { id: 1, name: '월급', image: 'src/icons/monthpay.png' },
  { id: 2, name: '알바비', image: 'src/icons/alba.png' },
  { id: 3, name: '당첨금', image: 'src/icons/lotto.png' },
  { id: 4, name: '성과금', image: 'src/icons/good.png' },
  { id: 5, name: '퇴직금', image: 'src/icons/home.png' },
  { id: 6, name: '잡소득', image: 'src/icons/money.png' },
]);

const expenseTags = ref([
  { id: 1, name: '데이트', image: 'src/icons/date.png' },
  { id: 2, name: '통신', image: 'src/icons/phone.png' },
  { id: 3, name: '선물', image: 'src/icons/present.png' },
  { id: 4, name: '공과금', image: 'src/icons/bill.png' },
  { id: 5, name: '카드', image: 'src/icons/card.png' },
  { id: 6, name: '카페', image: 'src/icons/cafe.png' },
  { id: 7, name: '교통', image: 'src/icons/transport.png' },
  { id: 8, name: '여행', image: 'src/icons/travel.png' },
  { id: 9, name: '쇼핑', image: 'src/icons/fashion.png' },
]);

const isIncome = ref(true);

const selectIncome = () => {
  isIncome.value = true;
};

const selectExpense = () => {
  isIncome.value = false;
};

const selectTag = (tagName, tagImg) => {
  EventBus.emit('tagSelected', { tagName, isIncome: isIncome.value, tagImg });
  emit('close');
};

const close = () => {
  emit('close');
};
</script>
