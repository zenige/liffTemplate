<template>
  <b-modal
    id="modal-scoped"
    v-model="isModalOpen"
    size="md"
    hide-footer
    modal-class="add-new-word-modal"
    @close="onCancel"
    @hidden="onCancel"
    @ok="onAddNewWord"
  >
    <template v-slot:modal-header="{ close }">
      <!-- Emulate built in modal header close button action -->
      <div class="modal-header bg-white py-2 px-0 mx-3 mt-2">
        <h4 class="modal-title txt_hc_modaltitle mr-4 pr-2">Add a new word</h4>
        <button
          type="button"
          class="close pr-1 pt_15p"
          data-dismiss="modal"
          @click="close()"
        >
          <img
            src="~assets/hc-libs/images/main/ic_close_grey.svg"
            width="37"
            loading="lazy"
          />
        </button>
      </div>
    </template>

    <template v-slot:default>
      <div class="modal-body px-3 px-md-3">
        <form method="post" @submit.prevent="onAddNewWord">
          <div class="d-flex flex-column text-left mb-3">
            <div class="learning-area-title">Question</div>
            <input
              name="question"
              v-model="question"
              type="text"
              class="form-control border-gray border"
            />
          </div>
          <div class="d-flex flex-column text-left">
            <div class="learning-area-title">Answer</div>
            <input
              name="answer"
              v-model="answer"
              type="text"
              class="form-control border-gray border"
            />
          </div>
          <div class="row d-flex justify-content-center">
            <div class="col-12 col-md-4 col-lg-4 mt-3">
              <div class="pt-2">
                <div class="form-group mb-0">
                  <button
                    type="submit"
                    class="btn btn_green_modal h2dot5 btn-block py-2"
                  >
                    Add
                  </button>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </template>
  </b-modal>
</template>

<script>
const ASCENDING = 'ASCENDING'
const DESCENDING = 'DESCENDING'

export default {
  name: 'AddNewWordModal',
  props: {
    isOpen: {
      type: Boolean,
      default: false,
    },
    onCancel: {
      type: Function,
      default: () => {},
    },
    getTrainedWordData: {
      type: Function,
      default: () => {},
    },
    currentPage: {
      type: Number,
      default: 1,
    },
    perPage: {
      type: Number,
      default: 10,
    },
    sortBy: {
      type: String,
      default: null,
    },
    sortDesc: {
      type: Boolean,
      default: false,
    },
    filter: {
      type: String,
      default: null,
    },
  },
  data: () => ({
    isModalOpen: false,
    question: '',
    answer: '',
  }),
  watch: {
    isOpen(newVal) {
      this.isModalOpen = newVal
    },
    isModalOpen(newVal) {
      if (!newVal) {
        this.onModalHide()
      }
    },
  },
  methods: {
    onModalHide() {
      this.$emit('onModalHide', false)
    },
    async onAddNewWord() {
      const body = {
        question: this.question,
        answer: this.answer,
      }
      if (this.question !== '' && this.answer !== '') {
        await this.$axios.post('train/trained', body)
        if (this.sortDesc === false) {
          this.$emit(
            'getTrainedWordData',
            this.filter,
            this.currentPage,
            this.perPage,
            this.sortBy,
            ASCENDING
          )
        } else {
          this.$emit(
            'getTrainedWordData',
            this.filter,
            this.currentPage,
            this.perPage,
            this.sortBy,
            DESCENDING
          )
        }
        this.question = ''
        this.answer = ''
        this.onCancel()
      } else {
        this.$bvToast.toast('Please fill question and answer', {
          variant: 'danger',
          toaster: 'b-toaster-bottom-left',
          noCloseButton: true,
        })
      }
    },
  },
}
</script>

<style lang="scss">
.add-new-word-modal {
  .modal-header {
    width: 100%;
    margin: 0 !important;
  }

  .modal-body {
    padding: 8px 0;
    overflow-y: auto;

    .btn {
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>
