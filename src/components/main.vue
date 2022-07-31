<template>
    <div class="container">
        <div class="header">
            <img class="header--logo" src="../assets/logo.png" alt="Logo" />
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
        <div class="factor-input">
            <div class="factor" v-for="input in fixedInputs" :key="input.id">
                <h3 class="factor--label">{{input.name}}</h3>
                <input :id="input.id" class="inputs" type="text" :placeholder="input.name" />
            </div>
        </div>
        <div class="manual-input" v-for="item in generateInput" :key="item.index">
            <h3 class="factor--label">{{ item }}</h3>
            <input class="customized-input" type="text" placeholder="" />
            <button class="remove-button" @click="removeOption(item.id)">x</button>
        </div>
        <button class="submit-btn">تایید فاکتور</button>
    </div>
</template>
<script>
export default {
    name: "TestComponent",
    data() {
        return {
            userInputData: "",
            generateInput: [],
            selectedOptions: [],
            fixedInputs : [
                {name : "نام و نام خانوادگی :" , id : "name"},
                {name : "مدل ماشین :", id :"carmodel"},
                {name : "رنگ خودرو :", id : "color"},
                {name : "پلاک :" , id : "pelak"},
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
                poolish: ["بدنه", "چراغ", "دیتیلینگ داخل", "سرامیک", "سوناکس"],
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
                console.log(this.generateInput)
            }
        },
        removeOption(event) {
            this.generateInput.splice(event, 1);
        }
    }
}
</script>