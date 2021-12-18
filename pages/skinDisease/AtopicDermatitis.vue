<template>
  <div>
    <Loader v-if="isLoading" />
    <div v-else>
      <div class="container container_short pg-bar">
        <div class="text-center">
          <b-progress
            :value="ans.length"
            :max="quiz.questions.length"
          ></b-progress>
        </div>
      </div>
      <div class="container container_short skin-content">
        <div class="row">
          <div class="col-12 mb-2 pb_me-4">
          <div>Atopic Dermatitis</div>
            <div class="pt-3">
              <div class="card-body">
                <div class="col-12 mt-3 mb-2 px-0">
                  <div class="ck-content">
                    <p class="question_txt">
                      {{ quiz.questions[questionIndex - 1].text }}
                    </p>
                    <figure
                      class="image mt-3"
                      v-if="quiz.questions[questionIndex - 1].questionImage"
                    >
                      <img
                        v-bind:src="
                          quiz.questions[questionIndex - 1].questionImage
                        "
                      />
                    </figure>
                  </div>
                </div>
                <div class="mt-4 px-0 mx-0">
                  <div
                    style="cursor: pointer"
                    class="col-12"
                    v-for="(item, index) in quiz.questions[questionIndex - 1]
                      .answers"
                    :key="index"
                  >
                    <a
                      v-if="
                        index === quiz.questions[questionIndex - 1].checkpoint
                      "
                    >
                      <div
                        class="card bg_green"
                        @click="checkpointbg(index, item.value)"
                      >
                        <div class="card-body">
                          <div class="col-12 d-flex align-items-center">
                            <div class="answer_txt" style="color: #fff">
                              {{ item.text }}
                            </div>
                          </div>
                        </div>
                      </div>
                    </a>
                    <a
                      v-if="
                        index !== quiz.questions[questionIndex - 1].checkpoint
                      "
                    >
                      <div
                        class="card card-list-hover"
                        @click="checkpointbg(index, item.value)"
                      >
                        <div class="card-body">
                          <div class="col-12 d-flex align-items-center">
                            <div class="answer_txt">{{ item.text }}</div>
                          </div>
                        </div>
                      </div>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="end === false">
        <div
          class="container container_short footerV2"
          v-if="quiz.questions[questionIndex - 1].check === false"
          disabled
        >
          <div class="row mt-3 mb-3">
            <div
              class="col-12 d-flex align-items-center justify-content-center"
            >
              <div class="m_width_120p">
                <div class="m_width_120p text-center footerBtn">ต่อไป</div>
              </div>
            </div>
          </div>
        </div>
        <div
          class="container container_short footer"
          v-if="quiz.questions[questionIndex - 1].check === true"
          @click="next"
        >
          <div class="row mt-3 mb-3">
            <div
              class="col-12 d-flex align-items-center justify-content-center"
            >
              <div class="m_width_120p">
                <div class="m_width_120p text-center footerBtn">ต่อไป</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="end === true">
        <div
          class="container container_short footerV2"
          v-if="quiz.questions[questionIndex - 1].check === false"
          disabled
        >
          <div class="row mt-3 mb-3">
            <div
              class="col-12 d-flex align-items-center justify-content-center"
            >
              <div class="m_width_120p">
                <div class="m_width_120p text-center footerBtn">
                  ส่งแบบสอบถาม
                </div>
              </div>
            </div>
          </div>
        </div>
        <div
          class="container container_short footer"
          v-if="quiz.questions[questionIndex - 1].check === true"
          @click="math"
        >
          <div class="row mt-3 mb-3">
            <div
              class="col-12 d-flex align-items-center justify-content-center"
            >
              <div class="m_width_120p">
                <div class="m_width_120p text-center footerBtn">
                  ส่งแบบสอบถาม
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
var quiz = {
  title: 'quiz sample',
  questions: [
    {
      text: '1.) คุณเป็นผื่นที่บริเวณ ศีรษะ/คอ คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/QJFL59y/1.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 2.25,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 4.5,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 6.75,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 9.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '2.) คุณเป็นผื่นที่บริเวณ แขนซ้าย คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/S5gLdWp/2.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 2.25,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 4.5,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 6.75,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 9.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '3.) คุณเป็นผื่นที่บริเวณ แขนขวา คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/jZjStxm/3.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 2.25,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 4.5,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 6.75,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 9.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '4.) คุณเป็นผื่นที่บริเวณ ขาซ้าย คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/5jTHMfJ/4.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 4.5,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 9.0,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 13.5,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 18.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '5.) คุณเป็นผื่นที่บริเวณ ขาขวา คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/80GY2MH/5.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 4.5,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 9.0,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 13.5,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 18.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '6.) คุณเป็นผื่นที่บริเวณ ลำตัวด้านหน้า คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/tC8Wqc9/6.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 4.5,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 9.0,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 13.5,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 18.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '7.) คุณเป็นผื่นที่บริเวณ แผ่นหลัง คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/b3KPgQK/7.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 4.5,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 9.0,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 13.5,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 18.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '8.) คุณเป็นผื่นที่บริเวณ อวัยเพศ คิดเป็นสัดส่วนประมาณกี่เปอร์เซ็นต์ของพื้นที่ในบริเวณนี้ทั้งหมด?',
      questionImage: 'https://i.ibb.co/p15m1WN/8.png',
      answers: [
        {
          text: '0% (ไม่เป็นผื่น)',
          value: 0,
        },
        {
          text: 'ประมาณ 1-25% ',
          value: 0.25,
        },
        {
          text: 'ประมาณ 26-50%',
          value: 0.5,
        },
        {
          text: 'ประมาณ 51-75%',
          value: 0.75,
        },
        {
          text: 'ประมาณ 76-100%',
          value: 1.0,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '9.) บริเวณที่คุณเป็นผื่นนั้น มีความเป็นผื่นแดงมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/JKs6fkR/redness.png',
      answers: [
        {
          text: 'None (ไม่เป็นผื่นแดง)',
          value: 0,
        },
        {
          text: 'Mild (เล็กน้อย บาง ๆ หรือมีสีชมพู)',
          value: 1,
        },
        {
          text: 'Moderate (ปานกลาง มีสีแดงหม่น ๆ ที่มองเห็นได้ชัด)',
          value: 2,
        },
        {
          text: 'Severe (มาก มีสีแดงเข้ม มองเห็นได้ชัดเจน)',
          value: 3,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '10.) บริเวณที่คุณเป็นผื่นนั้น เป็นตุ่มหรือมีรอยบวมนูนของผื่นมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/4R9cCJ7/edema.png',
      answers: [
        {
          text: 'None (ไม่เป็นตุ่มหรือมีรอยบวมนูน)',
          value: 0,
        },
        {
          text: 'Mild (เล็กน้อย แทบมองไม่เห็น)',
          value: 1,
        },
        {
          text: 'Moderate (ปานกลาง มองเห็นได้ชัด แต่ไม่เด่นชัดมาก)',
          value: 2,
        },
        {
          text: 'Severe (มองเห็นได้เด่นชัดเจนมาก)',
          value: 3,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '11.) บริเวณที่คุณเป็นผื่นนั้น มีสะเก็ดเลือดซึมมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/7X4GwF3/crusting.png',
      answers: [
        {
          text: 'Score 0 (ไม่มีสะเก็ดเลือดซึม)',
          value: 0,
        },
        {
          text: 'Score 1 (เล็กน้อย แทบมองไม่เห็น)',
          value: 1,
        },
        {
          text: 'Score 2 (ปานกลาง มองเห็นได้ชัด แต่ไม่เด่นชัดมาก)',
          value: 2,
        },
        {
          text: 'Score 3 (มาก มองเห็นได้เด่นชัดเจนมาก)',
          value: 3,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '12.) บริเวณที่คุณเป็นผื่นนั้น เป็นรอยถลอกหรือรอยผิวหนังที่แตกเป็นแผลของผื่นมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/LzGy9Hw/excoriation.png',
      answers: [
        {
          text: 'None (ไม่เป็นรอยถลอกหรือมีรอยผิวหนังที่แตก)',
          value: 0,
        },
        {
          text: 'Mild (เล็กน้อย มีไม่กี่จุด มองเห็นแค่ผิวเผิน)',
          value: 1,
        },
        {
          text: 'Moderate (ปานกลาง มีหลายจุด มองเห็นได้ชัดและ/หรือมีความลึกของแผลเล็กน้อย)',
          value: 2,
        },
        {
          text: 'Severe (มาก มีหลายจุดอย่างกว้างขวาง มองเห็นได้ชัดและ/หรือมีความลึกของแผลมาก)',
          value: 3,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '13.) บริเวณที่คุณเป็นผื่นนั้น มีความหนาหรือความแข็งตัวของผิวหนังมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/ncppvB1/lichenification.png',
      answers: [
        {
          text: 'None (ไม่เป็นรอยถลอกหรือมีรอยผิวหนังที่แตก)',
          value: 0,
        },
        {
          text: 'Mild (เล็กน้อย มีไม่กี่จุด มองเห็นแค่ผิวเผิน)',
          value: 1,
        },
        {
          text: 'Moderate (ปานกลาง มีหลายจุด มองเห็นได้ชัดและ/หรือมีความลึกของแผลเล็กน้อย)',
          value: 2,
        },
        {
          text: 'Severe (มาก มีหลายจุดอย่างกว้างขวาง มองเห็นได้ชัดและ/หรือมีความลึกของแผลมาก)',
          value: 3,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '14.) บริเวณที่คุณเป็นผื่นนั้น มีความแห้งกร้านมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/z6j0cJ3/dryness.png',
      answers: [
        {
          text: 'Score 0 (ไม่มีความแห้งกร้าน)',
          value: 0,
        },
        {
          text: 'Score 1 (เล็กน้อย)',
          value: 1,
        },
        {
          text: 'Score 2 (ปานกลาง)',
          value: 2,
        },
        {
          text: 'Score 3 (มาก)',
          value: 3,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '15.) บริเวณที่คุณเป็นผื่นนั้น มีอาการคันมากน้อยระดับไหน?',
      questionImage: 'https://i.ibb.co/mDVMtJc/15.jpg',
      answers: [
        {
          text: '0',
          value: 0,
        },
        {
          text: '1',
          value: 1,
        },
        {
          text: '2',
          value: 2,
        },
        {
          text: '3',
          value: 3,
        },
        {
          text: '4',
          value: 4,
        },
        {
          text: '5',
          value: 5,
        },
        {
          text: '6',
          value: 6,
        },
        {
          text: '7',
          value: 7,
        },
        {
          text: '8',
          value: 8,
        },
        {
          text: '9',
          value: 9,
        },
        {
          text: '10',
          value: 10,
        },
      ],
      checkpoint: '',
      check: false,
    },
    {
      text: '16.) บริเวณที่คุณเป็นผื่นนั้น ส่งผลต่อการนอนหลับของคุณมากน้อยระดับไหน? (เช่น ผื่นมีอาการคันมากจนส่งผลให้นอนไม่หลับ เป็นต้น)',
      questionImage: 'https://i.ibb.co/1bJkhnx/16.jpg',
      answers: [
        {
          text: '0',
          value: 0,
        },
        {
          text: '1',
          value: 1,
        },
        {
          text: '2',
          value: 2,
        },
        {
          text: '3',
          value: 3,
        },
        {
          text: '4',
          value: 4,
        },
        {
          text: '5',
          value: 5,
        },
        {
          text: '6',
          value: 6,
        },
        {
          text: '7',
          value: 7,
        },
        {
          text: '8',
          value: 8,
        },
        {
          text: '9',
          value: 9,
        },
        {
          text: '10',
          value: 10,
        },
      ],
      checkpoint: '',
      check: false,
    },
  ],
}

export default {
  layout: 'skinDisease',
  data() {
    return {
      quiz: quiz,
      questionIndex: 1,
      showg: false,
      ans: [],
      final: 0,
      end: false,
      isLoading: false,
      damage: {},
      userId: '',
    }
  },
  components: {
    Loader: () => import('~/components/Loader.vue'),
  },
  async mounted() {
    this.userId = this.$route.query.userId
    Vue.loadScript('https://static.line-scdn.net/liff/edge/2/sdk.js')
      .then(() => {
        // Script is loaded, do something
        liff
          .init({
            liffId: '1656721598-rbNWDBag',
          })
          .then(() => {
            console.log('PASS')
          })
      })
      .catch((err) => {
        console.log('f', err)
      })
    // this.isLoading = true
    // this.isLoading = false
  },
  methods: {
    async math() {
      let A =
        (this.ans[0] +
          this.ans[1] +
          this.ans[2] +
          this.ans[3] +
          this.ans[4] +
          this.ans[5] +
          this.ans[6] +
          this.ans[7]) /
        5

      let B =
        7 *
        ((this.ans[8] +
          this.ans[9] +
          this.ans[10] +
          this.ans[11] +
          this.ans[12] +
          this.ans[13]) /
          2)

      let C = this.ans[14] + this.ans[15]

      let sum = A + B + C
      this.final = sum.toFixed(2)
      console.log('คำตอบ', this.final)
      console.log('A', A)
      console.log('B', B)
      console.log('C', C)
      await this.calDamgae()
      await this.updateState()
      let res = await this.sendMessage()
      if(res){
        liff.closeWindow()
      }
      else{
        alert("sendMessage err")
      }
      this.$router.push(this.localePath('/questionnaire/submit-answer'))
    },
       calDamgae() {
    if (this.final >= 0 && this.final <= 15) {
     this.damage = {
          damageTH: 'มีความรุนแรงเล็กน้อย',
          damageEN: 'Mild',
        }
      } else if (this.final >= 16 && this.final <= 40) {
    this.damage = {
          damageTH: 'มีความรุนแรงปานกลาง',
          damageEN: 'Moderate',
        }
      } else if (this.final >= 41 && this.final <= 103) {
    this.damage = {
          damageTH: 'มีความรุนแรงมาก',
          damageEN: 'Moderate',
        }
 
      } else {
        this.damage = {
          damageTH: 'ไม่มีระดับความรุนแรง',
          damageEN: 'unkow',
        }
      }
    },
      sendMessage() {
      liff.sendMessages([
        {
          type:'text',
          text:'การทำแบบสอบถามเสร็จสิ้น'
        }
          // {
          //   type: 'text',
          //   text: `คุณมีระดับความรุนแรงของโรคผื่นภูมิแพ้ผิวหนัง (Atopic Dermatitis) อยู่ที่ ${this.final} คะแนน ซึ่งถือว่าอยู่ในระดับที่ ${this.damage.damageTH} (${this.damage.damageEN}) ตามเกณฑ์ของ The Scoring of Atopic Dermatitis (SCORAD) 😄`,
          // }
   
          // ,
          // {
          //   type: 'text',
          //   text:
          //     'คุณต้องการดูผลิตภัณฑ์ที่แนะนำหรือไม่?\n\nกรุณาพิมพ์ "ใช่" เพื่อดูผลิตภัณฑ์แนะนำ',
          // },
        ])
        .then(() => {
          console.log('message sent')
        })
        .catch((err) => {
          console.log('error', err)
        })
    },
        async updateState() {
      console.log(this.damage.damageEN)
      try {
        let body = {
          userId: this.userId,
          state: 'atopic',
          subState: this.damage.damageEN,
          score : this.final,
          subStateTH : this.damage.damageTH
        }
        await this.$axios.post(
          'https://4692-58-10-4-212.ngrok.io/api/updatestate',
          body
        )
      } catch (err) {
        alert(err)
      }
    },
    checkpointbg(value, score) {
      this.quiz.questions[this.questionIndex - 1].checkpoint = value
      this.quiz.questions[this.questionIndex - 1].check = true
      this.ans[this.questionIndex - 1] = score
      if (this.ans.length === this.quiz.questions.length) {
        this.end = true
      }
    },
    prev() {
      if (this.questionIndex > 1) {
        this.questionIndex--
      }
    },
    next() {
      if (
        this.questionIndex >= 1 &&
        this.questionIndex !== this.quiz.questions.length
      ) {
        this.questionIndex++
      }
      window.scrollTo(0, 0)
    },
    perpage(value) {
      this.questionIndex = value + 1
    },
    show() {
      if (this.showg == false) {
        this.showg = true
      }
    },
    close() {
      if (this.showg == true) {
        this.showg = false
      }
    },
    playAgain() {
      this.questionIndex = 0
    },
  },
}
</script>

<style scoped>
.pg-bar {
  position: fixed;
  top: 0;
  padding-top: 1rem;
  height: 40px;
  max-width: 100%;
  z-index: 100;
  background-color: #fff;
}
.skin-content {
  margin-bottom: 70px;
  margin-top: 20px;
}
.bg_green {
  background-color: #10c4cc;
}
.footer {
  position: fixed;
  bottom: 0%;
  max-width: 100%;
  background-color: #10c4cc;
  cursor: pointer;
}

.footerV2 {
  position: fixed;
  bottom: 0%;
  max-width: 100%;
  background-color: #c2cfe0;
  cursor: pointer;
}

.footer:hover {
  background-color: #10b4bd;
}
.footerBtn {
  font-family: 'Prompt-Medium';
  color: #fff;
  font-size: 22px;
  line-height: 30px;
}
.txt_title {
  font-family: 'Prompt-Regular';
  font-size: 20px;
  line-height: 33px;
  color: #333333;
}
.border_bottom_1_f3 {
  border-bottom: 1px solid #dddddd !important;
}
.txt_14 {
  font-size: 14px !important;
}
.txt_20 {
  font-size: 20px !important;
}
.txt_28 {
  font-size: 28px;
}
.line-height-1 {
  line-height: 1 !important;
}
.txt_bold {
  font-family: 'Prompt-Regular';
  color: #333333;
  font-size: 20px;
}
.circle_no_task {
  background: #ffffff;
  color: #000000;
  border-radius: 25px;
  display: inline-block;
  font-weight: bold;
  margin: 7px 3.9px;
  text-align: center;
  border: 2px solid #cccccc !important;
  padding: 10px;
  height: 46px;
  width: 46px;
  font-family: 'Prompt-Regular';
  font-size: 13px;
  line-height: 1.7;
}
.circle_no_task.did {
  background: #109cf1;
  color: #000000;
  border: 2px solid #cccccc !important;
}
.circle_no_task.doing {
  background: #e5e5e5;
  color: #000000;
  border: 2px solid #cccccc !important;
}
.question_txt {
  font-family: 'Prompt-Regular';
  font-style: normal;
  font-size: 20px;
  color: #333333;
}
.answer_txt {
  font-family: 'Prompt-Regular';
  font-style: normal;
  font-size: 15px;
  color: #333333;
}
.card-list-hover:hover {
  background-color: #f3f3f3;
}
.btn_gray_hover {
  border-radius: 5rem;
  border: 1px solid #bdbdbd;
}
.txt_grey_99 {
  color: #999999 !important;
}
.txt_title_btn {
  font-family: 'Prompt-Regular';
  font-size: 16px;
  line-height: 27.5px;
  color: #fff;
}
@media screen and (max-width: 768px) {
  .txt_title {
    font-family: 'Prompt-Regular';
    font-size: 18px;
    line-height: 33px;
    color: #333333;
  }
  .question_txt {
    font-family: 'Prompt-Medium';
    font-style: normal;
    font-size: 20px;
    color: #333333;
  }
  .answer_txt {
    font-family: 'Prompt-Regular';
    font-style: normal;
    font-size: 16px;
    color: #333333;
    line-height: 33px;
  }
  .txt_title_btn {
    font-family: 'Prompt-Regular';
    font-size: 14px;
    line-height: 27.5px;
    color: #fff;
  }
  .txt_bold {
    font-family: 'Prompt-Regular';
    color: #333333;
    font-size: 18px;
  }
}
.ck-content img {
  width: 100%;
  height: 100%;
}
.bg_gray {
  background-color: #f3f3f3;
}
.card-body,
.card-footer {
  padding: 0.825rem;
}
.m_width_120p {
  min-width: 120px;
}
</style>
