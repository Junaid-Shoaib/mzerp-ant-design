<template>
  <app-layout>
    <template #header>
      <div class="grid grid-cols-2">
        <h2 class="font-semibold text-lg text-white p-4">Reports</h2>
        <div class="justify-end">
          <!-- <select
            v-model="yr_id"
            class="
              pr-2
              ml-2
              pb-2
              text-gray-700
              w-full
              lg:w-5/12
              rounded-md
              float-right
            "
            label="year"
            @change="yrch"
          >
            <option v-for="type in years" :key="type.id" :value="type.id">
              {{ type.name }}
            </option>
          </select> -->
          <Select
            v-model:value="selected_year"
            :options="years"
            :field-names="{ label: 'end', value: 'id' }"
            filterOption="true"
            optionFilterProp="name"
            mode="single"
            placeholder="Please select"
            showArrow
            @change="yrch"
            class="w-1/2"
          />
          <Select
            v-model:value="selected"
            :options="options"
            :field-names="{ label: 'name', value: 'id' }"
            filterOption="true"
            optionFilterProp="name"
            mode="single"
            placeholder="Please select"
            showArrow
            @change="coch"
            class="w-1/2"
          />
        </div>
      </div>
    </template>
    <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-4">
    <a-row>
      <div v-if="$page.props.flash.success" class="bg-green-600 text-white">
        {{ $page.props.flash.success }}
      </div>
      <!-- <jet-button @click="create" class="mt-4 ml-8">Create</jet-button> -->

      <!-- <div v-if="errors.type">{{ errors.type }}</div> -->
    <a-col :span="12">
        <form
            target="_blank"
            @submit.prevent="submit_trial_range"
            v-bind:action="'trialbalance'"
            ref="form_trial_range"
            >
            <a-input
            :min="form.start"
            :max="form.end"
            v-model="form.date"
            style="width:70%"
            type="date"
            label="date"
            placeholder="Enter Begin date:"
            class="pr-2 ml-2 pb-2 rounded-md"
            name="date"
            required
            />
            <!-- <div
                class="
                border
                rounded-lg
                shadow-md
                p-1
                px-4
                m-2
                bg-gray-800
            text-white
            inline-block
            hover:bg-gray-700 hover:text-white
            "
            >
            <a href="trialbalance" target="_blank">Trial Balance</a>
        </div> -->
        <!-- <div
            class="
            border
            rounded-lg
            shadow-md
            p-1
            px-4
            mt-1
            bg-gray-800
            text-white
            ml-2
            inline-block
            hover:bg-gray-700 hover:text-white
            "
            > -->
               <Button type="primary" :disabled="form.processing" htmlType="submit">Trial Balance</Button>
            <!-- </div> -->
        </form>
    </a-col>
    <a-col :span="12">
      <form
        target="_blank"
        @submit.prevent="submit_bs_range"
        v-bind:action="'bs'"
        ref="form_bs_range"
        class="inline-block"
      >
        <a-input
          :min="form.start"
          :max="form.end"
          v-model:value="form.date"
          type="date"
          label="date"
          placeholder="Enter Begin date:"
          class="ml-2"
          name="date"
          hidden
          required
        />
        <!-- <div
        class="
          border
          rounded-lg
          shadow-md
          p-1
          px-4
          m-2
          bg-gray-800
          text-white
          inline-block
          hover:bg-gray-700 hover:text-white
        "
      >
        <a href="bs" target="_blank">Balance Sheet</a>
      </div> -->
        <!-- <div
          class="
            border
            rounded-lg
            shadow-md
            p-1
            px-4
            mt-1
            bg-gray-800
            text-white
            ml-2
            inline-block
            hover:bg-gray-700 hover:text-white
          "
        > -->
        <Button type="primary" :disabled="form.processing" htmlType="submit">Balance Sheete</Button>

        <!-- <Button type="submit">Balance Sheet</Button> -->
        <!-- </div> -->
      </form>

      <form
        target="_blank"
        @submit.prevent="submit_pl_range"
        v-bind:action="'pl'"
        ref="form_pl_range"
        class="inline-block ml-8"
      >
        <a-input
          :min="form.start"
          :max="form.end"
          v-model:value="form.date"
          type="date"
          label="date"
          placeholder="Enter Begin date:"
          class="pr-2 ml-2 pb-2 rounded-md"
          name="date"
          hidden
          required
        />
        <!-- <div
        class="
          border
          rounded-lg
          shadow-md
          bg-gray-800
          text-white
          p-1
          px-4
          m-2
          inline-block
          hover:bg-gray-700 hover:text-white
        "
      >
        <a href="pl" target="_blank">Profit or Loss A/C</a>
      </div> -->
        <!-- <div
          class="
            border
            rounded-lg
            shadow-md
            p-1
            px-4
            mt-1
            bg-gray-800
            text-white
            ml-2
            inline-block
            hover:bg-gray-700 hover:text-white
          "
        > -->


            <Button type="primary" :disabled="form.processing" htmlType="submit">Profit or Loss A/C</Button>
        <!-- </div> -->
      </form>
    </a-col>
      </a-row>
    </div>
  </app-layout>
</template>

<script>
import AppLayout from "@/Layouts/AppLayout";
import JetButton from "@/Jetstream/Button";
import { useForm } from "@inertiajs/inertia-vue3";
import Multiselect from "@suadelabs/vue3-multiselect";
import {
    Table,
    Select,
    InputSearch,
    Form,
    Input,
    Button,
    Row,
    Col,
} from "ant-design-vue";

export default {
  components: {
    AppLayout,
    Button,
    Table,
    Select,
    InputSearch,
    JetButton,
    Multiselect,
        "a-form": Form,
        "a-form-item": Form.Item,
        "a-input": Input,
        "a-textarea": Input.TextArea,
        "a-button": Button,
        "a-select": Select,
        "a-row": Row,
           "a-col"    : Col,
    },

  props: {
    errors: Object,
    data: Object,
    companies: Object,
    company: Object,
    accounts: Object,
    account_first: Object,
    years: Object,
    year: Object,
    min_start: Object,
    max_end: Object,
  },

  data() {
    return {
      co_id: this.company,
      options: this.companies,
      selected: this.company.name,
      yr_id: this.$page.props.yr_id,
      years: this.years,
      selected_year: this.year.name,

      form: {
        date: this.date ? this.date : this.max_end,
        start: this.min_start
          ? this.min_start
          : new Date().toISOString().substr(0, 10),
        end: this.max_end
          ? this.max_end
          : new Date().toISOString().substr(0, 10),
      },
      //   form: this.$inertia.form({
      //     account_id: this.account_first.id,
      //     date_start: null,
      //     date_end: null,
      //   }),

      //   form: this.$refs.form({
      //     account_id: this.account_first.id,
      //     date_start: "null",
      //     date_end: null,
      //   }),

      //   form: {
      //     account_id: this.account_first.id,
      //     date_start: null,
      //     date_end: null,
      //     // begin: null,
      //     // end: null,
      //   },
    };
  },
  //   setup(props) {
  //     const form = useForm({
  //       account_id: props.account_first.id,
  //       date_start: null,
  //       date_end: "",
  //     });
  //     return { form };
  //   },

  methods: {
    submit_trial_range: function () {
      this.$refs.form_trial_range.submit();
    },
    submit_bs_range: function () {
      this.$refs.form_bs_range.submit();
    },
    submit_pl_range: function () {
      this.$refs.form_pl_range.submit();
    },
    meth() {
      this.form.get(route("range"));
    },
    //TO GENERATE AN PDF WITH BUTTON
    submit: function () {
      this.$refs.form.submit();
    },
    // submit() {
    //   //   entries = this.entries;
    //   //   if (this.difference === 0) {
    //   this.form.post(route("range"));
    //   //   this.$inertia.get(route("range"), this.form);
    //   //   } else {
    //   //     alert("Entry is not equal");
    //   //   }
    // },

    create() {
      this.$inertia.get(route("years.create"));
    },

    // route() {
    //   this.$inertia.post(route("range"), this.form);
    //   //   this.$inertia.get(route("range"));
    // },

    route() {
      // this.$inertia.post(route("companies.store"), this.form);
      this.$inertia.get(route("pd"));
    },

    route() {
      // this.$inertia.post(route("companies.store"), this.form);
      this.$inertia.get(route("trialbalance"));
    },

    edit(id) {
      this.$inertia.get(route("years.edit", id));
    },

    destroy(id) {
      this.$inertia.delete(route("years.destroy", id));
    },
    coch(value) {
      this.$inertia.get(route("companies.coch", value));
    },

    yrch() {
      this.$inertia.get(route("years.yrch", this.yr_id));
    },
  },
};
</script>
