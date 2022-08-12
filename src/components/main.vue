<template>
    <div v-if="!showFactor" class="container">
        <div class="header">
            <img class="header--logo" src="../assets/splus.png" alt="Logo" />
        </div>
        <div class="main-container">
            <div class="factor-input">
                <div class="factor" v-for="input in fixedInputs" :key="input.id">
                    <h3 class="factor--label">{{ input.name }}</h3>
                    <input :id="input.id" v-model="input.value" class="inputs" type="text" :placeholder="input.name" />
                </div>
                <div>
                    <h3 class="factor--label">توضیحات : </h3>
                    <textarea v-model="tozihat" class="inputs" type="text"></textarea>
                </div>
            </div>

            <div class="main-stats">
                <h2 style="display : inline">نوع همکاری : </h2>
                <select class="main-menu" @change="checkStage">
                    <option :value="hamkari.value" v-for="hamkari in noeHamkari" :key="hamkari.index">{{ hamkari.name }}
                    </option>
                </select>
                <h2>فرم همکاری : </h2>
                <select class="main-menu" @change="showInput">
                    <option v-for="option in selectedOptions" :key="selectedOptions[option]">{{ option }}</option>
                </select>
            </div>



            <div class="manual-input" v-for="item in generateInput" :key="item.index">
                <h3 class="factor--label">{{ item }}</h3>
                <input class="customized-input" type="text" :id="item" placeholder="قیمت را وارد کنید " />
                <button class="remove-button" @click="removeOption(item.id)">حذف</button>
            </div>

            <button @click="displayFactor()" class="submit-btn">
                تایید فاکتور
            </button>
        </div>
    </div>
    <div v-if="showFactor" class="factor-container">
        <div class="factor-header">
            <h5 class="date">تاریخ : {{ persianDate}}</h5>
            <img class="factor-logo" src="../assets/splus.png" alt="splus Logo" />
            
        </div>
        <div class="factor-head-info">
            <h4 v-if="nam" class="header-info">نام و نام خانوادگی : <p class="header-info">{{ nam }}</p>
            </h4>
            <h4 v-if="pelak" class="header-info">شماره پلاک : <p class="header-info">{{ pelak }}</p>
            </h4>
            <h4 v-if="color" class="header-info">رنگ خودرو :<p class="header-info">{{ color }}</p>
            </h4>
            <h4 v-if="carmodel" class="header-info">مدل ماشین : <p class="header-info">{{ carmodel }}</p>
            </h4>
        </div>
        <div v-for="(item, expens) in generateInput" :key="item.id" class="done-items">
            <h4>{{ item }} : {{ finalExpens[expens] }}</h4>
        </div>
        <div class="customer-explain">
            <h4 class="header-info">تمامی خدمات در این مجموعه بدین شرح میباشد  : </h4>
            <p v-if="tozihat" class="header-info">{{ tozihat }}</p>
        </div>
        <div class="customer-payment">
            <h4>مبلغ قابل پرداخت : {{ allCosts }}</h4>
        </div>
        <div class="contact-info">
            <img class="contact-logo" src="../assets/splus.png" alt="splus Logo" />
            <h5 class="address">آدرس : خیابان ولیعصر - کوچه ربیعی - پلاک 24 و 26</h5>
            <h5  class="contact-number">شماره تماس : 0912111111 - 0935111111</h5>
        </div>
    </div>
</template>
<script>
export default {
    name: "HomePage",
    data() {
        return {
            nam: '',
            color: '',
            pelak: '',
            carmodel: '',
            tozihat: '',
            persianDate: '',
            showFactor: false,
            userInputData: "",
            generateInput: [],
            finalExpens: [],
            allCosts: 0,
            selectedOptions: [],
            fixedInputs: [
                { name: "نام و نام خانوادگی :", id: "name", value: '' },
                { name: "مدل ماشین :", id: "carmodel", value: '' },
                { name: "پلاک :", id: "pelak", value: '' },
                { name: "رنگ خودرو :", id: "color", value: '' },

            ],
            noeHamkari: [
                { name: "چرم کوبی ", value: "charmkoobi" },
                { name: "تودوزی", value: "todozi" },
                { name: "ترمیم کلید", value: "tarmimkey" },
                { name: "صفرشویی", value: "sefrShoee" },
                { name: "فایبر گلاس", value: "fiberGlass" },
                { name: "برق کشی ", value: "BarghKeshi" },
                { name: "واکس و پولیش", value: "poolish" },
                { name: "جوش پلاستیک", value: "Joosh" },
                { name: "لیزر", value: "lazer" },
                { name: "مکانیکی", value: "mechanic" }
            ],
            hamkariOptions: {
                charmkoobi: ["فرمان", "داشبورد", "رودری", "کنسول", "ستون"],
                todozi: ["رودری", "فرمان", "داشبورد", "صندلی", "سقف و سرستون", "کف"],
                tarmimkey: ["-", "ترمیم کلید"],
                sefrShoee: ["موتور", "صندوق", "کف", "صندلی", "سقف و ستون", "کابین"],
                fiberGlass: ["جوش", "ترمیم قطعه"],
                BarghKeshi: ["سقف کهکشانی", "تعمیر سیمکشی", "نورپردازی"],
                poolish: ["بدنه", "چراغ", "دیتیلینگ داخل", "سرامیک", "سوناکس", "ترمیم گلگیر"],
                Joosh: [" پلاستیک توری", "پلاستیک abs", "آرگون"],
                lazer: ["چرم", "پلاستیک"],
                mechanic: ["-", "مکانیکی"]
            },
        }
    },
    methods: {
        checkStage(event) {
            let hamkari = event.target.value;
            this.selectedOptions = this.hamkariOptions[hamkari]
        },
        showInput(event) {
            let selectedOption = event.target.value;
            if (selectedOption) {
                this.userInputData = selectedOption
                this.generateInput.push(selectedOption);
            }
        },
        removeOption(event) {
            this.generateInput.splice(event, 1);
        },
        displayFactor() {
            if (!this.showFactor) {
                this.nam = this.fixedInputs[0].value;
                this.carmodel = this.fixedInputs[1].value;
                this.pelak = this.fixedInputs[2].value;
                this.color = this.fixedInputs[3].value
                this.showFactor = true;
                let today = new Date().toLocaleDateString('fa-IR');
                this.persianDate = today
                setTimeout(() => {
                    this.showFactor = false
                }, 300000)
                //Remove a 0 from the time stamp of the time out
            }
            if (this.generateInput) {
                for (let i = 0; i < this.generateInput.length; i++) {
                    let test = document.getElementById(this.generateInput[i])
                    this.finalExpens.push(test.value)
                }
                for (let i = 0; i < this.finalExpens.length; i++) {
                    this.allCosts = this.allCosts + parseInt(this.finalExpens[i])
                }
            }
        }
    }
}
</script>