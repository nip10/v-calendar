<template>
  <div class="section">
    <h2>Calendar Attributes</h2>
    <div class="bg-gray-400 pt-10 pb-20 rounded-3xl mb-5">
      <div class="font-medium text-3xl mb-6">Dev test</div>
      <v-calendar view="weekly" ref="cal" @dayclick="onDayClick"/>
    </div>
  </div>
</template>

<script>
const PopoverRow = require('@/components/PopoverRow').default;
const Locale = require('@/utils/locale').default;
const locale = new Locale();

export default {
  components: {
    PopoverRow,
  },
  data() {
    return {
      pageForThisMonth: null,
      pageForNextMonth: null,
      incId: 5,
      editId: 0,
      todos: [],
    };
  },
  computed: {
    thisMonth() {
      return this.pageForThisMonth.month - 1;
    },
    thisMonthYear() {
      return this.pageForThisMonth.year;
    },
    nextMonth() {
      return this.pageForNextMonth.month - 1;
    },
    nextMonthYear() {
      return this.pageForNextMonth.year;
    },
    highlights() {
      return [
        {
          highlight: 'red',
          contentStyle: {
            color: 'white',
          },
          dates: [
            // Use single dates
            new Date(this.nextMonthYear, this.nextMonth, 6),
            new Date(this.nextMonthYear, this.nextMonth, 23),
            // ...or date ranges
            {
              start: new Date(this.thisMonthYear, this.thisMonth, 2),
              end: new Date(this.thisMonthYear, this.thisMonth, 4),
            },
            // ...or complex date patterns
            {
              start: new Date(this.thisMonthYear, this.thisMonth, 1),
              ordinalWeekdays: { [-1]: 7 }, // Last Saturday of the month
            },
          ],
        },
        {
          highlight: 'blue',
          contentStyle: {
            color: 'white',
          },
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 1),
            {
              start: new Date(this.thisMonthYear, this.thisMonth, 10),
              end: new Date(this.thisMonthYear, this.thisMonth, 12),
            },
            {
              start: new Date(this.nextMonthYear, this.nextMonth, 22),
              end: new Date(this.nextMonthYear, this.nextMonth, 26),
            },
          ],
        },
        {
          highlight: 'teal',
          contentStyle: {
            color: 'white',
          },
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 14),
            {
              start: new Date(this.thisMonthYear, this.thisMonth, 24),
              end: new Date(this.thisMonthYear, this.thisMonth, 25),
            },
            new Date(this.thisMonthYear, this.thisMonth, 28),
            new Date(this.nextMonthYear, this.nextMonth, 4),
            {
              start: new Date(this.nextMonthYear, this.nextMonth, 16),
              end: new Date(this.nextMonthYear, this.nextMonth, 17),
            },
          ],
        },
      ];
    },
    dots() {
      return [
        {
          dot: 'red',
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 1),
            new Date(this.thisMonthYear, this.thisMonth, 10),
            new Date(this.thisMonthYear, this.thisMonth, 22),
            new Date(this.nextMonthYear, this.nextMonth, 6),
            new Date(this.nextMonthYear, this.nextMonth, 16),
          ],
        },
        {
          dot: 'teal',
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 4),
            new Date(this.thisMonthYear, this.thisMonth, 10),
            new Date(this.thisMonthYear, this.thisMonth, 15),
            new Date(this.nextMonthYear, this.nextMonth, 1),
            new Date(this.nextMonthYear, this.nextMonth, 12),
            {
              start: new Date(this.nextMonthYear, this.nextMonth, 20),
              end: new Date(this.nextMonthYear, this.nextMonth, 25),
            },
          ],
        },
        {
          dot: 'blue',
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 12),
            new Date(this.thisMonthYear, this.thisMonth, 26),
            new Date(this.thisMonthYear, this.thisMonth, 15),
            new Date(this.nextMonthYear, this.nextMonth, 9),
            new Date(this.nextMonthYear, this.nextMonth, 5),
            new Date(this.nextMonthYear, this.nextMonth, 6),
            new Date(this.nextMonthYear, this.nextMonth, 20),
            new Date(this.nextMonthYear, this.nextMonth, 25),
          ],
        },
      ];
    },
    bars() {
      return [
        {
          bar: 'red',
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 1),
            new Date(this.thisMonthYear, this.thisMonth, 10),
            new Date(this.thisMonthYear, this.thisMonth, 22),
            new Date(this.nextMonthYear, this.nextMonth, 6),
            new Date(this.nextMonthYear, this.nextMonth, 16),
          ],
        },
        {
          bar: 'teal',
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 4),
            new Date(this.thisMonthYear, this.thisMonth, 10),
            new Date(this.thisMonthYear, this.thisMonth, 15),
            new Date(this.nextMonthYear, this.nextMonth, 1),
            new Date(this.nextMonthYear, this.nextMonth, 12),
            {
              start: new Date(this.nextMonthYear, this.nextMonth, 20),
              end: new Date(this.nextMonthYear, this.nextMonth, 25),
            },
          ],
        },
        {
          bar: 'blue',
          dates: [
            new Date(this.thisMonthYear, this.thisMonth, 12),
            new Date(this.thisMonthYear, this.thisMonth, 26),
            new Date(this.thisMonthYear, this.thisMonth, 15),
            new Date(this.nextMonthYear, this.nextMonth, 9),
            new Date(this.nextMonthYear, this.nextMonth, 5),
            new Date(this.nextMonthYear, this.nextMonth, 6),
            new Date(this.nextMonthYear, this.nextMonth, 20),
            new Date(this.nextMonthYear, this.nextMonth, 25),
          ],
        },
      ];
    },
    popovers() {
      return [
        // Todo attributes
        ...this.todos.map(todo => ({
          key: todo.id,
          dates: todo.dates,
          customData: todo,
          order: todo.id,
          dot: {
            color: todo.color,
            class: todo.isComplete ? 'opacity-25' : '',
          },
          popover: {
            visibility: 'click',
          },
        })),
        // 'Add todo' attribute
        {
          key: 'add-todo',
          dates: {},
          popover: true,
        },
      ];
    },
  },
  created() {
    this.refreshMonthData();
  },
  mounted() {
    this.refreshTodos();
  },
  methods: {
    onDayClick(day) {
      console.log("day clicked", day)
    },
    refreshMonthData() {
      const { month, year } = locale.getThisMonthComps();
      this.pageForThisMonth = { month, year };
      this.pageForNextMonth = locale.getNextMonthComps(month, year);
    },
    refreshTodos() {
      this.todos = [
        {
          id: 1,
          description: 'Take Noah to basketball practice.',
          isComplete: false,
          dates: new Date(this.thisMonthYear, this.thisMonth, 1),
          color: 'blue',
        },
        {
          id: 2,
          description: 'Get some milks.',
          isComplete: false,
          dates: new Date(this.thisMonthYear, this.thisMonth, 5),
          color: 'red',
        },
        {
          id: 3,
          description: 'Pay the utility bill.',
          isComplete: false,
          dates: new Date(this.thisMonthYear, this.thisMonth, 19),
          color: 'orange',
        },
        {
          id: 4,
          description: 'Pick up clothes from the cleaners.',
          isComplete: false,
          dates: new Date(this.thisMonthYear, this.thisMonth, 19),
          color: 'purple',
        },
        {
          id: 5,
          description: 'Lunch with Leo.',
          isComplete: false,
          dates: new Date(this.thisMonthYear, this.thisMonth, 22),
          color: 'green',
        },
      ];
    },
    addTodo(day) {
      this.editId = ++this.incId;
      this.todos.push({
        id: this.editId,
        description: 'New todo',
        isComplete: false,
        dates: day.date,
      });
    },
    toggleTodoComplete(todo) {
      todo.isComplete = !todo.isComplete;
    },
    toggleTodoEdit(todo, updateLayout) {
      this.editId = this.editId === todo.id ? 0 : todo.id;
      this.$nextTick(() => updateLayout());
    },
    deleteTodo(todo, updateLayout) {
      this.todos = this.todos.filter(t => t !== todo);
    },
  },
  directives: {
    focusSelect: {
      inserted(el) {
        el.focus();
        el.select();
      },
    },
  },
};
</script>
